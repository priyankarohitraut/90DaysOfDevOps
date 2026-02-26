
---

## Files & Directories Created

### Files
- devops-file.txt
- team-notes.txt
- project-config.yaml
- heist-project/vault/gold.txt
- heist-project/plans/strategy.conf
- bank-heist/access-codes.txt
- bank-heist/blueprints.pdf
- bank-heist/escape-plan.txt

### Directories
- app-logs/
- heist-project/
- heist-project/vault/
- heist-project/plans/
- bank-heist/

---

## Task 1 – Understanding File Ownership

I ran:
```bash
ls -l
````

Output format:

```
-rw-r--r-- 1 owner group size date filename
```

What I understood:

* Owner → The main user who owns the file.
* Group → A group of users who can also access the file.
* Permissions are based on owner, group, and others.

Example:

```
-rw-r--r-- 1 priyanka priyanka 0 Feb 26 devops-file.txt
```

Here:
Owner = priyanka
Group = priyanka

Difference:
Owner is a single user.
Group can contain multiple users.

---

## Task 2 – Basic chown Practice

Created file:

```bash
touch devops-file.txt
```

Checked owner:

```bash
ls -l devops-file.txt
```

Created users:

```bash
sudo useradd tokyo
sudo useradd berlin
```

Changed owner:

```bash
sudo chown tokyo devops-file.txt
sudo chown berlin devops-file.txt
```

After change:

* devops-file.txt → berlin:priyanka

I verified again using:

```bash
ls -l devops-file.txt
```

---

## Task 3 – Basic chgrp Practice

Created file:

```bash
touch team-notes.txt
```

Created group:

```bash
sudo groupadd heist-team
```

Changed group:

```bash
sudo chgrp heist-team team-notes.txt
```

After change:

* team-notes.txt → priyanka:heist-team

---

## Task 4 – Change Owner and Group Together

Created file:

```bash
touch project-config.yaml
```

Created user:

```bash
sudo useradd professor
```

Changed both owner and group in one command:

```bash
sudo chown professor:heist-team project-config.yaml
```

Created directory:

```bash
mkdir app-logs
```

Changed ownership:

```bash
sudo chown berlin:heist-team app-logs
```

This helped me understand that we can change user and group together using:

```
sudo chown user:group filename
```

---

## Task 5 – Recursive Ownership Change

Created structure:

```bash
mkdir -p heist-project/vault
mkdir -p heist-project/plans
touch heist-project/vault/gold.txt
touch heist-project/plans/strategy.conf
```

Created group:

```bash
sudo groupadd planners
```

Changed complete directory ownership:

```bash
sudo chown -R professor:planners heist-project/
```

Verified:

```bash
ls -lR heist-project/
```

All files and folders changed successfully.

The `-R` option means Recursive (applies to all subfolders and files).

---

## Task 6 – Practice Challenge

Created users:

```bash
sudo useradd nairobi
```

Created groups:

```bash
sudo groupadd vault-team
sudo groupadd tech-team
```

Created directory and files:

```bash
mkdir bank-heist
touch bank-heist/access-codes.txt
touch bank-heist/blueprints.pdf
touch bank-heist/escape-plan.txt
```

Set different ownership:

```bash
sudo chown tokyo:vault-team bank-heist/access-codes.txt
sudo chown berlin:tech-team bank-heist/blueprints.pdf
sudo chown nairobi:vault-team bank-heist/escape-plan.txt
```

Verified:

```bash
ls -l bank-heist/
```

Everything was changed correctly.

---

## Commands I Learned

* ls -l → Check ownership
* chown → Change owner
* chgrp → Change group
* chown user:group → Change both
* chown -R → Recursive ownership change

---
