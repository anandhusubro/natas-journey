# Natas 07

## Objective

Analyze how the application loads content and determine whether user-controlled input can influence which files are displayed.

## Skills Practiced

* URL Analysis
* Parameter Manipulation
* Web Application Reconnaissance
* File Inclusion Investigation

## Tools Used

* Web Browser
* View Source
* URL Manipulation

## Challenge Description

The application provided navigation links that dynamically loaded content based on values passed through URL parameters. The goal was to understand how the application handled these parameters and identify unintended file access.

## Methodology

1. Examined the application's navigation links.
2. Observed how URL parameters changed when different pages were selected.
3. Investigated how the application used parameter values to determine displayed content.
4. Tested whether alternative file paths could be referenced.
5. Analyzed the application's response to modified input.

## Key Observation

The application relied on user-controlled input to determine which file should be displayed. Insufficient validation allowed access to resources beyond the intended content pages.

## Lessons Learned

* User-supplied input should never be trusted.
* File paths derived from user input require strict validation.
* URL parameters can reveal application logic.
* Small implementation flaws can expose sensitive resources.

## Concepts Introduced

* File Inclusion
* URL Parameters
* Input Validation
* Path Traversal
* Information Disclosure

## Screenshots
The page source contained a hint indicating the location of sensitive content. By analyzing the application's navigation structure and observing how content was loaded through URL parameters, it became clear that the displayed page was determined by a user-controlled value.After identifying the relevant file reference from the source code, the URL parameter was modified to request the hinted resource directly. The application processed the modified input and returned content that was not intended to be accessible through the normal navigation flow.


<img width="1701" height="583" alt="image" src="https://github.com/user-attachments/assets/692d8bc7-0623-4417-a083-02a7fdfa28b5" />


<img width="839" height="343" alt="image" src="https://github.com/user-attachments/assets/087fd325-0df3-477e-8107-4f350b757ba9" />


<img width="1497" height="648" alt="image" src="https://github.com/user-attachments/assets/fd0fbede-361d-47a7-bf90-8eef706123ed" />


