# Proxies

## What is a proxy server?

A proxy server is an intermediate piece of software or hardware that sits between the client and the server. Clients connect to a proxy to make a request for a service like a web page, file, or connection from the server. Essentially, a proxy server (aka the forward proxy) is a piece of software or hardware that facilitates the request for resources from other servers on behalf of clients, thus anonymizing the client from the server.

## Forward Proxy

![Forward Proxy](forward-proxy.png)

Typically, forward proxies are used to cache data, filter requests, log requests, or transform requests (by adding/removing headers, encrypting/decrypting, or compressing a resource).

> A forward proxy can hide the identity of the client from the server by sending requests on behalf of the client.

In addition to coordinating requests from multiple servers, proxies can also optimize request traffic from a system-wide perspective. Proxies can combine the same data access requests into one request and then return the result to the user; this technique is called collapsed forwarding. Consider a request for the same data across several nodes, but the data is not in cache. By routing these requests through the proxy, they can be consolidated into one so that we will only read data from the disk once.

## Reverse Proxy

A reverse proxy retrieves resources from one or more servers on behalf of a client. These resources are then returned to the client, appearing as if they originated from the proxy server itself, thus anonymizing the server. Contrary to the forward proxy, which hides the client’s identity, a reverse proxy hides the server’s identity.

![Reverse Proxy](reverse-proxy.png)

In the above diagram, the reverse proxy hides the final server that served the request from the client. The client makes a request for some content from facebook.com; this request is served by facebook’s reverse proxy server, which gets the response from one of the backend servers and returns it to the client.

A reverse proxy, just like a forward proxy, can be used for caching, load balancing, anonymizing the servers, or routing requests to the appropriate servers.

## Summary

A proxy is a piece of software or hardware that sits between a client and a server to facilitate traffic. A forward proxy hides the identity of the client, whereas a reverse proxy conceals the identity of the server. So, when you want to protect your clients on your internal network, you should put them behind a forward proxy; on the other hand, when you want to protect your servers, you should put them behind a reverse proxy.
