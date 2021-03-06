---
layout: post
title: ZeroMQ Library for C Language
excerpt: General points to remember while writing programs with ZeroMQ library.
category: networking
comments: false
google_adsense: false
---
One socket may have many outgoing and many incoming connections.

Mostly it's obvious which node should be doing `zmq_bind()` (the server) and which should be doing `zmq_connect()` (the client).

A server node can bind to many endpoints (that is, a combination of protocol and address) and it can do this using a single socket.

The inter-process ipc transport is disconnected, like tcp. We call this disconnected because ZeroMQ's tcp transport doesn't require that the endpoint exists before you connect to it. The inter-thread transport, inproc, is a connected signaling transport. 

A traditional networked application has one process or one thread per remote connection, and that process or thread handles one socket. ZeroMQ lets you collapse this entire structure into a single process and then break it up as necessary for scaling.

ZeroMQ gives your applications a single socket API to work with, no matter what the actual transport (like in-process, inter-process, TCP, or multicast). It automatically reconnects to peers as they come and go. It queues messages at both sender and receiver, as needed. It limits these queues to guard processes against running out of memory. It handles socket errors. It does all I/O in background threads. It uses lock-free techniques for talking between nodes, so there are never locks, waits, semaphores, or deadlocks.

To actually read from multiple sockets all at once, use `zmq_poll()`. An even better way might be to wrap zmq_poll() in a framework that turns it into a nice event-driven reactor.

You will receive all parts of a multipart message, or none at all. There is no way to cancel a partially sent message, except by closing the socket.

One of the problems you will hit as you design larger distributed architectures is discovery. That is, how do pieces know about each other? It's especially difficult if pieces come and go, so we call this the "dynamic discovery problem".  
There are quite a few answers to this, but the very simplest answer is to add an intermediary; that is, a static point in the network to which all other nodes connect. In classic messaging, this is the job of the message broker. ZeroMQ doesn't come with a message broker as such, but it lets us build intermediaries quite easily.  
Adding a pub-sub proxy solves the dynamic discovery problem in our example. We set the proxy in the "middle" of the network. The proxy opens an XSUB socket, an XPUB socket, and binds each to well-known IP addresses and ports. Then, all other processes connect to the proxy, instead of to each other. It becomes trivial to add more subscribers or publishers.

ZeroMQ supports buiding bridges between networks. A bridge is a small application that speaks one protocol at one socket, and converts to/from a second protocol at another socket. A protocol interpreter, if you like. A common bridging problem in ZeroMQ is to bridge two transports or networks. We can use this model for example to connect a multicast network (pgm transport) to a tcp publisher.

To make utterly perfect multithread programs (and I mean that literally), we don't need mutexes, locks, or any other form of inter-thread communication except messages sent across ZeroMQ sockets.

Do not use or close sockets except in the thread that created them.

PUSH and DEALER will distribute messages to all available receivers. But PUB doesn't distribute.

We use PAIR sockets for signalling between threads.

When you want to coordinate a set of nodes on a network, PAIR sockets won't work well any more. This is one of the few areas where the strategies for threads and nodes are different. Principally, nodes come and go whereas threads are usually static. PAIR sockets do not automatically reconnect if the remote node goes away and comes back.

Subscriptions do a prefix match. That is, they look for "all messages starting with XYZ". The obvious question is: how to delimit keys from data so that the prefix match doesn't accidentally match data. The best answer is to use an envelope because the match won't cross a frame boundary.

The ROUTER socket invents a random identity for each connection with which it works. If there are three REQ sockets connected to a ROUTER socket, it will invent three random identities, one for each REQ socket. ROUTER will route messages asynchronously to any peer connected to it, if you prefix the identity as the first frame of the message.

DEALER is like an asynchronous REQ socket, and ROUTER is like an asynchronous REP socket. Where we use a REQ socket, we can use a DEALER; we just have to read and write the envelope ourselves. Where we use a REP socket, we can stick a ROUTER; we just need to manage the identities ourselves.  
Think of REQ and DEALER sockets as "clients" and REP and ROUTER sockets as "servers". Mostly, you'll want to bind REP and ROUTER sockets, and connect REQ and DEALER sockets to them.

The REQ client must initiate the message flow. A REP server cannot talk to a REQ client that hasn't first sent it a request. Technically, it's not even possible.

When we use a DEALER to talk to a REP socket, we must accurately emulate the envelope that the REQ socket would have sent, or the REP socket will discard the message as invalid. So, to send a message, we:  
* Send an empty message frame with the MORE flag set; then
* Send the message body.  

And when we receive a message, we:  
* Receive the first frame and if it's not empty, discard the whole message;
* Receive the next frame and pass that to the application.

Router to router combination sounds perfect for N-to-N connections, but it's the most difficult combination to use. You should avoid it until you are well advanced with ZeroMQ (Freelance pattern). An alternative DEALER to ROUTER design for peer-to-peer work is explained later.

The common thread in this valid versus invalid breakdown is that a ZeroMQ socket connection is always biased towards one peer that binds to an endpoint, and another that connects to that. Further, that which side binds and which side connects is not arbitrary, but follows natural patterns. The side which we expect to "be there" binds: it'll be a server, a broker, a publisher, a collector. The side that "comes and goes" connects: it'll be clients and workers. Remembering this will help you design better ZeroMQ architectures.

The identity concept in ZeroMQ refers specifically to ROUTER sockets and how they identify the connections they have to other sockets. More broadly, identities are used as addresses in the reply envelope.

You can force the ROUTER socket to use a logical address in place of its identity. The zmq_setsockopt reference page calls this setting the socket identity.

Freelance pattern is a brokerless peer to peer distributed architecture.

Discovery for proximity networks, that is, a set of nodes that find themselves close to each other. We can define "close to each other" as being "on the same network segment". It's not going to be true in all cases but it's true enough to be a useful place to start.
