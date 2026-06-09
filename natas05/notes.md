# Natas 05

## Objective

Investigate how the application determines whether a user is authenticated and identify weaknesses in the implementation.

## Skills Practiced

* Cookie Analysis
* Session Management Testing
* HTTP Request Inspection
* Web Application Security

## Tools Used

* Browser Developer Tools
* Burp Suite
* HTTP Cookies

## Challenge Description

The application displayed an "Access disallowed" message indicating that the user was not logged in. The objective was to understand how the application tracked authentication status.

## Methodology

1. Inspected the application's requests and responses.
2. Examined cookies stored by the browser.
3. Identified a cookie used to represent authentication state.
4. Modified the cookie value and resent the request.
5. Observed the server's response after the modified request was processed.

## Key Observation

The application trusted a client-controlled cookie to determine whether a user was authenticated.

## Lessons Learned

* Cookies are stored on the client and can be modified.
* Authentication decisions should not rely solely on client-controlled values.
* Session data should be validated server-side.
* Intercepting proxies and browser tools are effective for analyzing application behavior.

## Concepts Introduced

* HTTP Cookies
* Session Management
* Authentication
* Client-Side Trust
* Authorization Bypass

## Screen Shots

* The application indicates that the user is not logged in. Investigation of the application's cookies revealed that authentication status was being tracked through a client-controlled value. Modifying the cookie and resubmitting the request resulted in successful authentication, demonstrating the risks of trusting client-side state for access control decisions.
  
<img width="949" height="513" alt="image" src="https://github.com/user-attachments/assets/75db0d96-030f-4c3b-81ba-57000d79daac" />

<img width="956" height="867" alt="image" src="https://github.com/user-attachments/assets/fbf30a34-30dc-4b52-88e7-f16a545dcc6b" />
