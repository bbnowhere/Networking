**Postfix troubleshooting and admin commands**

## 🧩 **1. Check Postfix Service & Basic Status**

These confirm Postfix is running and correctly configured.

```bash
# Check Postfix status
systemctl status postfix

# Start / stop / restart Postfix
systemctl start postfix
systemctl stop postfix
systemctl restart postfix
systemctl reload postfix  # reload config without dropping mail

# Check Postfix process
ps aux | grep postfix

# Check listening ports (should show :25, maybe :587 or :465)
netstat -tulnp | grep master
ss -ltnp | grep postfix
```

---

## 🗂️ **2. Check Postfix Configuration**

```bash
# Print current configuration values
postconf

# Check a specific parameter
postconf myhostname
postconf -n   # show only non-default settings

# Validate configuration syntax
postfix check

# Where Postfix stores its data
postconf data_directory
postconf queue_directory
```

---

## 🧾 **3. Mail Queue Operations**

Postfix keeps undelivered mail in queues — **maildrop**, **incoming**, **active**, **deferred**, etc.

```bash
# List mail queue (summary)
mailq
postqueue -p

# Flush the queue (try to deliver again)
postqueue -f

# Delete all queued mail (careful!)
postsuper -d ALL

# Delete all deferred mails
postsuper -d ALL deferred

# View specific queue entry
postcat -q QUEUE_ID

# Requeue specific message
postsuper -r QUEUE_ID
```

Example:

```bash
postcat -q 485019600AB
```

shows message headers and body for that queued mail.

---

## 🔍 **4. Log Analysis (maillog / syslog)**

Postfix logs are your best friend.

```bash
# Common locations
/var/log/maillog
/var/log/mail.log

# Follow live logs
tail -f /var/log/maillog
tail -f /var/log/mail.log | grep postfix

# Search for specific message ID or sender
grep 485019600AB /var/log/maillog
grep "status=" /var/log/maillog

# Count deferred mails
grep "deferred" /var/log/maillog | wc -l

# View reject or bounce messages
grep "reject" /var/log/maillog
grep "bounce" /var/log/maillog
```

---

## ⚙️ **5. DNS and MX Verification**

Mail delivery issues often come from DNS or relay problems.

```bash
# Check MX record
dig mx example.com
host -t mx example.com
nslookup -type=mx example.com

# Check reverse DNS (PTR)
dig -x <your_server_ip>

# Test HELO and banner
telnet mail.example.com 25
# or
nc mail.example.com 25
```

You should see something like:

```
220 mail.example.com ESMTP Postfix
```

---

## 📬 **6. Test Sending Mail**

```bash
# Send test mail (interactive)
sendmail user@example.com
Subject: Test
This is a test message
.

# or use `mail` command if available
echo "Test mail" | mail -s "Test" user@example.com

# or test with swaks (SMTP test tool)
swaks --to user@example.com --from test@yourdomain.com --server localhost
```

---

## 🔐 **7. TLS, SASL, and Authentication Checks**

```bash
# Check if Postfix supports TLS
postconf smtpd_tls_security_level

# Test STARTTLS support
openssl s_client -starttls smtp -connect mail.example.com:25

# Test SMTP AUTH
telnet mail.example.com 587
# or
openssl s_client -starttls smtp -connect mail.example.com:587
```

---

## 🧠 **8. Queue Directory Structure**

For reference:

```
/var/spool/postfix/
├── active       -> currently being delivered
├── bounce       -> bounce messages
├── defer        -> temporarily failed messages
├── deferred     -> retry later
├── incoming     -> new mail
├── maildrop     -> mail from local submission
├── hold         -> manually held messages
```

---

## 🛡️ **9. Common Fix Commands**

```bash
# Fix ownership/permissions if broken
postfix set-permissions

# Restart all Postfix daemons cleanly
postfix stop
postfix start

# Enable/disable Postfix temporarily
systemctl disable postfix
systemctl enable postfix
```

---

## 📈 **10. Monitoring Commands**

```bash
# Show mail statistics
grep "status=" /var/log/maillog | awk '{print $7}' | sort | uniq -c

# Show top deferred domains
grep deferred /var/log/maillog | awk '{print $8}' | cut -d= -f2 | sort | uniq -c | sort -nr | head
```

---

## 🧩 **11. Debugging Tools**

```bash
# Run Postfix in debug mode
postfix set-permissions
postfix check
postfix -v start   # verbose logging

# Enable verbose logging for SMTP
postconf -e "debug_peer_level = 3"
postconf -e "debug_peer_list = example.com"
systemctl reload postfix
```

---

## 🧹 **12. Cleanup / Maintenance**

```bash
# Remove old deferred mails (older than 7 days)
find /var/spool/postfix/deferred -type f -mtime +7 -delete

# Check queue size
mailq | grep -c '^[A-F0-9]'
```
