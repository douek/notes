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


#### RPC - Remote Procedure Call
RPC is about actions. The client send the method name and parameters to the servers and recive a JSON or XML response.

You will use `GET` for read-only operations and `POST` for the rest. 

RPC is great for operations that are not encapsulated with CRUD and that have side effects on more than one resource.  
RPC is not exclusive to HTTP (binary serialization), and it is consider high proformance protocol.  Options are gRPC, Thrift and Apache.

#### GraphQL
