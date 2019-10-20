---
title: "Anatomy of HTTP Request"
date: "2019-10-20T11:46:37.121Z"
template: "post"
draft: false
slug: "/posts/anatomy-of-http-request/"
category: "Docker"
tags:
  - "Programming"
  - "Javscript"
  - "Deployment"
  - "Request"
  - "Internet"
  - "OSI"
description: "In this post, we will be exploring how does a request moves across the network and reach the server. This communication of hosts happens because of the networking models and we are going to discuss one of the networking models in this."
---
![Networking Model](/media/post13-image1.png)

We know that one computer can transmit data to another computer, another computer receives the data and respond with data. Have we ever asked ourselves how does this happen, what the principles, models behind it? We know the high-level stuff, have you ever tried to find out what happens inside. Well, we are about to find out.

In this post, we are going to find out what the layers a request needs to pass through and how does it pas

There is two networking model:
- **OSI Model**
- **TCP / IP Model**

**OSI model** no longer used these days but we are going to discuss it in this post because everything has started from it and we want to make our base strong.

**TCP / IP model** is currently used networking world these days.
We use this everywhere i.e to talk to another computer, to the printer, to switch, to the router, etc.. Maybe I will write another article about this.

Let's start with our deep discussion of the OSI model.

### What is the OSI Model?
OSI stands for Open systems interconnection. It is 7 layered architecture where each layer contributes to transmitting the data from one person to another person.

OSI model divides the whole to task into seven smaller and manageable tasks. Each layer assigned to a particular task. Each layer is self-contained so that the task assigned to each layer can be performed independently.

OSI model layers:
- Application
- Presentation
- Session
- Trasport
- Network
- Data Link
- Physical

Let's start exploring OSI layers one by one.

#### Application Layer
An application layer that serves as a window for end-users and application processes to access the network service. This layer has many responsibilities, including error handling and recovery, data flow over a network.

This layer provides services like this:
- Simple Mail Transfer Protocol
- Web surfing
- File Transfer
- Web Chat
- Email Client
- Virtual Terminal etc.

More than 15 protocols are used in the application layer including SMTP, FTP (File Transfer Protocol), Telnet, etc.

#### Presentation Layer
The Presentation layer mainly concerned with the syntax and semantics of the information exchanged between the two systems. It acts as a data translator for the network. This layer is part of the operating system which converts data to one format to another format.

This layer has three functions:
- Translation
- Encryption / Decryption
- Compression

***Translation***: different computers system can use different encoding methods, so the presentation layer handles the interoperability between the different encoding methods. It converts the data send-dependent format to a common format and changes the common format into a receiver-dependent format at the receiving end.

***Encryption / Decryption***: Encryption is needed to maintain the privacy. It converts the sender-transmitted information to another form and sends the result over the network.

***Compression***: Compression is a process of compressing the data. It reduces the number of bits to be transmitted.

#### Session Layer
This layer is responsible for the establishment of connection, maintenance of sessions, authentication and also ensure security.

This layer has three functions:
- **Session establishment, maintenance and termination**:
This layer allows two processes to establish, use and terminate a connection.

- **Synchronization**:
This layer allows a process to add checkpoints into the data to ensure it gets transmitted to the receiving end without any loss.
- **Dialog Controller**:
The session layer allows two systems to start communication with each other in half-duplex or full-duplex.

#### Transport Layer
This layer provides services to the application layer and takes services from the network layer that we will discuss next. The data in the transport layer referred to as Segments. This is responsible for end to end delivery of a complete message. It also provides acknowledgment of successful data transmission and re-transmit the data if any error found.

***At sender's end***:
The transport layer receives the formatted data from the upper layers, performs Segmentation and also implements **Flow and Error control** to ensure proper data transmission. It also adds Source and Destination port number into its header and forwards it to the Network layer.

***At receiver's end***:
The transport layer reads the port number from its header and forwards the data to the respective application. It also performs the sequencing and reassembling of the segmented data.
- ***Segmentation and Reassembly***: This layer accepts the message from the session layer, break the message down to the smaller units. Each of the segments produced has its header. This layer at the destination reassembles the message.
- ***Service Point Addressing***: To deliver the data to the correct process. Thus transport layer includes the type of header called service point header or port number so that it can deliver the data to the correct process.

