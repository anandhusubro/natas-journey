# Natas 06

## Objective

Analyze the application's source code to identify how user input is validated and locate sensitive information used for authentication.

## Skills Practiced

* Source Code Analysis
* Web Enumeration
* PHP Code Review
* Information Disclosure

## Tools Used

* View Source
* Web Browser
* Developer Tools

## Challenge Description

The application contained a form that required a secret value to gain access. The source code was available for inspection and revealed details about how the secret was validated.

## Methodology

1. Inspected the application's source code.
2. Identified a PHP include statement referencing an external file.
3. Investigated the referenced resource.
4. Analyzed the contents of the included file.
5. Used the discovered information to satisfy the application's validation logic.

## Key Observation

Sensitive application data was stored in a file that was directly accessible through the web server.

## Lessons Learned

* Source code can reveal valuable information about application structure.
* Included files may expose sensitive data if they are web-accessible.
* Secrets should not be stored in publicly accessible locations.
* Information disclosure vulnerabilities often lead to authentication bypass.

## Concepts Introduced

* PHP include Statements
* Information Disclosure
* Source Code Review
* Sensitive Data Exposure

## ScreenShots
The screenshot shows the contents of an included PHP file being viewed directly through the browser's developer tools. The file contained a server-side variable used by the application during authentication.By analyzing the application's source code, it was possible to identify the referenced include file and investigate its contents. The file exposed sensitive information that should not have been accessible from the web server.

<img width="960" height="521" alt="image" src="https://github.com/user-attachments/assets/6371597f-2ac9-422a-89a0-58b500f86aa5" />


<img width="946" height="586" alt="image" src="https://github.com/user-attachments/assets/2b25e041-32f4-4169-b614-8195fcc578ed" />


<img width="278" height="505" alt="image" src="https://github.com/user-attachments/assets/01996b05-f770-4f86-b2b5-d034ea069d41" />
