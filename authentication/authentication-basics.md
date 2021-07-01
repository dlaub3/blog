---
quality: medium
author:
title: Website Authentication Basics - From Server Side Sessions to JWTs
date: 2021-06-11
draft: false
description: A guide to understanding website authentication
tags: ["jwt", "server side sessions"]	
categories: ["authentication"]
toc: false

---

## A basic summary of authentication

**Scenario**: Given I am a user and I want to view my profile page, I must first login before I can view my profile.

### 1. Becoming Authenticated

**Scenario**: As a user I send the server my login credentials by submitting some type of login form.

The server fist looks up the username in the database which verifies that an account does exist for that username. Next the server verifies the password. 
Today most servers don't store passwords in the database. Instead they store a one-way(hopefully) hash of the password (If you'd like to learn more about securely hashing a password I recommend this article: https://crackstation.net/hashing-security.htm ). 
In order to validate the password, the server will first hash the password it received in the login request and then compare the newly generated hash with the hash stored in the database. 
If they match, the server will send the browser some data to use as proof of authentication. The proof of authentication is often sent in a cookie which the browser automatically stores. 

### 2. Validating Authentication 

There are two parts to validating authentication: 
- 1. The browser must send some proof of authentication to the server (typically in a cookie) 
- 2. The server must validate this proof and not blindly accept it as true. 

But why bother storing this proof of authentication at all? Why not just send the username and password with every request? Good question.
The truth is, sending your username and password over every request would work. But it's very insecure. 
By using some other proof of authentication the username and password are only sent over the network one time and it's possible to easily revoke the authentication without requiring users to change their passwords.

Note: The fact that a user has to send their username and password to login is why it's important to use HTTPS.

## Authentication Strategies

### Sever Side Sessions
Server Side Sessions is a very basic means of authentication. With this method the server keeps a cache of users who have authenticated...on the server. 
Whenever a request comes in the server will check the cookie(which is automatically sent to the server by the browser and is typically where the proof of authentication is stored) and the servers cache (to validate the data from the cookie). 
If the user is authenticated the server will allow them to see content that would otherwise be hidden such as the users profile page.

Cons:
One limitation of this method is that the cache is stored on a single server. If there is a lot of traffic the server will struggle to handle all of the requests, slow down, and possibly crash. 
The only way to improve the server's performance is to upgrade physical components like RAM and CPU. This is called vertical scaling(in contrast to horizontal scaling which involves adding more servers to help share the load). 
If you tried to combine server side sessions with horizontal scaling a user may login on server #1, but the next request may be handled by server #2, which the user hasn't logged into. When that happens server #2 will ask the user to login again. 
As you can see, that would be a very bad experience for users.

### Authentication Database 
Storing the users authentication data in a database is one way to overcome the limitations of server side sessions. In this scenario a user will login on server #1 and server #1 will store the authentication details in the database. 
Now a subsequent request may be handled by server #2. Server #2 will check the database to see if the user is logged in and find the data that was stored by server #1. So now the application may be scaled horizontally as well as vertically. 

Cons: 
However, the bottleneck for scalability is now the authentication database since every server must query it for authentication. This is also a single point of failure; if the database goes down, no one can login. 

Here is a link to a great course on this technique: https://www.linkedin.com/learning/php-managing-persistent-sessions

### JWT(JSON Web Tokens)
A JWT is a string of characters like this `eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c`. 
It contains all of the information that a server needs to authenticate a user. This makes a JWT stateless since the state of being logged in is stored in the token itself and not on the server or in a database. 
Whenever a user authenticates the server sends the browser the JWT, the browser will store this token and send it in all subsequent request to the server as proof of being authenticated (again, a cookie is commonly used for this purpose). 
Now whenever an application needs to scale horizontally its not dependent on looking up login information in a database it can simply validate the JWT and then allow the user to see his profile page. 

Note: JWTs are signed by a secret key on the server. This makes it impossible for someone to construct a fake JWT or for a user to modify their JWT before sending it back to the server.

Cons:
One downside to using JWTs is that it's not possible to easily invalidate a single users JWT. You can change the secret key used to sign all tokens, but this will invalidate all tokens that have been signed with that key.

### OAuth/Auth0 and OpenID Connect
Auth0 (zero - it's a number not a letter) is a company/service that handles the implementation details of authentication for you. As you can see on the company website they offer Single Sign On, Multi-Factor Authentication and more.

OAuth and OpenID Connect are authentication protocols that allow you to use your login for one website to login to another website. It's the technology that allows you to login to so many other websites using only your GitHub credentials.
This is a great video on that subject: https://www.youtube.com/watch?v=996OiexHze0

## Security
In this article I've assumed that cookies will be used for authentication. Cookies are vulnerable to XSS(Cross Site  Scripting) and CSRF(Cross Site Request Forgery). You can mitigate this by setting the cookie attributes `SameSite`, `Secure`, and `HttpOnly`.

- `SameSite=Strict` the cookie will not be sent with requests initiate by a third party website
- `Secure` the cookie will only be sent over secure HTTPS connections
- `HttpOnly` the cookie cannot be accessed with JavaScript

## Summary
In most scenarios you won't be required to implement authentication yourself. You’ll just need a basic understanding of how it works and the trade-offs between various methods. 
You’ll also need to understand the security risks and how authentication can be compromised. 

