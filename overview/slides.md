!SLIDE subsection

# WebSocket Browser Apps<br>with Spring
## [Rossen Stoyanchev](https://twitter.com/rstoya05)

!SLIDE small bullets incremental
# Java API for WebSocket
# (JSR-356)
<br><br>
* Much debate on JSR expert group
* Convergance of pre-existing implementations
* Resulted in all-new WebSocket runtimes

!SLIDE small bullets incremental
# Runtime Support

* Tomcat 8 (currently RC3)-<br>expect backport to Tomcat 7 soon
* Jetty 9 has native WebSocket API-<br>Jetty 9.1 adds `JSR-356`
* Glassfish 4 w/ Tyrus WebSocket engine
* More to follow

!SLIDE small bullets incremental
# Why JSR-356 Isn't Enough?
<br><br>
* No fallback options...<br>means no IE<10 + potential network issues
* No sub-protocol support...<br>means hard to build real apps
* No way to [handle HTTP and WebSocket requests](https://java.net/jira/browse/WEBSOCKET_SPEC-211)<br>from one place (i.e. "front controller" pattern)
* Too low level--<br>need a framework or you'll end up building one

!SLIDE smaller bullets incremental
# WebSocket API - Very Low Level
<br><br>
* Single WebSocket connection per client
* results in single `@ServerEndpoint` per application
* limits you to one annotated class
* Not the right level of abstraction--<br>much like plain Servlet is too course-grained

!SLIDE smaller bullets incremental
# Raw WebSocket

* A message is a blank page--<br>you can write anything (custom format?)
* Can't provide useful annotations--<br>what's the equivalent of `@RequestMapping`?
* How do broadcast to some subset of users?
* No guarantees for message delivery

!SLIDE smaller bullets incremental
# What's in Spring Framework 4?
<br><br>
* Transparent WebSocket emulation based on [SockJS](sockjs.org)
* Abstractions for building messaging architectures--<br>`Message`, `MessageChannel`, `MessageHandler`, ...
* [STOMP](http://stomp.github.io/) sub-protocol support
* A programming model and foundation for<br>beyond `"Hello WebSocket world!"`

!SLIDE small bullets incremental
# [SockJS](https://github.com/sockjs/sockjs-client)
<br><br>
* Emulate WebSocket API as close as possible
* At least one streaming protocol per major browser
* Cross-domain and cookie support
* Polling in old browsers, hosts behind restrictive proxies
* No Flash inside

!SLIDE small

## SockJS and WebSocket
## are transport layer concerns both interesting on their own
<br><br><br>
## However this presentation
## focuses on the higher-level programming model




