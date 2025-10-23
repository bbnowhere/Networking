# **Comprehensive Report –  WordPress Site**

---

## 1️⃣ **Pages and Content**

| Type                                               | Count           | Notes                                                        |
| -------------------------------------------------- | --------------- | ------------------------------------------------------------ |
| **Static Pages** (`wp_posts` → `post_type='page'`) | 87              | Published pages like About, Contact, etc.                    |
| **Posts** (`wp_posts` → `post_type='post'`)        | 0               | No standard blog posts exist.                                |
| **Terms / Taxonomies** (`wp_terms`)                | 2631            | Includes categories, tags, nav menus, and custom taxonomies. |
| **Taxonomies Used**                                | See below table |                                                              |

**Taxonomy Summary**

| Taxonomy Name       | Term Count | Used Term Count |
| ------------------- | ---------- | --------------- |
| post_tag            | 2583       | 1551            |
| category            | 21         | 20              |
| nav_menu            | 10         | 10              |
| news_category       | 8          | 4               |
| people_category     | 5          | 4               |
| event_categories    | 3          | 3               |
| wp_pattern_category | 1          | -               |

**Dynamic Pages**: ~1,585 (terms actively used in content)

---

## 2️⃣ **Fields Available for User Input**

| Source                                    | Count | Notes                                                  |
| ----------------------------------------- | ----- | ------------------------------------------------------ |
| `wp_postmeta` (custom fields / post meta) | 178   | Across posts, pages, and custom post types.            |
| User profile fields (`wp_usermeta`)       | N/A   | Could be counted separately (default + custom fields). |
| ACF Field Groups (`acf-field`)            | N/A   | If ACF used, check `wp_posts.post_type='acf-field'`.   |

> **Note:** 178 fields include meta fields added by plugins or themes for posts/pages.

---

## 3️⃣ **Forms Available for User Input**

| File / Location       | Type              | Notes                                    |
| --------------------- | ----------------- | ---------------------------------------- |
| `page-pubmed.php`     | Custom-coded form | “Get in Touch” or PubMed submission form |
| `page-pubmed-old.php` | Custom-coded form | Older version of submission form         |
| `header.php`          | Search form       | `<form role="search">`                   |
| `search.php`          | Search form       | `<form role="search">`                   |

> **Observation:** No plugin-based forms (Contact Form 7, WPForms, Gravity Forms, etc.) were detected.
> Input fields can be counted manually or via PHP DOM parsing.

**Example PHP script to count input fields:**

```php
$doc = new DOMDocument();
libxml_use_internal_errors(true);
$html = file_get_contents('path-to-form-file.php');
$doc->loadHTML($html);
$inputs = $doc->getElementsByTagName('input');
$textareas = $doc->getElementsByTagName('textarea');
$selects = $doc->getElementsByTagName('select');
$total = $inputs->length + $textareas->length + $selects->length;
echo "Total user input fields: $total";
```

---

## 4️⃣ **User Roles**

| Source                    | Count | Notes                                                            |
| ------------------------- | ----- | ---------------------------------------------------------------- |
| WordPress `wp_user_roles` | N/A   | Serialized in `wp_options.option_name='wp_user_roles'`           |
| Default WordPress roles   | 5     | `administrator`, `editor`, `author`, `contributor`, `subscriber` |
| Custom roles              | N/A   | Any plugins may add roles like `manager`, etc.                   |

**PHP snippet to list roles:**

```php
$roles = maybe_unserialize(get_option('wp_user_roles'));
echo "Number of roles: " . count($roles);
echo "Roles: " . implode(', ', array_keys($roles));
```

---

## 5️⃣ **Server-Side Scripts / Languages**

**From HTTP headers:**

```
Server: Apache/2.4.62 (Rocky Linux) OpenSSL/3.2.2
X-Powered-By: PHP/8.0.30
Content-Type: text/html; charset=UTF-8
```

**Conclusion:**

| Attribute            | Value         | Notes                                 |
| -------------------- | ------------- | ------------------------------------- |
| Web Server           | Apache 2.4.62 | Running on Rocky Linux                |
| Server-side Language | PHP 8.0.30    | WordPress core is PHP-based           |
| Other Languages      | None detected | No ASP, JSP, Python, or Ruby detected |

**File-based scan:** You can check `/var/www/html` for `.php`, `.py`, `.jsp`, `.asp` to verify other server-side scripts.

---

## 6️⃣ **Directory Sizes**

* `ls -lSh` reports **directory entry size**, not contents (so `4.0K` is normal for a directory).
* Correct command to check actual size:

```bash
du -sh /var/www/html/instem
```

* To sort all directories by size:

```bash
du -sh * | sort -hr
```

---

## 7️⃣ **Notes / Observations**

1. **Static vs Dynamic Content**: The site heavily relies on static pages (87) and dynamic taxonomy pages (~1,585).
2. **Forms**: “Get in Touch” forms are **custom-coded**, not plugin-based. Counting input fields requires inspecting HTML/PHP.
3. **User Input Fields**: 178 custom fields across posts/pages; more may exist in user meta or custom plugins.
4. **Server Environment**: Apache + PHP 8.0 on Rocky Linux; no other server-side languages detected.
5. **Roles**: Default WordPress roles exist; any custom roles would be visible in `wp_user_roles`.
6. **Directory Sizes**: `ls` shows only directory entry size; use `du -sh` to check real disk usage.
7. **Dynamic Pages / Taxonomies**: Multiple custom taxonomies (`news_category`, `people_category`, `event_categories`) exist, forming dynamic archive pages.

---

### ✅ **Summary Table**

| Metric                          | Count / Info                             |
| ------------------------------- | ---------------------------------------- |
| Static pages                    | 87                                       |
| Posts                           | 0                                        |
| Dynamic pages (taxonomy-driven) | ~1,585                                   |
| Custom fields (post meta)       | 178                                      |
| Forms (plugin-based)            | 0                                        |
| Forms (custom-coded)            | 2–3 (Get in Touch, PubMed forms)         |
| User roles                      | 5 default + possible custom roles        |
| Server-side language            | PHP 8.0.30                               |
| Web server                      | Apache 2.4.62                            |
| OS                              | Rocky Linux                              |
| Theme directory size            | Check with `du -sh /var/www/html/instem` |

---

If you want, I can **also make a visual diagram/report** showing:

* Static vs dynamic pages
* Custom fields vs forms
* Roles
* Server setup

This can be shared as a **one-page site audit**.

Do you want me to do that?
