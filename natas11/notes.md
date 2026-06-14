# Natas Level 11 Write-up

## Overview
**Level:** Natas 11  
**Challenge:** Manipulate the encrypted `data` cookie to reveal the password for the next level.

## Enumeration

After logging in, the page allows changing the background color. Viewing the page source reveals that the application stores user preferences inside a cookie called `data`.

The relevant function is:

```php
function xor_encrypt($in) {
    $key = '<secret>';
    $text = $in;
    $outText = '';

    for($i=0; $i<strlen($text); $i++) {
        $outText .= $text[$i] ^ $key[$i % strlen($key)];
    }

    return $outText;
}
```

## Exploitation

The encryption process is:

Plaintext XOR Key = Ciphertext

Rearranging the equation gives:

Ciphertext XOR Plaintext = Key


**Step 1:** Copy the data cookie

Using the browser's developer tools, copy the value of the data cookie.


**Step 2:** Recover the XOR key

Base64-decode the cookie and XOR it with the known plaintext:

{"showpassword":"no","bgcolor":"#ffffff"}

The resulting bytes reveal a repeating pattern. The repeating XOR key is:

qw8J

**Step 3:** Create a malicious cookie

Modify the JSON so that showpassword is set to "yes":

{"showpassword":"yes","bgcolor":"#ffffff"}

Use the recovered key to XOR-encrypt the new JSON and Base64-encode the output.

The following PHP script generates the new cookie value:

<?php
function xor_encrypt($in, $key) {
    $text = $in;
    $outText = "";

    for($i = 0; $i < strlen($text); $i++) {
        $outText .= $text[$i] ^ $key[$i % strlen($key)];
    }

    return $outText;
}

$key = "qw8J";

$data = json_encode(array(
    "showpassword" => "yes",
    "bgcolor" => "#ffffff"
));

echo base64_encode(xor_encrypt($data, $key));
?>


**Step 4:** Replace the cookie
Open Developer Tools → Application/Storage → Cookies.
Replace the existing data cookie with the generated value.
Refresh the page.

The application now processes:

{"showpassword":"yes","bgcolor":"#ffffff"}

and reveals the password for Natas Level 12.

<img width="1347" height="552" alt="image" src="https://github.com/user-attachments/assets/0b08a0ee-8adf-4ce5-ae78-1c5a191fd881" />

