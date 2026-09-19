# ☁️ CloudVault

### Secure file storage and management for teams.

CloudVault is a **database-driven file storage and management system** built for students, faculty, and project teams who work with shared files.

Instead of scattering files across different platforms and devices, CloudVault brings them together in one place — with **folders, sharing, permissions, version history, tags, search, and activity tracking**.

> Built as a DBMS project using **MySQL**.

---

## 👥 Team D15

<table>
  <tr>
    <th colspan="2">☁️ CLOUDVAULT — TEAM D15</th>
  </tr>
  <tr>
    <td><b>👤 Member</b></td>
    <td><b>Roll.Number</b></td>
  </tr>
  <tr>
    <td>Patteboyina Yadunandan</td>
    <td>AM.SC.U4CSE25341</td>
  </tr>
  <tr>
    <td>Karan Sanjay</td>
    <td>AM.SC.U4CSE25325</td>
  </tr>
  <tr>
    <td>Shijakeet Mukharjee</td>
    <td>AM.SC.U4CSE25345</td>
  </tr>
  <tr>
    <td>Sreedev Nair</td>
    <td>AM.SC.U4CSE25351</td>
  </tr>
</table>

## ✨ Features

* 📁 **File & Folder Management** — Organize files in one place
* 🔗 **File Sharing** — Share files with users or groups
* 🔐 **Access Control** — Manage who can access files
* 🕐 **Version History** — Track and restore previous versions
* 🏷️ **File Tagging** — Categorize files for easier organization
* 🔎 **File Search** — Quickly find stored files
* ♻️ **Deleted File Recovery** — Recover deleted files
* 📊 **Storage Tracking** — Monitor storage usage
* 🔗 **Controlled Sharing Links** — Set expiry dates and download limits
* 🧾 **Activity Tracking** — Record important file operations
* ♻️ **Duplicate Detection** — Use file checksums to identify duplicates

---

## 🗄️ Database Driven

The core of CloudVault is its **relational database**.

**MySQL** stores the metadata and relationships behind the files, while the actual uploaded files are stored separately in object storage.

```text
                    CloudVault
                        │
             ┌──────────┴──────────┐
             │                     │
             ▼                     ▼
       MySQL Database        Object Storage
             │                     │
       ┌─────┴─────┐               │
       │           │               │
     Users       Files        Actual Files
     Groups      Versions
     Permissions Tags
     Sharing     Activity
```

The database connects users, files, groups, permissions, versions, tags, and sharing information using **primary keys, foreign keys, and relational tables**.

---

## 🧩 Core Modules

| Module             | Purpose                           |
| ------------------ | --------------------------------- |
| 👤 Users & Groups  | Manage users and group membership |
| 📁 Files & Folders | Organize stored files             |
| 🔐 Permissions     | Control file access               |
| 🔗 Sharing         | Share files with users or groups  |
| 🕐 Version History | Track and restore file versions   |
| 🏷️ Tags           | Categorize files                  |
| 🔎 Search          | Find files quickly                |
| ♻️ Recovery        | Recover deleted files             |
| 📊 Storage         | Track storage usage               |
| 🧾 Activity Log    | Record important operations       |

---

## 🔄 How It Works

```text
        Upload File
             │
             ▼
      ┌──────────────┐
      │  CloudVault  │
      └──────┬───────┘
             │
       ┌─────┴─────┐
       ▼           ▼
    MySQL      Object Storage
   Metadata       File
       │
       ▼
 Permissions / Sharing / Versions
       │
       ▼
    Activity Log
```

The database manages the **structured information and relationships**, while object storage handles the actual file contents.

---

## 🛠️ Tech Stack

| Layer           | Technology                       |
| --------------- | -------------------------------- |
| 🗄️ Database    | MySQL                            |
| ☁️ File Storage | Object Storage                   |
| 🌐 Application  | Web-based File Management System |

---

## 🎯 Project Objective

CloudVault aims to solve common file-management problems such as:

* Duplicate files
* Unclear file versions
* Accidental data loss
* Uncontrolled file sharing
* Difficulty tracking file activity

At the same time, the project demonstrates how **relational database concepts can be applied to a practical real-world system**.

---

**23CSE202 — Database Management Systems**
*Amrita School of Computing, Amritapuri*

---

<p align="center">

### ☁️ CloudVault

**Store. Share. Organize.**

</p>
