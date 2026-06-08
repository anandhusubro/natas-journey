# Natas 03

## Objective

Discover information that has been intentionally hidden from search engines and identify exposed resources.

## Skills Practiced

* Web Reconnaissance
* robots.txt Analysis
* Directory Enumeration
* Resource Discovery

## Tools Used

* Web Browser
* View Page Source
* Browser Developer Tools

## Challenge Description

A comment in the page source indicated that information leaks had been addressed and that search engines would no longer find the hidden content.

## Methodology

1. Inspected the page source for clues.
2. Identified a reference to search engine indexing.
3. Investigated the website's robots.txt file.
4. Analyzed paths excluded from crawler access.
5. Explored the discovered directory structure.

## Key Observation

The robots.txt file revealed a directory that had been excluded from search engine indexing but remained directly accessible.

## Lessons Learned

* robots.txt is not a security control.
* Directories excluded from search engines can still be accessed by users.
* Hidden locations should not be considered protected locations.
* Reconnaissance often reveals information that is not visible in the application's interface.

## Concepts Introduced

* robots.txt
* Search Engine Crawlers
* Directory Enumeration
* Information Disclosure

## ScreenShots
*It’s easy to make an educated guess that it involves the google-bot and hence the robots.txt file. Accessing the file gives us a secret directory named as /s3cr3t/
<img width="838" height="412" alt="image" src="https://github.com/user-attachments/assets/286446e2-4028-44ef-a576-2605cfce7088" />

*You will find a users.txt file inside the directory with the password for the next level.
 <img width="782" height="417" alt="image" src="https://github.com/user-attachments/assets/49dbc0fd-8ceb-4bdc-9802-cfe781f0a5f7" />

 *Voila

 
 <img width="691" height="267" alt="image" src="https://github.com/user-attachments/assets/8cfa2cb6-ff04-48fb-9830-880794a99a9f" />

 



