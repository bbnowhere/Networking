Here’s your content converted into a clean, **GitHub-flavored Markdown (GFM)** format—properly structured, consistent, and ready to commit:

# Mock Interview – Tricky Scenarios

## ⚡ Scenario 1: Works in Dev, Breaks in Prod

**Question:**
Your feature works perfectly in Dev and Stage, but breaks in Production. What do you check?

**Strong Answer:**

* Check **configuration differences** (`config_split`, `settings.php`)
* Verify **environment variables**
* Check **caching differences (Varnish)**
* Compare:

  * Enabled modules
  * PHP versions
* Look at **Prod logs** (Acquia logs / watchdog)
* Check if **DB content differs**

> 💡 Bonus: Mention **read-only filesystem issues in Prod**

---

## ⚡ Scenario 2: Varnish Nightmare

**Question:**
A page shows outdated content even after clearing Drupal cache.

**Strong Answer:**

* Identify **Varnish caching issue**
* Check headers:

  * `X-Cache: HIT/MISS`
* Run:

  ```bash
  drush cr
  ```
* Purge Varnish:

  * Acquia UI / BAN request
* Ensure proper:

  * Cache tags
  * Cache contexts

> 💡 Strong candidates mention **cache invalidation strategy**

---

## ⚡ Scenario 3: Deployment Breaks Site

**Question:**
After deployment, site shows white screen (WSOD). What do you do?

**Strong Answer:**

* Check:

  * PHP errors (logs)
  * Missing dependencies (`composer install`)
* Run:

  ```bash
  drush updb
  drush cr
  ```
* Rollback code if needed
* Verify:

  * Config sync issues
  * Missing modules

> 💡 Bonus: Mention **fatal error due to class not found**

---

## ⚡ Scenario 4: Config Sync Conflict

**Question:**
Two developers push conflicting configs. Site breaks. Fix?

**Strong Answer:**

* Identify conflicting config (`core.extension`, views, fields)
* Run:

  ```bash
  drush config:status
  ```
* Resolve conflicts manually
* Re-export clean config:

  ```bash
  drush cex
  ```
* Enforce:

  * **Config freeze in Prod**

> 💡 Strong answer includes **Git conflict resolution strategy**

---

## ⚡ Scenario 5: DB Sync Disaster

**Question:**
A developer accidentally pushed Dev DB to Production.

**Strong Answer:**

* Immediately:

  * Stop writes
  * Inform team
* Restore:

  * From Acquia backup
* Review:

  * Access controls
* Prevent:

  * Restrict DB sync permissions

> 💡 Interviewers look for **calm + structured response**

---

## ⚡ Scenario 6: Slow Website (High Traffic)

**Question:**
Site becomes slow during traffic spike.

**Strong Answer:**

* Check:

  * Varnish hit rate
  * DB slow queries
* Enable:

  * Page cache
  * CDN
* Optimize:

  * Views queries
  * Uncached blocks
* Scale:

  * Acquia auto-scaling

> 💡 Mention **anonymous vs authenticated traffic**

---

## ⚡ Scenario 7: Cron Not Running

**Question:**
Cron jobs are not executing.

**Strong Answer:**

* Check:

  ```bash
  drush cron
  ```
* Verify:

  * Acquia scheduled cron settings
* Check:

  * Queue workers
* Inspect logs

> 💡 Bonus: Mention **queue backlog issues**

---

## ⚡ Scenario 8: External API Failure

**Question:**
Your site depends on an external API. It goes down.

**Strong Answer:**

* Implement:

  * Fallback mechanism
  * Cached responses
* Use:

  * Timeout handling
* Show:

  * Friendly error message
* Log failures

> 💡 Strong answer = **resilience design**

---

## ⚡ Scenario 9: Security Issue

**Question:**
A vulnerability is reported in a Drupal module.

**Strong Answer:**

* Immediately:

  * Check advisory
* Update:

  ```bash
  composer update
  ```
* Apply patch if needed
* Deploy quickly
* Inform stakeholders

> 💡 Mention **security SLA**

---

## ⚡ Scenario 10: Multi-site Issue (Acquia Site Factory)

**Question:**
One site works, another breaks in same codebase.

**Strong Answer:**

* Check:

  * Site-specific config
  * Domain mapping
* Inspect:

  * `settings.php` per site
* Verify:

  * DB differences

> 💡 Bonus: Mention **feature flags / config split**

---

## ⚡ Scenario 11: Content Editors Complain (Critical)

**Question:**
Editors say content disappears randomly.

**Strong Answer:**

* Check:

  * Revision system
  * Workflows / moderation
* Verify:

  * Permissions
* Look at:

  * Autosave / editorial workflows

> 💡 Strong answer shows **understanding of business users**

---

## ⚡ Scenario 12: Why Should We Use Acquia?

**Question:**
Why not AWS or normal hosting?

**Strong Answer:**

* Drupal-optimized platform
* Built-in:

  * CI/CD
  * Monitoring
  * Security
* Faster development lifecycle
* Enterprise support

---

## 🔷 Killer Closing Answer

**Question:** Tell me about a challenging issue you solved.

> “We had a production issue where pages were serving stale content due to Varnish caching. I analyzed cache headers, identified missing cache tags, implemented proper invalidation, and reduced stale content issues by 90%.”

---

## 🔥 Final Tip

Since you are:

* Senior Drupal Developer
* Enterprise Architect

Always include:

* **Root cause analysis**
* **Prevention strategy**
* **Business impact**