**This layer can have two types of connection**:
##### Connection-Oriented Service
It is a three-phase process which includes
- Connection establishment
- Data Transfer
- Termination

In this type of transmission, the receiver sends the acknowledgment, back to the sender that the packet has successfully received at the destination. Because of this, this transmission is reliable and secure.

##### Connectionless service
It only has one phase which is Data Transfer. In this type of transmission, the receiver does not acknowledge the sender it has received the data. This approach allows for much fast communication between the devices. However, it is not reliable and secure as compare to the Connection-oriented service.

#### Network Layer
The network layer works for transmissions of data from one host to the other located host in different networks. It also takes care of the packet routing i.e. selection for the shortest route in the network, from the number of routes available.

This layer has two main functions:
- **Routing**: The network layer protocols determine which route suitable for source to destination. This function of the network layer is known as routing.
- **Logical Routing**: To identify each device on the internetwork uniquely, the network layer defines an addressing scheme. The sender and receiver's IP address is placed on a header by the network layer.

#### Data-Link Layer
The layer is responsible for the node to node delivery of the message. The main function of this layer to make sure that data transfer error-free from one to another node, over the physical layer.

Data-Link layer again divides into two layers:
- Logical Link Layer
- Media Access Control

The packet received from the network layer is again divided into the frames depending on the frame size of NIC (Network Interface Card).

DDL also adds the sender and receiver's MAC address in the header. It obtains the receiver's MAC by making an ARP (Address Resolution Protocol) request on to the wired network "What has that IP address" and receiver reply with its MAC address.

There are the functions of the network layer:
- **Framing**: It provides the function to transfer the set of bits that are meaningful to the receiver. This accomplishes by adding a pattern to the start and end of the frame.
- **Physical Addressing**: After creating the frames, the data link layer adds physical addresses (MAC) of the sender and receiver in the header of each frame.
- **Error control**: It is a mechanism of error control in which it detects and retransmit the corrupted and lost frames.
- **Flow Control**: The data must be constant on both sides else the data may get corrupted thus, flow control coordinates to the receiver get acknowledgment how much size of data can be received at the receiver end.
- **Access Control**: When the single communication channel is shared by the multiple devices, the MAC sub-layer of the data link layer determines which device has control over the communication channel at the given time.

***NOTE***: You must be wondering why we are again using the flow control because it has already been used in the transport layer. Providing error control at the data-link is an optimization, never a requirement.
Packets have a maximum size of 65535 bytes. Whereas the maximum size of frames (take the example of ethernet) is 1500 bytes. The network layer doesn't know these parameters. It might send the larger packets which again gets break down to 10 frames, of which 2 are lost on the average. It would take very long for the package to get through. Instead, if individual frames are acknowledged and re-transmitted, the errors get corrected directly and more quickly.

#### Physical Layer
It is the lowest layer of the OSI model. This layer is responsible for the actual physical connection between the devices. The physical layer contains information in the form of bits. When sending data from this layer it converts 1's and 0's to signal form and transmit it to the receiver and on the receiver's end, it gets converted back to the 1's and 0's form.

Physical layers functions:
- **Bit synchronization**: The physical layer provides the synchronization of the bits by providing a clock. This clock control both the sender and receiver thus providing synchronization at a bit level.
- **Bit Rate Control**: This layer also controls how many bits should be transferred per second.
- **Physical topologies**: The physical layer specifies how the different, devices/nodes are arranged in a network i.e. bus, star or mesh topology.
- **Transmission mode**: The physical layer also defines which way should the data flow between the two connected devices. The various mode is simplex, half-duplex and full-duplex. 

These were the seven layers of the OSI model and their functions inside the OSI model. The TCP / IP just the contraction of these layers.

I hope you must have enjoyed it. I write articles like this every week, you can check those out here [mountainfirefly.dev](https://mountainfirefly.dev).

[I tweet about JavaScript and code tips here.](https://twitter.com/mountainfirefly)
