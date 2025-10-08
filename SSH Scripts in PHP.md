# Drupal 11: Mailing List Subscription via SSH

## Overview

This document details how to enable a Drupal 11 plugin to subscribe users to mailing lists using a **remote Mailman server** (`it@lists`) via SSH, including lessons learned, issues, and solutions.

---

## 1. Problem Description

* Goal: Subscribe the `final_email` of a Drupal node to Mailman mailing lists (`all`, `community`) automatically after a certain moderation state (`profile_published`).
* Issue: Running `shell_exec()` in Drupal to execute the remote command failed:

  ```
  Failed to execute mailing list subscription command for test@ext.example.res.in to all
  ```
* Observations:

  * `php -r "var_dump(function_exists('shell_exec'));"` → `bool(true)` → `shell_exec()` is enabled.
  * The same command works **manually in terminal** but fails when run by Apache user from Drupal.

---

## 2. Key Causes Identified

1. **User context mismatch**

   * Drupal runs PHP under `apache` user.
   * `apache` user did not have SSH key-based access to `it@lists`.

2. **Passwordless sudo requirement**

   * `sudo /usr/lib/mailman/bin/add_members` requires `NOPASSWD` for non-interactive execution.

3. **SSH known_hosts issues**

   * `apache` home directory (`/usr/share/httpd`) doesn’t exist or isn’t writable.
   * Host key verification blocks SSH execution.

4. **Key generation failures**

   * `sudo -u apache ssh-keygen` failed because `/var/www/.ssh` did not exist.

---

## 3. Steps to Fix Issues

### 3.1 Generate SSH Key for `apache`

```bash
sudo mkdir -p /var/www/.ssh
sudo chown apache:apache /var/www/.ssh
sudo chmod 700 /var/www/.ssh

sudo -u apache ssh-keygen -t ed25519 -f /var/www/.ssh/id_ed25519 -N ""
```

* **Key files**:

  * Private key: `/var/www/.ssh/id_ed25519`
  * Public key: `/var/www/.ssh/id_ed25519.pub`

---

### 3.2 Copy Public Key to `it@lists`

**On `lists` server**:

```bash
sudo -u it mkdir -p ~/.ssh
sudo chmod 700 ~/.ssh

# Append the public key from Drupal server
echo "ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIKmEudEWge7qJR8AcW9s1iTFyGzdB4kyFCRyj5BBSapZ apache@priweb2.example.res.in" >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
chown it:it ~/.ssh/authorized_keys
```

* Ensures `apache@priweb2` can SSH into `it@lists` **without password**.

---

### 3.3 Passwordless sudo for `add_members`

**On `lists` server:**

```bash
sudo visudo
```

Add:

```
it ALL=(ALL) NOPASSWD: /usr/lib/mailman/bin/add_members
```

* Allows `sudo /usr/lib/mailman/bin/add_members` to run non-interactively.

---

### 3.4 Configure Known Hosts for Apache User

```bash
sudo touch /var/www/.ssh/known_hosts
sudo chown apache:apache /var/www/.ssh/known_hosts
chmod 600 /var/www/.ssh/known_hosts
```

* Use in SSH command:

  ```bash
  -o UserKnownHostsFile=/var/www/.ssh/known_hosts
  -o StrictHostKeyChecking=no
  ```

---

### 3.5 Test SSH Manually

```bash
sudo -u apache ssh -i /var/www/.ssh/id_ed25519 -o StrictHostKeyChecking=no -o UserKnownHostsFile=/var/www/.ssh/known_hosts it@lists "echo test"
```

* Should print `test` **without asking for password**. ✅

---

## 4. SubscribeToMailingLists Function

### Updated PHP Function (Drupal 11)

```php
protected function subscribeToMailingLists(string $final_email): void {
  if (empty($final_email)) {
    \Drupal::logger('itservices')->warning('Cannot subscribe to mailing lists: final_email is empty.');
    return;
  }

  if (!filter_var($final_email, FILTER_VALIDATE_EMAIL)) {
    \Drupal::logger('itservices')->error('Invalid final_email format for mailing list subscription: @email', [
      '@email' => $final_email,
    ]);
    return;
  }

  $lists = ['all', 'community'];
  $ssh_path = '/usr/bin/ssh';
  $private_key = '/var/www/.ssh/id_ed25519';
  $known_hosts = '/var/www/.ssh/known_hosts';

  foreach ($lists as $list) {
    $remoteCmd = "echo '$final_email' | sudo /usr/lib/mailman/bin/add_members -r - $list";
    $sshCmd = "$ssh_path -i $private_key -o StrictHostKeyChecking=no -o UserKnownHostsFile=$known_hosts it@lists " . escapeshellarg($remoteCmd);

    // Capture output & exit code
    $output = shell_exec($sshCmd . " 2>&1; echo ExitCode:$?");

    if ($output === null) {
      \Drupal::logger('itservices')->error('Failed to execute mailing list subscription command for @email to @list', [
        '@email' => $final_email,
        '@list' => $list,
      ]);
    } else {
      \Drupal::logger('itservices')->notice('Mailing list subscription output for @email to @list: @output', [
        '@email' => $final_email,
        '@list' => $list,
        '@output' => trim($output),
      ]);
    }
  }

  \Drupal::logger('itservices')->notice('Mailing list subscription process completed for @email', [
    '@email' => $final_email,
  ]);
}
```

---

## 5. Lessons Learned

1. **`shell_exec()` may fail silently** if:

   * SSH authentication fails
   * Non-interactive sudo prompts for password
   * Apache user cannot write to `.ssh` or known_hosts

2. **Apache user (`www-data` / `apache`) does not have a home directory** writable by default.

   * Always use a folder like `/var/www/.ssh` for key storage.

3. **Passwordless sudo is essential** for non-interactive commands requiring elevated privileges.

4. **Always specify SSH options** in automated scripts:

   * `-i` for private key
   * `-o StrictHostKeyChecking=no`
   * `-o UserKnownHostsFile=/path/to/writable/known_hosts`

5. **Testing manually** with `sudo -u apache ssh ...` is crucial before integrating with Drupal.

6. **Logging stdout, stderr, and exit codes** in Drupal helps debug `shell_exec()` failures.

---

## 6. References

* [Mailman `add_members` documentation](https://www.gnu.org/software/mailman/)
* [Drupal 11: shell_exec() & Logging](https://www.drupal.org/docs)
* [SSH Key Authentication Best Practices](https://www.ssh.com/ssh/keygen/)

---

✅ After following this guide, your Drupal plugin can reliably subscribe `final_email` addresses to mailing lists without requiring passwords.

