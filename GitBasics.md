# 🚀 1. Drupal Modern Setup (Foundation)

* Drupal installed using **Composer**
* Project structure:

```text
testdev/
 ├── web/              → public folder (document root)
 ├── config/           → configuration (VERY IMPORTANT)
 ├── vendor/           → dependencies (auto-generated)
 ├── composer.json
 ├── composer.lock
```

👉 Web server must point to:

```text
/web
```

---

# 🧠 2. What is Git?

Git = Version Control System

👉 Tracks:

* Code changes
* Who changed what
* When changes happened

---

# ⚙️ 3. Git Basic Setup

### First-time config:

```bash
git config --global user.name "myname"
git config --global user.email "email"
```

---

# 📁 4. Correct Git Location (VERY IMPORTANT)

❌ Wrong:

```text
/web
```

✅ Correct:

```text
testdev/
```

👉 Git must be at **project root**

---

# 🔁 5. Initialize Git

```bash
git init
git add .
git commit -m "Initial Drupal setup"
```

---

# 🌐 6. Connect to GitHub

* Create repo on GitHub

```bash
git remote add origin git@github.com:bbnowhere/testdev.git
git branch -M main
git push -u origin main
```

---

# 🔐 7. GitHub Authentication

❌ Password not allowed
✅ Use SSH

### Setup:

```bash
ssh-keygen -t ed25519 -C "email"
```

Add key to GitHub → SSH Keys

Test:

```bash
ssh -T git@github.com
```

---

# 🌿 8. Branching Strategy

```text
main      → production
develop   → staging
feature/* → development
```

Example:

```bash
git checkout -b feature/login-fix
git commit -m "Fix login"
git push origin feature/login-fix
```

---

# ⚠️ 9. Merge Conflict (Important Skill)

Happens when:

* Same file changed in two places

Example:

```text
<<<<<<< HEAD
your code
=======
other code
>>>>>>> origin/main
```

### Fix:

* Edit manually
* Remove markers
* Then:

```bash
git add .
git commit -m "Resolved conflict"
```

---

# 🚨 10. Sensitive Files (CRITICAL)

## ❌ Never commit:

```text
/web/sites/default/settings.php
/web/sites/default/files/
/vendor/
```

---

## ✅ Add to `.gitignore`

```text
/vendor/
/web/sites/*/files/
/web/sites/*/settings.php
/web/sites/*/services.yml
```

---

## 🔧 If already committed:

```bash
git rm --cached web/sites/default/settings.php
git commit -m "Remove sensitive file"
git push
```

---

# 🔐 11. Security Best Practice

* Change DB password if exposed
* Never store secrets in Git
* Use:

```php
settings.local.php
```

---

# 🔄 12. Drupal + Git Workflow (VERY IMPORTANT)

Drupal uses:

* DB → runtime config
* Files → deployment config

---

## When you change config (Views, content types)

```bash
drush cex -y
git add config/sync
git commit -m "Export config"
git push
```

---

## When pulling changes

```bash
git pull
drush cim -y
drush cr
```

---

# ⚙️ 13. Manual Deployment Flow (Before CI/CD)

On server:

```bash
git pull origin main
composer install --no-dev
drush updb -y
drush cim -y
drush cr
```

---

# 🚨 14. Common Errors You Faced (And Learned)

## 1. Dubious ownership

Fix:

```bash
git config --global --add safe.directory <path>
```

---

## 2. Author identity unknown

Fix:

```bash
git config --global user.name
git config --global user.email
```

---

## 3. Remote not found

Fix:

```bash
git remote add origin <repo>
```

---

## 4. Authentication failed

Fix:
👉 Use SSH instead of password

---

## 5. Non-fast-forward push

Fix:

```bash
git pull --allow-unrelated-histories
```

---

## 6. Merge conflict

Fix manually → commit




