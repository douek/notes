# Designing Web APIs Book notes

### Chapter 1 - Why Building an API
* What is the buissnes case?
* Who are the users?
  * Internal developer
  * External developer
  * APIs as a product

### Chapter 2 - API Paradigms

### Request-Response APIs
Typically expose as an interface through HTTP-base web serve. APIs define a set of endpoints and clients make http requests for data to those endpoints.
Responses are typically sent back as JSON or XML.

Common paragdims are: REST, RPC and GraphQL

#### REST - Representational State Transfer
Expose data and resurces throug http, can be excces through the web.  
Expose method for each CRUD operations:
* `POST` for create
* `GET` for read
* `PUT` for update
* `DELETE` for delete

Resources are part of the URLs are usally implement to a list/collection `/user` and to a specifc element details `/users/{user-id}`.  

The URL represent the relationships between the collection and elements.  
`POST /posts/{post-id}/comment`, it is clear from the URL he operation is create a comment for the speceif post (and not general comment to a comments collaction).  

You may need to use REST api for non-CRUD operation, like lock/archive a resource (`PUT`) or search for a resource (`GET`).

* Pros:
  * Standard method names, format and status codes
  * Utilizes HTTP features
  * Easy to maintain
* Cons:
  * Big payloads
  * Multiple calls
* Use for APIs doing CRUD operations

#### RPC - Remote Procedure Call
RPC is about actions. The client send the method name and parameters to the servers and recive a JSON or XML response.

You will use `GET` for read-only operations and `POST` for the rest. 

RPC is great for operations that are not encapsulated with CRUD and that have side effects on more than one resource.  
RPC is not exclusive to HTTP (binary serialization), and it is consider high proformance protocol.  Options are gRPC, Thrift and Apache.

* Pros:
  * Easy to understand
  * Lightweight payloads
  * High performance
* Cons:
  * Discovery is difficult
  * Limited standards
  * Can lead to function explosion.
* Use for APIs exposing serval actions

#### GraphQL
Query language API, client define the stucture and recive the information from the server in the exact structure.  
There is only one URL entrypoint, and it is supported `POST` and `GET` operations.

Adventage over REST and RPC:
* Avoid multiple API calls - can query all information needed in a single call.
* Avoid versioning - Adding fields and types not effecting existing queries, it easy to analize fields usage.
* Client control the payload size and recive only the needed data.
* Query validation

The only drawback is the complaxity added to the API provider. The server need to do additional processing and query optimazation. While for internal usage it is possible to predict the use case and how to optimize the operation, it is harder for external users.

* Pros:
  * No multiple calls
  * Avoids versioning
  * Smaller payloads
  * Strongly typed
  * Built-in introspection
* Cons:
  * Additional query parsing 
  * Backend optimization is difficult
  * Complicated fo simple API
* Use for query flexibility and maintaining consistency
  
### Event-Driven APIs
A thing with a request-response API is that the client need to poll to get the new/updated data.  
Polling in low frequeancy may result missing updates, but polling in high frequancy would lead to high waste of resurces (that may not returned any new data at all).

There are 3 common mechanzem to share data about events in real-time: WebHooks, WebSockets, HTTP Streaming.
 
#### WebHooks
Url that excepts HTTP `POST` (or `GET`, `PUT`,`DELETE`). An API provider implementing WebHooks will post messages to the configured URL whenever there is something new.
 
* Pros:
  * Easy server-server communication
  * HTTP protocol
* Cons:
  * Do not work across firewall and in browsers
  * Headling failures, retries and security is hard
* Use for trigger server-to-server real-time events

#### WebSocket
Streaming communication over TCP, client- server can communicate simultaneously (full-duplex) at low overhead. Designed to work with ports 80 and 443, and requier to maintain live connections (not suitable for mobile devices) which can cause issues when scaling.

* Pros:
  * Two way streaming communication
  * Native browser support
  * Can bypass firewalls
* Cons:
  * Need to maintain persistent connection
  * Bot HTTP
* Use for two-way real-time browser and server communication

#### HTTP Streaming
The server will continue to push noew data in a persist live connection opened by the client.  
There are 2 option to transmit data from server over persisted connection :  
1. Set the `Transfer-Encodeing` header to `chunked` - let the client know that the data will arrived in chunkes.
2. Server-sent events (SSE) - good when working with browsers (EventSource API)

* Pros:
  * Stream over simple HTTP
  * Native browser support
  * Can bypass firewalls
* Cons:
  * Two-way communication is difficult
  * Requier re-connecting to recive diffrent events
* Use for one-way communication over simple HTTP

