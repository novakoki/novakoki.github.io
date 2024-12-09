---
title: A Note of Computer Network
date: 2015-08-20
locale: en_US
---

<!-- [TOC] -->

I. Introduction
---------------

### 1. Computer Network and Distributed System

 Example: World Wide Web, Hadoop

---
### 2. Uses

- **Business Application**
	- Resource Sharing *(Client-Server Model)*
	- Communication Medium
	- Electric Commerce
- **Home Application**
	- Connectivity *(Peer-to-Peer)*
	- Person-to-Person communication
		- Instant Messaging (Twitter)
		- Social Network (Facebook)
		- Wiki (Wikipedia)
	- Electric Commerce (B2C, B2B, G2C, C2C, P2P)
	- Entertainment
	- Ubiquitous Computing (RFID, IOT)
- **Mobile Users**
	- Wireless Network
	- Text Messaging (SMS)
	- Mobile Commerce
	- Sensor Network
	- Wearable Computers
- **Social Issues**
	- Network Neutrality
	- DMCA
	- Privacy
	- Spam
	- Secruity
	- Law (Electric Gambling)
	
	---


### 3. Hardware

- Sorted by **Type**
	- Broadcast
	- Point-to-Point
- Sorted by **Scale**
	- Internetworks
	- Wide Area Network
	- Metropolitan Area Network
	- Local Area Network
	- Personal Area Network

---


### 4. Software

* **Layer, Protocol, Interface, Service**
Most networks are organized as a stack of **layers**.
A **protocol** is an agreement between the communicating parties on how communication is to proceed.
The **interface** defines which primitive operations and services the lower layer makes available to the upper one.
A **service** is a set of primitives(operations) that a layer provides to the layer above it.

- **Design Issues**

1. Reliability
	- Error Detection
	- Error Correction
	- **Routing**
2. Evolution
	- Addressing or Naming
	- **Internetworking**
3. Resource Allocation
	- **Multiplexing**
	- **Flow Control**
	- **Congestion**
	- Quality of Service
4. Secure


- **Connection Oriented** VS **Connectionless**
**Realiable communication may not be availble in a given layer;
The delays inherent in providing a reliable service may be unacceptable.**

	1. Connection Oriented
		+ **Byte Stream** VS **Message Stream**
		+ Voice over IP
	2. Connectionless
		+ Datagram
		+ Request-Reply
- Service Primitive

---


### 5. Reference Model



| OSI | TCP/IP | Used in the book |
| --- | --- | --- |
| Application | Application | Application |
| Presentation | - | - |
| Session | - | - |
| Transport | Transport | Transport |
| Network | Network | Network |
| Data Link | Link | Data Link |
| Physical | - | Physical |


### 6. Network Standardization



---


II. The Physical Layer
----------------------


> It defines the electrical, timing and other interfaces by which
>  **bits** are sent as **signals** over **channels**.


### 1. The Theoretical Basis for Data Communication

* Fourier Analysis
* Bandwidth-Limited Signals
	+ **Bandwidth**
	 The width of the frequency range transmitted without being strongly attenuated is called the bandwidth.
	+ **Baseband**
	 Signals that run from 0 up to the maximum frequency are called baseband signals.
	+ **Passband**
	 Signals that are shifted to occupy a higher range of frequencies, as is the case of all wireless transmissions, are called passband signals.
* The Maximum Data Rate of a Channel


	+ Nyquist’s theorem
	$$maximum\ data\ rate=2B\log\_2V\ bits/sec$$
	+ Shannon’s theorem
	$$maximum\ data\ rate=B\log\_2(1+S/N)\ bits/sec$$
	
	
	$$10\log\_{10}S/N=1\ dB$$


### 2. Digital Modulation

* Baseband
	+ NRZ
	+ NRZI
	+ Manchester
	+ Bipolar
	+ 4B/5B
* Passband
	+ ASK
	+ FSK
	+ PSK
	+ Constellation Diagram


### 3. Multiplexing

* FDM
* TDM
* CDMA


### 4. Switching

III. The Data Link Layer
------------------------

### 1. Framing

IV. The Medium Access Control Sublayer
--------------------------------------

V. The Network Layer
--------------------

VI. The Transport Layer
-----------------------

VII. The Application Layer
--------------------------

VIII. Network Security
----------------------
