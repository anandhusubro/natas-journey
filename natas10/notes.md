# Natas 10

## Objective

Evaluate whether input filtering mechanisms effectively prevent command injection vulnerabilities within a web application.

## Skills Practiced

* Source Code Analysis
* Input Validation Assessment
* Command Injection Testing
* Security Control Evaluation

## Tools Used

* Web Browser
* View Source
* Linux Command Knowledge

## Challenge Description

The application attempted to improve security by filtering specific shell metacharacters before processing user input. The goal was to determine whether the implemented restrictions were sufficient to prevent abuse of the underlying command execution functionality.

## Source Code Analysis

```php
if(preg_match('/[;|&]/',$key)) {
    print "Input contains an illegal character!";
} else {
    passthru("grep -i $key dictionary.txt");
}
```

The application blocks the characters:

```text
;
|
&
```

before executing the command.

## Methodology

1. Reviewed the application's source code.
2. Identified the input validation mechanism.
3. Analyzed which characters were being filtered.
4. Investigated whether the command could still be influenced without using blocked characters.
5. Tested alternative inputs that bypassed the blacklist.
6. Successfully manipulated the command execution flow despite the implemented restrictions.

## Key Observation

The application relied on a blacklist approach, attempting to prevent exploitation by blocking a limited set of characters. However, user input was still passed directly into a shell command, leaving alternative attack paths available.

## Lessons Learned

* Blacklisting dangerous characters is not a reliable security control.
* Security mechanisms should focus on eliminating root causes rather than blocking individual payloads.
* User-controlled data should never be inserted directly into shell commands.
* Attackers often succeed by finding alternative methods rather than using the most obvious payloads.

## Concepts Introduced

* Command Injection
* Input Filtering
* Blacklist Bypass
* Shell Interpretation
* Secure Input Validation

## Screen shots
<img width="1890" height="735" alt="image" src="https://github.com/user-attachments/assets/ec21f17e-0b67-4d94-9103-014144fd8356" />




