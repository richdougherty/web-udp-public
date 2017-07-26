# Web UDP

This repo is to **express public need** for potential **Web UDP** technology to enable **server-client low-latency communication**.  
And **shape the requirements** from existing needs and potential future applications.  
To motivate W3C Members and browser vendors to start discussion and propose spec as a solution.

PR's and discussions in [Issues](https://github.com/Maksims/web-udp-public/issues) are welcome.  
Spread the word, [RT's](https://twitter.com/mrmaxm/status/890256659607584768) are welcome.

## Table of Contents

- [Overview](#overview)
- [Problem](#problem)
- [Possible Solutions](#possible-solutions)
  - [1. WebSockets UDP Extension](#1-websockets-udp-extension)
  - [2. Web UDP - new simple API](#2-web-udp---new-simple-api)
  - [3. WebRTC 2.0 - simplified](#3-webrtc-20---simplified)
- [Requirements](#requirements)
- [Security](#security)
- [Public discussions and demand](#public-discussions-and-demand)
- [Potential first adapters](#potential-first-adapters)

## Overview

Web evolved over past years rapidly, improving networking capabilities using such technologies as: **WebSockets, WebRTC, HTTP/2, Server-Sent Events, QUIC**.

Those technologies have own area of application and enabled web platform to have:
1. Voice and video communication between browsers (WebRTC)
2. Peer-to-peer data exchange (WebRTC)
3. Real-time server-client communication (WebSockets)
4. Faster website delivery (HTTP/2)
5. Faster connection and transfer of data (QUIC)
6. Efficient updating website content in real-time (Server-Sent Events)

*and many more rich and powerful experiences*

In recent years another medium have born: **HTML5 Games**. Adobe Flash is [going away](https://blogs.adobe.com/conversations/2017/07/adobe-flash-update.html), and HTML5 games industry is steadily growing.

Another new trend has born: IO Games - one of the most accessible multiplayer games out there. Some of popular examples: [agar.io](http://agar.io/), [slither.io](http://slither.io/) and [many more](http://iogames.space/). They all rely on currently the only available technology for real-time server-client communication - WebSockets.

But this limits network capabilities as **TCP for games has limits**.  
It's worth mentioning that it's **not only about the games** - there are plenty use cases for Web UDP.

## Problem

**Latency** and **congestion**. TCP has reliable and ordered packet delivery and is connection based protocol.  
The need of UDP over TCP is not under question as it is widely discussed in many articles about benefits of UDP over TCP in different cases. Here we trying just to summarize it.

**Reliability** - this is not always desired by application logic, outdated information might not be relevant anymore and can be ignored. TCP will ensure redelivery of packets, which leads to congestion of packets delivery leading to latency spikes. UDP does not guarantee reliability out of the box, thus avoids congestion problem which can be used as benefits to ensure data is delivered ASAP.

**Ordered Delivery** - TCP uses sequencing number and acknowledgments to guarantee ordered *read* of packets, and if any packets are not delivered through sequence it will redeliver them. This leads to blocking read, which leads to latency spikes and gets worse if network environment has higher packet loss.

**Connection** - is actually a good element for most developers, especially from security and web platform point of view.

## Possible Solutions

Protocol that does not guarantee reliability and ordered delivery out of the box, allowing developer to implement any of the techniques on top of it if required. This provides stable delivery timing and low latency out of the box.

### 1. WebSockets UDP Extension

One of the option to solve this, is by adding new extension to existing websocket protocol. Which would implement extra functionality to establish UDP packets exchange. This would benefit from existing security of WebSockets, provide handshake mechanism and allow developers to follow progressive approach where they can fall-back to TCP logic if UDP extension is not supported by either side. [QUIC](https://www.chromium.org/quic) - is potential transport implementation for such extension.

### 2. Web UDP - new simple API

Another approach would be by developing completely new API, very similar to WebSockets that would address unique requirements of UDP networking logic, as well as potentially provide extra features, e.g. optional arguments for sending packets with reliability or ordered delivery within "channels". This is something developers can implement them self with different specifics on top of raw UDP.

### 3. WebRTC 2.0 - simplified

Current state of WebRTC is very complex. Due to this is not well adopted and requires a lot developer resources. Simply speaking, it is not solo-web-dev friendly like WebSockets are today. Potential option of extending the spec to modularize and simplify the requirements that would allow use of some parts of WebRTC making it much more accessible for server-client scenarios.

## Requirements

1. [**Security**](#security)
2. **Connection based** - to prevent UDP probing as well as make it simpler to use.
3. **Server-client** - p2p is already solved by WebRTC, and due to nature of security p2p is not reliable for applications where decisions can't be trusted to clients. This affects monetization and application logic, where decisions should be made by authoritative server and not clients.
4. **Simple to use** - WebSockets success is very much because of its simplicity to implement server-side protocol, data framing and how simple it is to use in browser.
5. **Minimum header overhead** - to minimize traffic.
6. **Minimum opinion or tech requirements** - WebRTC suffers from complexity and requirements when WebSockets are minimal in that terms enabling its adaption by wide variety of platforms and web developers.

## Security

Low-level access to UDP protocol will lead to many security issues:
1. DDoS attacks
2. Ports scanning
3. Not using HTTP for handshake
4. Lack of encryption

API should address those security concerns, without overcomplicating the API for front-end as well as implementation for back-end.  
That's why [UDPSocket](https://www.w3.org/TR/tcp-udp-sockets/) is not an option.

## Public discussions and demand

- [Why can't I send UDP packets from a browser?](https://new.gafferongames.com/post/why_cant_i_send_udp_packets_from_a_browser/) - detailed analysis why on UDP in browsers and proposed alternative solution.
- [Hacker News: WebRTC: the future of web games (getkey.eu)](https://news.ycombinator.com/item?id=13264952) - discussion why WebRTC is not suitable alternative.
- [A comprehensive dive into WebRTC for client-server web games](http://blog.brkho.com/2017/03/15/dive-into-client-server-web-games-webrtc/) - from article it is apparent how complex WebRTC is, and how hard to implement. Due it's complexity [it is slow on adoption](http://caniuse.com/#feat=rtcpeerconnection) by major browser vendors. But networking benefits of UDP over TCP is clear from article.

## Potential first adopters

- http://agar.io/
- http://slither.io/
- Real-time data visualization (where timing is critical)
- Various HTML5 multiplayer games (e.g. http://iogames.space/)
- Facebook Instant Games
- [WebTorrent](https://webtorrent.io) (to enable a browser [DHT](https://en.wikipedia.org/wiki/Distributed_hash_table) for peer discovery)
- *Please PR your case*
