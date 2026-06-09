# Natas 04

## Objective

Gain access to a restricted page by understanding how the web application determines whether a user is coming from an authorized location.

## Skills Practiced

* HTTP Request Analysis
* Header Manipulation
* Web Application Testing
* Traffic Interception

## Tools Used

* Browser Developer Tools
* Burp Suite Repeater
* HTTP Headers

## Challenge Description

The application indicated that access was restricted to users arriving from a specific location. The challenge required understanding how the server identified the origin of a request.

## Methodology

1. Inspected the application's response message for clues.
2. Captured the HTTP request using Burp Suite.
3. Analyzed the request headers sent by the browser.
4. Identified the header used to indicate the source of a request.
5. Modified the request and replayed it using Burp Suite Repeater.

## Key Observation

The application trusted a client-supplied HTTP header to determine whether a user was authorized to access the resource.

## Lessons Learned

* HTTP headers can be modified by the client.
* Client-controlled values should not be used for access control decisions.
* Security mechanisms based solely on request metadata can be bypassed.
* Intercepting proxies are useful for understanding and manipulating web traffic.

## Concepts Introduced

* HTTP Headers
* Referer Header
* Request Manipulation
* Burp Suite Repeater
* Trust Boundaries

## Screen Shots
<img width="947" height="391" alt="image" src="https://github.com/user-attachments/assets/d8d79589-68ac-459e-a673-536da3e5df0d" />


* The screenshot demonstrates the use of Burp Suite Repeater to modify and resend an HTTP request. After intercepting the request to the target application, the Referer header was manually edited to match the value expected by the server.

The modified request was then sent through Repeater, allowing the response to be analyzed without repeatedly interacting with the browser. The server accepted the manipulated request and returned a successful response, demonstrating that access control was based on a client-supplied HTTP header.

<img width="956" height="933" alt="image" src="https://github.com/user-attachments/assets/1ba1a618-45b1-4f04-9ed8-dfc867648253" />



