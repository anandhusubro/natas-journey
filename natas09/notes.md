# Natas 09

## Objective

Investigate how user input is processed by the application and determine whether it can influence operating system commands executed on the server.

## Skills Practiced

* Source Code Analysis
* Command Injection
* Linux Command-Line Fundamentals
* Input Validation Assessment

## Tools Used

* Web Browser
* View Source
* Linux Command Knowledge

## Challenge Description

The application accepted user input through a search parameter and used it to perform a lookup operation. Reviewing the source code revealed that the input was passed directly into a system command without sanitization.

## Source Code Analysis

```php
if(array_key_exists("needle", $_REQUEST)) {
    $key = $_REQUEST["needle"];
}

if($key != "") {
    passthru("grep -i $key dictionary.txt");
}
```

The application takes user-controlled input and inserts it directly into a command executed by the operating system.

## Methodology

1. Reviewed the application source code.
2. Identified the use of the `passthru()` function.
3. Traced the flow of user input from the request parameter into the shell command.
4. Analyzed how command-line arguments are processed by `grep`.
5. Manipulated the input to influence the command's behavior.
6. Leveraged the modified command to access information outside the intended scope of the application.

## Key Observation

User input was inserted directly into a shell command without validation or sanitization. This allowed command arguments to be manipulated and changed the behavior of the executed command.

## Lessons Learned

* User-controlled input should never be passed directly to shell commands.
* Understanding how command-line programs process arguments is critical when analyzing vulnerabilities.
* Applications should validate and sanitize all external input before using it in system commands.
* Source code review can quickly reveal dangerous functionality.

## Concepts Introduced

* Command Injection
* Argument Injection
* PHP `passthru()`
* Linux Command Execution
* Input Validation

## Screenshots
The application's source code revealed that user input was passed directly into a system command through the PHP passthru() function. By understanding how the command-line utility processed arguments, it was possible to influence the command's behavior and access information beyond the application's intended functionality. This demonstrates the risks of executing shell commands with unsanitized user input.

<img width="1902" height="738" alt="image" src="https://github.com/user-attachments/assets/d5852c06-e820-4e67-a44d-ee04d9b345d9" />

