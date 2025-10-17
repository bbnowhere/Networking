## 🧩 What is **PHP-FPM**

**PHP-FPM** stands for **PHP FastCGI Process Manager**.
It’s the **engine** that runs your PHP code on a web server.

When you open a Drupal (or any PHP) page like `/user/login`, your browser sends a request to Apache or Nginx → which forwards it to PHP-FPM → which executes the PHP code → and sends HTML back to the web server → then to your browser.

### In short:

```
Browser → Apache → PHP-FPM → PHP code → Apache → Browser
```

---

### 🧠 Why PHP-FPM Exists

Before PHP-FPM, PHP ran as:

* **mod_php** inside Apache (less efficient).
* or **CGI scripts** (very slow).

**PHP-FPM** was introduced to:
✅ Handle **many users at once** efficiently
✅ Avoid restarting PHP for each request
✅ Allow **process control** (you can tune number of PHP workers)
✅ Provide **error and slow logs** for debugging

---

## ⚙️ How PHP-FPM Works

PHP-FPM runs a **master process** and several **worker (child) processes**:

* The **master** listens for incoming PHP requests.
* The **workers** execute your PHP scripts.
* Each worker handles one request at a time.

You can configure how many workers to run using:

```ini
pm = dynamic
pm.max_children = 20
pm.start_servers = 5
pm.min_spare_servers = 5
pm.max_spare_servers = 10
```

---

## 📜 What “log” Means

A **log** is just a **record of what’s happening inside a system or application** — like a diary for your server.

PHP-FPM writes logs to help you monitor, debug, and understand its behavior.

### Common PHP-FPM Logs:

1. **Error log** — shows warnings, crashes, or config problems
   📄 `/var/log/php-fpm/error.log` or `/var/log/php-fpm/www-error.log`

   Example:

   ```
   [WARNING] [pool www] server reached pm.max_children setting (10)
   ```

   → means too many PHP requests at once — increase `pm.max_children`.

---

2. **Slow log** — lists PHP scripts that are taking too long to execute
   📄 `/var/log/php-fpm/www-slow.log`

   Example:

   ```
   [pool www] pid 18342
   script_filename = /var/www/html/index.php
   [0x00007f4b8f4b7348] db_query() /var/www/html/modules/views/views.module:42
   ```

   → tells you which file or function is slow.

---

3. **Access log** *(optional)* — shows requests being handled
   📄 `/var/log/php-fpm/www-access.log`

---

4. **System log (via systemd)**
   You can also view PHP-FPM logs through system journal:

   ```bash
   sudo journalctl -u php-fpm
   ```

   Example:

   ```
   php-fpm.service: Consumed 18h 11min CPU time
   ```

   → summary of CPU usage for that service.

---

## 🧰 In Summary

| Concept   | Meaning                                                 | Example                              |
| --------- | ------------------------------------------------------- | ------------------------------------ |
| PHP-FPM   | PHP FastCGI Process Manager — runs PHP code efficiently | Handles your Drupal site’s PHP files |
| Worker    | A process that executes one PHP request at a time       | Child process under PHP-FPM          |
| Log       | Recorded events and errors                              | “server reached max_children”        |
| Slow Log  | Shows scripts taking long time                          | `/var/log/php-fpm/www-slow.log`      |
| Error Log | Shows warnings and crashes                              | `/var/log/php-fpm/error.log`         |


