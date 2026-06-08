# Natas 02

## Objective

Investigate a page that appears to contain no useful information and identify hidden resources exposed by the web server.

## Skills Practiced

* Resource Enumeration
* Directory Discovery
* Source Code Inspection
* Web Reconnaissance

## Tools Used

* Web Browser
* View Page Source
* Browser Developer Tools

## Challenge Description

The page stated that there was nothing on it, encouraging further inspection of the underlying resources.

## Methodology

1. Examined the visible page content.
2. Inspected the HTML source.
3. Identified referenced resources.
4. Investigated the location of discovered resources.
5. Enumerated exposed content within the same directory.

## Key Observation

Resources linked by a web page can reveal additional locations that expose unintended information.

## Lessons Learned

* Hidden files are not necessarily protected.
* Directory exposure can reveal sensitive information.
* Enumeration is often more valuable than guessing.

## Concepts Introduced

* Directory Listing
* Resource Enumeration
* Information Disclosure
* Web Application Reconnaissance
## ScreenShots
<img width="1899" height="544" alt="image" src="https://github.com/user-attachments/assets/714daafd-107e-4c92-a0f8-71b8b8b06492" />

*Found Suspicious looking link "http://natas2.natas.labs.overthewire.org/files/pixel.png"
<img width="1753" height="759" alt="image" src="https://github.com/user-attachments/assets/15f56c95-99ce-4371-92ad-303cd493e731" />

*Checked "http://natas2.natas.labs.overthewire.org/files" and found 
<img width="846" height="429" alt="image" src="https://github.com/user-attachments/assets/3a57139f-1775-4538-8644-3b5afe8b1d8b" />

*Found password in users.txt
<img width="487" height="205" alt="image" src="https://github.com/user-attachments/assets/28252cf1-b48c-4c6c-abca-15740ae08525" />





