---
title: "Stop Copying CORS Headers - CORS Explained"
source: "https://www.youtube.com/watch?v=cGg7aRcIm8o"
cover: "https://i.ytimg.com/vi/cGg7aRcIm8o/maxresdefault.jpg"
author:
  - "LearnThatStack"
description: "This video covers the security reasoning behind CORS, what \"origin\" precisely means, the two-level protection model, how the preflight conversation works ste..."
created: 2026-04-17
---
```vid
https://www.youtube.com/watch?v=cGg7aRcIm8o
Title: Stop Copying CORS Headers - CORS Explained
Author: LearnThatStack
Thumbnail: https://i.ytimg.com/vi/cGg7aRcIm8o/mqdefault.jpg
AuthorUrl: https://www.youtube.com/@LearnThatStack
```

![](https://www.youtube.com/watch?v=cGg7aRcIm8o)

## Transcript

**0:00** · You've seen this error. The move, Google it, find the answer, paste some headers in, see if it goes away. Or the newer move, ask the AI, copy the four headers it confidently hands you, paste them in.

**0:15** · Maybe it worked. Maybe you added four headers and removed two, and now you're afraid to touch it in case it breaks again. But what's actually going on here? In most cases, the server got your request. It responded. the data was ready. Your browser is the one blocking you from reading it. Your browser, the one you use every day, doing this on purpose. And there's more.

**0:37** · For many modern API calls, your browser is sending a completely separate HTTP request before your actual one, one you never wrote that doesn't appear anywhere in your code. If your server doesn't handle it, your real request never goes out. That's what this video covers and hopefully help you actually diagnose COS issues instead of pasting headers until something changes.

**1:04** · To understand COS, you need to understand the attack it was designed to prevent. The attack is called cross-sight request forgery, CSRF.

**1:14** · You're logged into your bank session cookie in the browser. Valid. You open another tab. Could be anything. Doesn't have to be obviously evil. Just a website.

**1:25** · That page quietly runs this. That flag credentials include tells the browser to attach cookies to the request. Evil site can't read your bank's cookie. But when it makes a request to your bank.com, the browser automatically attaches your bank.com's cookies to it. That's just how cookies work. They travel with requests to their domain regardless of where the request came from. The bank receives what looks like an authenticated request from you. transfer goes through. You didn't click anything.

**1:58** · Modern browsers like Chrome and Edge have made this specific attack harder, but plenty of older systems are still exposed. The threat model still holds.

**2:07** · So, browsers established a rule.

**2:13** · For many cross origin requests, the browser protects the response, not the request itself. But for more sensitive requests, there's a second layer that stops the request from leaving the browser. We'll cover that later. Coors isn't a separate security system. It's the controlled way to relax that rule when the server says it's safe to do so.

**2:34** · The server is saying, "I know this origin. I trust it. Let the response through." Coors didn't create the restriction. It created the exception.

**2:43** · The attacker cannot fake the answer. The attacker's site can fire cross origin requests, but it cannot forge the server's response headers. The browser goes to the real server to ask. Only the real server can reply. As long as the server is correctly configured, the attacker is locked out of both sides of that conversation. The server declares what it allows through response headers.

**3:06** · The browser enforces that on behalf of the user. Your code works within what those two agreed. Everything else, the pre-flight, the headers, the mistakes is how those three interact.

**3:18** · Most developers think origin means domain. It doesn't. And origin is three things: protocol, host name, and port.

**3:28** · All three. The port is usually implied.

**3:31** · HTTPS uses 443. HTTP uses 80. But the moment you use a non-default port, it becomes part of the origin. Change any one of them completely different origin.

**3:43** · The path is irrelevant. Same protocol, host and port, same origin. Different protocol, different origin.

**3:52** · Different subdomain, different origin.

**3:54** · They share a domain name. The browser does not care about family resemblance.

**3:59** · Different port, different origin.

**4:03** · This is why localhost 3000 and localhost 8000 are different origins. Even on your own computer, the browser has no idea they're both yours. It applies the same rules it would between two completely unrelated websites. Which is why your first cos error happens in local development. Front end on 3000, backend on 8,000. Your own machine, different origins.

**4:29** · A developer hits this and for a brief moment genuinely questions their sanity.

**4:35** · You make the exact call in Postman.

**4:37** · Works. Same call in your browser. Coors error. You haven't changed a single thing. Coors is a browser feature. It does not exist at the network level.

**4:49** · Postman's desktop app makes HTTP calls directly to the server. No browser, no concept of an origin, no user session to hijack, no idea who you're logged into or what other tabs you have open. Same as curl, same as wget. They're just programs talking to servers. Chorus is not their problem. They are doing fine.

**5:12** · The same origin policy only exists in a browser because that's the only environment where untrusted code from the internet runs alongside your authenticated sessions. The attack needs three things.

**5:26** · Postman satisfies none of them. There is genuinely nothing to protect against.

**5:32** · The browser isn't being paranoid. It's being precise.

**5:36** · When debugging, Postman tells you whether the endpoint works. It tells you nothing about cores. Those are separate questions answered by separate tools.

**5:46** · For simple requests, the server already responded. the browser intercepted it before your code could see it. That's what blocked means. Not that the server refused, but that the browser stopped your JavaScript from reading the response.

**6:02** · For requests that trigger a pre-flight, which we'll cover next, the server might not have received anything at all. The browser stopped it before it left. For certain requests, your browser is sending a completely separate HTTP request before your actual one. you never wrote it. It doesn't appear in your code. It determines whether your real request goes at all. These are called pre-flights. Your browser has been doing this every time you send JSON to a cross origin API.

**6:35** · The browser puts every cross origin request into two categories. Simple requests go straight through.

**6:42** · Pre-flighted requests need permission first. A request stays simple when the method is get, head or post. The headers are from a small standard set and if content type is used, it must be one of these three values. Application/json is not on this list. All three must hold.

**7:03** · The moment you violate any one, even just adding an authorization header to a get request, the browser triggers a pre-flight. These look oddly specific.

**7:13** · They're not arbitrary. These are exactly the kinds of requests HTML forms could always make since the early web before any of this existed. A form posting cross origin was always possible. The browser doesn't add friction here because that attack surface already existed and server developers were always expected to handle it themselves.

**7:37** · Simple request browser sends it. Server responds with the right course header.

**7:41** · JavaScript reads the response. One round trip. For simple requests, the request reaches the server regardless. If the course headers are wrong, the browser blocks your JavaScript from reading the response. For pre-flighted requests, the browser checks first. If the pre-flight fails, your request never leaves. Two different levels of protection depending on what the request can do.

**8:07** · What triggers a pre-flight? any of these content type/lication/json and authorization header put, delete, or patch. The moment you use any of them, the browser quietly inserts a permissions check before your request goes anywhere. It sends an options request, the HTTP method specifically designed for asking a server what it will accept. The browser isn't inventing something new. options has been in the HTTP spec since 1999.

**8:40** · The browser is asking, "Are you expecting this kind of request?" Because I'm about to send it.

**8:46** · The pre-flight declares the origin, the method it plans to use, and the headers it wants to send. The server responds with what it permits. Any 2x status works. 200 or 204 are both common. If the real request fits within those permissions, the browser proceeds.

**9:05** · Pre-flight passes, the actual request goes out. You never wrote any of that.

**9:11** · Your browser did it every time.

**9:14** · If you've ever opened the network tab and seen an options request you didn't write, that's the pre-flight. Your browser going behind your back politely.

**9:24** · If your server has no handler for options, it returns a 404. Pre-flight fails. Real request never fires.

**9:33** · The console reports corores error. The console does not say your options route returned a 404 or there was a pre-flight involved. The console has reported a corors error. That's the full extent of what the console is prepared to share with you today. Open the network tab.

**9:51** · Find the options request. If it's returning a 404, that's your problem.

**9:56** · Not your corores headers, not your allow origin. your options handler. The console just called it a corors error.

**10:04** · Most corores libraries handle this automatically. If you've done it manually, check the pre-flight is a negotiation. These headers are the vocabulary of that exchange. Some appear on every response, some only on pre-flights. One handles credentials, one controls what JavaScript can actually read. None of them are arbitrary.

**10:27** · Access control allow origin on every response pre-flight and final. It answers the browser's core question. Is this origin allowed to see what came back? Wildcard means any origin fine for a genuinely public API, not for anything involving user data or sessions. For multiple specific origins, production and staging, for example, check the incoming origin header against an allow list and echo back the match.

**10:54** · That very origin line, don't skip it if you're behind a CDN.

**11:02** · Without it, a CDN caches the response for one origin and serves it to a request from another. Coores fails intermittently. Hard to reproduce, hard to trace. You also can't commaepparate multiple origins. The spec allows one value or the wild card. The allow list pattern is how you handle multiple.

**11:24** · Access control allow methods lives in the pre-flight response declares which HTTP methods the server accepts. If your actual request uses a method not listed here, the pre-flight fails and your request never goes out. Straightforward access control allow headers. Allow methods covers which HTTP methods are permitted. Allow headers covers which request headers are permitted. Declares which request headers the server accepts. You've probably seen this. Get works fine.

**11:57** · You switch to post with a JSON body. Coors error. You didn't change the server. You didn't change the headers. Almost always the same cause.

**12:07** · content type application JSON wasn't declared in allow headers. Even if the server handles that header perfectly, if it's not declared here, the browser won't allow it through. The server's capability and its cores declaration are entirely separate things. Access control allow credentials. You are likely to encounter this if you move from JWT tokens to session cookies. By default, cross origin requests don't include cookies or HTTP credentials.

**12:38** · Authorization headers with JWTs worked.

**12:41** · You switched to cookiebased sessions, it stopped. This is what changed. Server side true. Client side credentials equals include. Both are required. When allow credentials is true, you cannot use wildcard for allow origin. The browser rejects that combination. It's in the spec. The browser is not going to change its mind. If any origin could make credentialed requests as your users, you're right back to the CSRF problem.

**13:10** · And the last one, access control expose headers, the one you find out about after shipping. Even after a course check passes and the response reaches your JavaScript, the browser still controls which response headers your code can actually see. The full response is there. The browser just doesn't show it all to JavaScript by default. Only these seven are readable by default. Anything outside this list, custom headers, rate limit data, request ids, returns null when your code tries to read it.

**13:42** · Not an error, no warning, just null.

**13:47** · This header tells the browser to expose them. One line. Check this before you ship.

**13:54** · The server declares through every course header we just covered. the browser enforces. It holds both the server's policy and the user's interests simultaneously.

**14:05** · Your code works within what those two agreed. It can't change the policy. It can only make requests that fit inside it. This is why you can't fix cores from the client side. The browser is not the problem. The policy lives on the server.

**14:21** · When you hit a cores error, open the network tab. Look at the actual headers, not the console. The console gives you a summary. The network tab gives you evidence. Check if a pre-flight fired.

**14:34** · Options to the same endpoint. What status did it return? What came back? If a pre-flight fired, does the response allow the right origin, methods, and headers. If no pre-flight, is allow origin present? Does it exactly match your request's origin? one of four things almost always.

**14:56** · Look at that error again. Same error.

**14:58** · Hopefully, you're reading it differently. Now, no access control allow origin header is present doesn't mean your code is broken. It means the server hasn't told the browser what it allows. The browser is waiting, enforcing the absence of a declaration.

**15:14** · Your browser is running code from hundreds of websites at once on behalf of a user who's logged into their bank and their email. The same origin policy is what keeps those contexts from colliding. Coors is how servers deliberately open a gap in that wall when they actually mean to.

**15:33** · Server declares, browser enforces. Your job is to make sure those two are aligned.

**15:40** · If this helped, please like and share.

**15:42** · Thanks for watching and see you in the




