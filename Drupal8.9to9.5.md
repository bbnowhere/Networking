# Drupal Upgrade Guide: 8.9 → 9.5


---

## 🧭 Overview

This document records the complete process and troubleshooting steps for upgrading a Drupal project from **version 8.9** to **version 9.5**, including resolving Zend, Webprofiler, and config directory issues.

---

## ✅ 1. Pre‑Upgrade Checklist

1. **Backup everything**
   - Database, files, and `sites/default/settings.php`
2. **Ensure system requirements**
   - PHP ≥ 7.4 (recommended 8.0/8.1)
   - Compatible database version
3. **Upgrade to latest 8.9.x patch**
4. **Check module and theme compatibility**
   - Use the [Upgrade Status](https://www.drupal.org/project/upgrade_status) module
5. **Review custom code for deprecated APIs**
6. **Migrate to Composer if not already using it**
7. **Use a staging environment for testing**

---

## 🧰 2. Upgrade Steps (Composer)

```bash
# Update Drupal core to 9.4 or 9.5
composer require 'drupal/core-recommended:^9.5' 'drupal/core-composer-scaffold:^9.5' 'drupal/core-project-message:^9' --update-with-dependencies

# Update dependencies
composer update

# Rebuild caches and apply updates
drush cr
drush updb
```

---

## ⚠️ 3. Issue 1: Zend Feed Error

**Error:**
```
Class 'Zend\\Feed\\Reader\\Extension\\AbstractEntry' not found
```

**Cause:** Old Feeds module using deprecated Zend classes.

**Fix Options:**
1. Update Feeds module to a Drupal 9 compatible version:
   ```bash
   composer require drupal/feeds:^3.0 --update-with-dependencies
   ```
2. Or install Laminas bridge temporarily:
   ```bash
   composer require laminas/laminas-zendframework-bridge laminas/laminas-feed
   ```
3. Remove any `modules/feeds/src/Zend/` directories from old Feeds code.

---

## ⚙️ 4. Issue 2: Webprofiler Fatal Error

**Error:**
```
Class Drupal\\webprofiler\\Entity\\Decorators\\Config\\ConfigEntityStorageDecorator
must implement Drupal\\Core\\Entity\\EntityStorageInterface::getEntityClass
```

**Cause:** Old Webprofiler (Devel) module incompatible with Drupal 9.4+.

**Fix Options:**
1. Update Devel/Webprofiler:
   ```bash
   composer require drupal/devel:^5.1 --update-with-dependencies
   ```
2. Or disable Webprofiler completely:
   ```bash
   drush pmu webprofiler -y
   rm -rf modules/devel/webprofiler
   composer dump-autoload -o
   drush cr
   ```
3. Optional patch (temporary workaround):
   Add to `ConfigEntityStorageDecorator.php`:
   ```php
   public function getEntityClass($entity_type_id) {
       return $this->decorated->getEntityClass($entity_type_id);
   }
   ```

---

## 🗂️ 5. Issue 3: Config Sync Directory Missing

**Error:**
```
The config sync directory is not defined in $settings["config_sync_directory"]
```

**Fix:**
Add this line to `sites/default/settings.php`:

```php
$settings['config_sync_directory'] = '../config/sync';
```

Then create the directory if missing:
```bash
mkdir -p ../config/sync
chmod 755 ../config/sync
```

Re‑run:
```bash
drush status
drush updb
```

---

## 🎯 6. Post‑Upgrade Checklist (Drupal 9.5)

1. **Clear caches and update entities**
   ```bash
   drush cr
   drush updb
   drush entup -y
   ```
2. **Check contributed module compatibility**
   ```bash
   drush pm:list --type=module --status=enabled --no-core
   ```
3. **Scan for deprecated code**
   ```bash
   composer require drupal/deprecation_inspector --dev
   vendor/bin/deprecation-inspector scan web/modules/custom
   ```
4. **Verify PHP environment**
   - Use PHP 8.0 or 8.1
5. **Export configuration**
   ```bash
   drush cex -y
   ```
6. **Plan for next upgrade (Drupal 10)**

---

## ✅ 7. Final State

- Drupal Core: **9.5.x**
- Feeds: **3.x (Laminas‑based)**
- Webprofiler: **Updated or Removed**
- Config Sync Directory: **Defined and working**
- All caches clear and database schema up to date.

---

**Next step:** Prepare for Drupal 10 — much easier once you’re stable on 9.5!
