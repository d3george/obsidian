---
title: "WebSockets Explained: 5 Things Most Developers Never Learn"
source: "https://www.youtube.com/watch?v=BKonNa7XPdg"
cover: "https://www.youtube.com/img/desktop/yt_1200.png"
author:
  - "ByteMonk"
description: "Most developers use WebSockets every day for chat, live dashboards, and multiplayer games. Almost none of them know why WebSockets are designed the way they are.In this video we break down 5 design"
created: 2026-05-30
---
```vid
https://www.youtube.com/watch?v=BKonNa7XPdg
Title: WebSockets Explained: 5 Things Most Developers Never Learn
Author: ByteMonk
Thumbnail: https://i.ytimg.com/vi/BKonNa7XPdg/mqdefault.jpg
AuthorUrl: https://www.youtube.com/@ByteMonk
```

![](https://www.youtube.com/watch?v=BKonNa7XPdg)

Most developers use WebSockets every day for chat, live dashboards, and multiplayer games. Almost none of them know why WebSockets are designed the way they are.  
  
In this video we break down 5 design decisions hidden inside the WebSocket protocol. By the end you will understand the handshake, the masking, the magic key, and the HTTP/2 tradeoff well enough to hold your own in any system design interview.  
  
What you will learn:  
\- Why the WebSocket handshake starts life as a normal HTTP request  
\- How the Sec-WebSocket-Key and that magic GUID defeat proxy cache attacks  
\- The clever trick where the handshake secretly warms up your TCP connection  
\- Why every frame a client sends has to be masked, and why server frames don't  
\- Why WebSockets ride on ports 80 and 443  
\- When HTTP/2 with server-sent events actually beats WebSockets  
  
Chapters:  
00:00 The WebSocket questions most devs can't answer  
00:40 Real-time before WebSockets: the HTTP problem  
01:06 Polling vs long polling  
01:31 What WebSockets changed: full-duplex connections  
01:58 The HTTP Upgrade handshake (101 Switching Protocols)  
02:36 #1 The magic key that blocks proxy cache attacks  
04:09 #2 The handshake secretly warms up TCP slow start  
05:18 #3 Why clients must mask every frame  
06:23 #4 Why WebSockets use ports 80 and 443  
07:07 #5 WebSockets vs HTTP/2 multiplexing  
07:42 Recap: the 5 takeaways  
08:22 Final thoughts  
  
Want to go deeper on system design? Check out my System Design course: https://academy.bytemonk.io/systemdesign  
  
Resources:  
\- ByteMonk Blog: https://blog.bytemonk.io/  
\- Cybersecurity Course: https://academy.bytemonk.io/cybersec  
\- System Design Course: https://academy.bytemonk.io/systemdesign  
\- LinkedIn: https://www.linkedin.com/in/bytemonk/  
\- Github: https://github.com/bytemonk-academy  
  
  
Playlists:  
https://www.youtube.com/playlist?list=PLJq-63ZRPdBt423WbyAD1YZO0Ljo1pzvY  
https://www.youtube.com/playlist?list=PLJq-63ZRPdBssWTtcUlbngD\_O5HaxXu6k  
https://www.youtube.com/playlist?list=PLJq-63ZRPdBu38EjXRXzyPat3sYMHbIWU  
https://www.youtube.com/playlist?list=PLJq-63ZRPdBuo5zjv9bPNLIks4tfd0Pui  
https://www.youtube.com/playlist?list=PLJq-63ZRPdBsPWE24vdpmgeRFMRQyjvvj  
https://www.youtube.com/playlist?list=PLJq-63ZRPdBslxJd-ZT12BNBDqGZgFo58  
  
#WebSockets #SystemDesign #BackendEngineering #DistributedSystems #SoftwareEngineering

## Transcript

### The WebSocket questions most devs can't answer

**0:00** · You know what's funny? Every day, millions of developers use websockets for chat apps, live dashboards, multiplayer games. But most of them have no idea why websockets work the way they do. Like, why does the handshake involve this weird magic string? Why does every message from your browser have to be masked with exor? And here is one that will blow your mind. Websockets actually piggyback on HTTP in a way that tricks TCP into performing better. Today I'm going to show you five things about websockets that most developers never learn.

**0:30** · And by the end, you will understand this protocol at a level that will make you dangerous in system design interviews. Let's start from the very beginning.

### Real-time before WebSockets: the HTTP problem

**0:44** · So imagine it's 2005. You're building a chat application. How do you get messages from the server to the browser?

**0:51** · Well, HTTP is request response. The browser asks, the server answers. That's it. But what if the server has new information like your friend just sent you a message? The server can't just call you. HTTP doesn't work that way.

### Polling vs long polling

**1:07** · The server has to wait for you to ask.

**1:10** · So developers came up with hacks.

**1:12** · Polling, ask the server every few seconds. Got anything? Got anything? Got anything? And it works, but wasteful.

**1:21** · You're burning bandwidth and server resources. even when there is nothing happening. Long polling slightly better.

**1:28** · You ask the server and the server holds the connection open until it has something to say. Then you immediately reconnect and wait again. But you are still tearing down and rebuilding HTTP connections constantly.

### What WebSockets changed: full-duplex connections

**1:40** · And then comes websockets. What if you could just keep the connection open and both sides can send messages whenever they want? That's the core idea. One TCP connection, full duplex communication, no more request response dance. But here's the thing, the way websockets establish this connection is where it gets really clever. Websockets don't create their own special connection from scratch. They start as a regular HTTP request.

### The HTTP Upgrade handshake (101 Switching Protocols)

**2:07** · See that it's just a get request, but it says, "Hey server, I'd like to upgrade this connection to websocket."

**2:16** · The server says 101 switching protocols, which basically means, okay, from this point forward, you're not speaking HTTP anymore. You're speaking websocket. And that TCP connection, it stays open. No new handshake needed. Now, this seems simple enough, but there are some really clever design decisions hidden here.

### 1 The magic key that blocks proxy cache attacks

**2:38** · Let's go through them. Let's talk about this SEC websocket key thing. The client sends a random string and the server sends back a different string. Why?

**2:48** · Here's the problem the designers were solving on the internet. There are these things called transparent proxies. They set between you and the server and the cache responses to make things faster.

**2:59** · You might not even know they exist. Your ISP might have one. Your corporate network definitely has one. Now imagine this scenario. You make a websocket request to chat.bitemunk.io.

**3:11** · The proxy sees, oh, a get request to / chat. Let me catch that 101 response.

**3:18** · Later, some other user asks for the same URL. The proxy serves the cache 101 response. That user's browser think it has a websocket connection, but it doesn't. It's talking to the proxy, not the server. This is bad. Really bad. It could leak data between users or worse, it could be exploited for attacks. So, here's the fix. The client generates a random key for every connection. The server takes that key, concatenates it with this magic GU ID, then hashes it with SHA one and sends back the result.

**3:52** · Now, if a proxy caches this response and serves it to someone else, the accept value won't match what the client expects. The connection fails. Proxy caching defeated. By the way, that GU ID, it's not cryptographically special.

**4:07** · It's just a random string the spec authors chose. It's the process that matters, not the specific value. Okay, this one is subtle but beautiful. You know how TCP has this thing called slow start. When you open a new TCP connection, it doesn't just blast data at full speed. It starts cautiously, sending a little bit, waiting for acknowledgement, and then gradually increasing. This is smart for congestion control, but it means new connections are slow at first. You have to warm up the pipe. Now, here's the clever part about websockets.

### 2 The handshake secretly warms up TCP slow start

**4:37** · The HTTP upgrade request here isn't just protocol formality. It's doing work. Those byes flowing back and forth, they're growing your TCP congestion window. By the time you are sending actual websockets frame, you have already spent a couple of round trips. The connection has warmed up.

**4:56** · You're not starting from zero. If websockets had invented their own protocol on a custom port with a custom handshake, you would pay the slow start penalty twice. once for TCP, once for the websocket handshake. By reusing HTTP, you emotize the cost. And this is why I called the HTTP request the bait.

**5:15** · It looks like protocol overhead, but it's secretly warming up your TCP pipe.

### 3 Why clients must mask every frame

**5:21** · Here is something weird. Every websocket message sent from the client to the server has to be masked. Exort with random four byte key. But messages from the server to client, no masking required. Why this asymmetry?

**5:35** · Now imagine a malicious web page. It opens a websocket connection to a vulnerable server, but it carefully crafts its websocket frames to look exactly like a valid HTTP response. If there's a transparent proxy in the middle that doesn't understand websockets, it might see those bites and think, "Oh, this looks like an HTTP response for jQuery.js. Let me cache it." Now, the next user who requests jQuery from that proxy gets the attacker's malicious code instead. This is called cash poisoning.

**6:04** · The fix force clients to mask all outgoing data with a random key. Now the attacker can't predict what will actually hit the wire.

**6:14** · They can't craft a payload that looks like a valid HTTP response. Server toclient doesn't need masking because servers are trusted. They're not going to poison their own caches.

### 4 Why WebSockets use ports 80 and 443

**6:25** · This is a quick one, but important. Web soockets run on ports 80 and 443, the same ports as HTTP and HTTPS.

**6:34** · This isn't an accident. Corporate firewalls typically block everything except web traffic. If websockets use port 8080 or some custom port, they block it in half the environments where people actually need them. And remember, the connection starts as HTTP. So even deep packet inspection firewalls that look at the actual content see a normal HTTP request. By the time it upgrades, the connection is established. This is pragmatic protocol design. Websockets work in the real world because they disguise themselves as HTTP until they are safely through the door.

**7:06** · And here's the last one, and this is where websockets show their age. Let's say your app has three real-time features: chat, notifications, and livestock prices. With websockets, you might open three separate connections. Each one is a separate TCP connection, separate handshakes, separate slow start, separate memory on the server. Now look at HTTP2.

### 5 WebSockets vs HTTP/2 multiplexing

**7:32** · HTTP2 has multiplexing built-in. One TCP connection can carry multiple independent streams. Server can push data wherever it wants using servers and events or HTTP2 push. Websockets still win when you need true birectional communication. gaming, collaborative editing, anything where both client and servers send unpredictably.

### Recap: the 5 takeaways

**7:54** · But for server push updates to client, HTTP2 with servers and events is often simpler and more efficient. So there you have it. Five things most developers never learn about websockets. The magic Q8 prevents proxy caching attacks. HTTP upgrade is secretly warming up TCP.

**8:11** · Client masking prevents cache poisoning.

**8:13** · Port 8443 are chosen for firewall traversal. and no built-in multiplexing.

**8:18** · HTTP2 does it better. Web sockets are one of those technologies that seem simple on the surface, but when you dig into the design decisions, you find really elegant solutions to real world problems. And if you want to go deeper on system design topics like this, check out my system design course link in the description. And I'll see you in the next one.




