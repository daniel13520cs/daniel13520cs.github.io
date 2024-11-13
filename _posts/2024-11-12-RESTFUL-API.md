---
title: "Introduciton of RESTFUL API"
categories:
tags:
  - api
  - backend
  - rest
---

## What is RESTFUL API?

Representational State Transfer (REST) is a software architecture that imposes conditions on how an API should work.

## Use case

RESTFUL API are generally implemented in web services and thoses that implement REST architecture are called RESTful web services.

## RESTFUL API Properties

- **Uniform interface**: It indicates that the server transfers information in a standard format. The formatted resource is called a representation in REST. This format can be different from the internal representation of the resource on the server application.
- **Statelessness**: This refers to a communication method in which the server completes every client request independently of all previous requests.
- **Layered system**: The client can connect to other authorized intermediaries between the client and server, and it will still receive responses from the server.
- **Cacheability**: support caching, which is the process of storing some responses on the client or on an intermediary to improve server response time.
- **Code on demand**: Servers can temporarily extend or customize client functionality by transferring software programming code to the client.

## Why do we use RESTFUL API

1. RESTFUL decouples client and server side applications and makes both more scalable.
2. Representaiton makes the APIs easier to understand and manage.

## Example

| **HTTP Method** | **Endpoint URL**               | **Description**                          | **Example curl Command**                                                                                         |
|-----------------|--------------------------------|------------------------------------------|-------------------------------------------------------------------------------------------------------------------|
| **GET**         | `/items`                       | Retrieve all items                       | `curl -X GET http://127.0.0.1:5000/items`                                                                         |
| **GET**         | `/items/<item_id>`             | Retrieve a single item by ID             | `curl -X GET http://127.0.0.1:5000/items/1`                                                                      |
| **POST**        | `/items`                       | Create a new item                        | `curl -X POST http://127.0.0.1:5000/items -H "Content-Type: application/json" -d "{\"name\":\"New Item\",\"price\":12.99}"` |
| **PUT**         | `/items/<item_id>`             | Update an existing item by ID            | `curl -X PUT http://127.0.0.1:5000/items/1 -H "Content-Type: application/json" -d "{\"name\":\"Updated Item\",\"price\":14.99}"` |
| **DELETE**      | `/items/<item_id>`             | Delete an item by ID                     | `curl -X DELETE http://127.0.0.1:5000/items/1`                                                                   |

## Summary

A RESTful Web API offers a standardized approach that meets the demands of modern web applications, providing scalability and ease of maintenance.

{% include EndBlog.md %}
