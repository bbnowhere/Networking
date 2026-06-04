If you're using **GAM 7.44.03**, here are some useful day-to-day commands for Google Workspace administration.

**Official documentation:**
[GAM Wiki](https://github.com/GAM-team/GAM/wiki?utm_source=chatgpt.com)

## User Management

### List all users

```bash
gam print users
```

### Get user information

```bash
gam info user user@domain.com
```

### Create a user

```bash
gam create user user@domain.com firstname John lastname Doe password Password123
```

### Suspend a user

```bash
gam update user user@domain.com suspended on
```

### Unsuspend a user

```bash
gam update user user@domain.com suspended off
```

### Delete a user

```bash
gam delete user user@domain.com
```

### Force password change

```bash
gam update user user@domain.com changepassword on
```

---

## Organizational Units (OU)

### List OUs

```bash
gam print orgs
```

### Move user to OU

```bash
gam update user user@domain.com org "/Employees"
```

### Print users in an OU

```bash
gam print users query "orgUnitPath='/Employees'"
```

---

## Group Management

### List all groups

```bash
gam print groups
```

### Group information

```bash
gam info group staff@domain.com
```

### Create a group

```bash
gam create group staff@domain.com name "Staff Group"
```

### Add member

```bash
gam update group staff@domain.com add member user@domain.com
```

### Add manager

```bash
gam update group staff@domain.com add manager user@domain.com
```

### Add owner

```bash
gam update group staff@domain.com add owner user@domain.com
```

### Remove member

```bash
gam update group staff@domain.com remove user user@domain.com
```

### List members

```bash
gam print group-members group staff@domain.com
```

([Google Groups][1])

### List nested group members

```bash
gam print group-members group staff@domain.com recursive
```

([Google Sites][2])

---

## Gmail

### Show Gmail signature

```bash
gam user user@domain.com show signature
```

### Update Gmail signature

```bash
gam user user@domain.com signature "<b>IT Team</b>"
```

### Show delegates

```bash
gam user user@domain.com show delegates
```

### Add delegate

```bash
gam user user@domain.com add delegate assistant@domain.com
```

### Remove delegate

```bash
gam user user@domain.com delete delegate assistant@domain.com
```

([Medium][3])

---

## Drive

### List files owned by a user

```bash
gam user user@domain.com show filelist
```

### Transfer Drive ownership

```bash
gam create datatransfer user old@domain.com gdrive new@domain.com
```

### List shared drives

```bash
gam print shareddrives
```

### Add manager to Shared Drive

```bash
gam user admin@domain.com add drivefileacl <driveid> user manager@domain.com role organizer
```

---

## ChromeOS Devices

### List ChromeOS devices

```bash
gam print cros
```

### Get device info

```bash
gam info cros <deviceid>
```

### Disable device

```bash
gam update cros <deviceid> action disable
```

---

## Reports & Audits

### Login activity

```bash
gam print logins
```

### Admin activity

```bash
gam print admin
```

### Drive activity

```bash
gam print driveactivity
```

### User last login

```bash
gam info user user@domain.com
```

---

## Calendar

### List calendars

```bash
gam user user@domain.com print calendars
```

### Add calendar access

```bash
gam user user@domain.com add calendaracl calendar@domain.com user other@domain.com role reader
```

---

## Bulk Operations with CSV

### Create users from CSV

```bash
gam csv users.csv gam create user ~email firstname ~firstname lastname ~lastname password ~password
```

### Suspend users from CSV

```bash
gam csv users.csv gam update user ~email suspended on
```

### Add users to a group

```bash
gam csv users.csv gam update group staff@domain.com add member ~email
```

---

## Security

### Sign out user from all sessions

```bash
gam user user@domain.com signout
```

### Turn off 2-Step Verification

```bash
gam user user@domain.com turnoff 2sv
```

### Delete application-specific passwords

```bash
gam user user@domain.com delete asps
```

---

## Useful Reporting Commands

### Users who never logged in

```bash
gam print users query "isSuspended=false"
```

### Suspended users

```bash
gam print users query "isSuspended=true"
```

### Users with admin privileges

```bash
gam print admins
```

### Groups and member counts

```bash
gam print groups members managers owners countsonly totalcount
```

([Google Sites][2])

[1]: https://groups.google.com/g/google-apps-manager/c/mtYrDUo0Gag?utm_source=chatgpt.com "how to list members of the group using gam"
[2]: https://sites.google.com/view/gam--commands/people/groups?utm_source=chatgpt.com "Groups - GAMADV-XTD3"
[3]: https://medium.com/neural-engineer/how-to-delegate-gmail-access-with-gam-48567e0b4b1f?utm_source=chatgpt.com "How to Delegate Gmail Access with GAM | by PI"
