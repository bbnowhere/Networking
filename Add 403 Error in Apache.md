# ✅ **Step-by-Step: Custom 403 Forbidden Page in Apache**

---

## **1️⃣ Create Your Custom 403 Error Page**

Create a file like:

**/var/www/html/403.html**

Example:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Access Denied</title>
    <style>
        body { font-family: Arial; text-align:center; padding:60px; }
        .box { display:inline-block; padding:30px; border:2px solid #d00; border-radius:10px; }
        h1 { color:#d00; }
    </style>
</head>
<body>
    <div class="box">
        <h1>403 - Access Forbidden</h1>
        <p>You don't have permission to access this resource.</p>
        <p>Please contact admin if you believe this is a mistake.</p>
    </div>
</body>
</html>
```

---

## **2️⃣ Enable Custom ErrorDocument in Apache**

### **Option A — Add in VirtualHost (Recommended)**

Edit your site configuration:

```
sudo nano /etc/apache2/sites-available/000-default.conf
```

Add inside the `<VirtualHost>` block:

```
ErrorDocument 403 /403.html
```

Example:

```apache
<VirtualHost *:80>
    DocumentRoot /var/www/html
    ErrorDocument 403 /403.html
</VirtualHost>
```

---

### **Option B — Add in .htaccess (If AllowOverride is enabled)**

Inside your site folder (e.g., `/var/www/html/.htaccess`):

```
ErrorDocument 403 /403.html
```

⚠ You must have:

```
AllowOverride All
```

in your VirtualHost for .htaccess to work.

---

## **3️⃣ Reload Apache**

```
sudo systemctl reload apache2
```

---

## **4️⃣ Test It**

Create a forbidden folder and deny access:

```
mkdir /var/www/html/secret
echo "Private" > /var/www/html/secret/test.txt
chmod -R 700 /var/www/html/secret
```

Now visit:

```
http://yourserver/secret/test.txt
```
