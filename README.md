# ☁️ CloudVault

### A Database-Driven Secure File Storage & Management System

CloudVault is a **database-driven file storage and management platform** designed to provide secure, organized, and controlled management of digital files.

The system combines **file storage with a relational database backend** to manage users, files, folders, access permissions, file versions, sharing, approvals, and activity records.

> **23CSE202 — Database Management Systems Project**

---

## ✨ Overview

Managing files across academic and collaborative environments can become difficult when there is no centralized system for controlling access, tracking changes, and maintaining file history.

**CloudVault** addresses this by providing a centralized platform where users can upload, organize, share, and manage files while the database maintains the metadata and relationships required to keep the system consistent and auditable.

The project focuses particularly on the **DBMS layer**, using structured relational data to handle users, files, groups, permissions, versions, sharing, and activity logs.

---

## 🚀 Features

### 👤 User Management

* User registration and authentication
* User profiles
* Role-based access control
* Controlled access to resources

### 📁 File Management

* Upload and manage files
* File and folder organization
* File metadata management
* File size and type tracking
* File search and retrieval

### 🔐 Access Control

* Role-based permissions
* Group-based access
* Controlled file sharing
* Permission management
* Restricted access to unauthorized users

### 🕒 File Versioning

* Maintain multiple versions of files
* Track changes between versions
* Preserve previous file states
* Restore or access earlier versions

### ✅ Approval Workflow

* Controlled file approval process
* Track approval status
* Record actions performed during the workflow

### 📊 Activity Auditing

* Track important user activities
* Record file operations
* Maintain an activity history
* Improve accountability and traceability

### 🗄️ Database Management

The relational database stores and manages important metadata including:

* Users
* Files
* Folders
* Groups
* Roles
* Permissions
* File versions
* Sharing information
* Approval records
* Activity logs

---

## 🏗️ System Architecture

CloudVault follows a layered architecture where the **database acts as the central source of structured metadata**.

```text
                    ┌──────────────────────┐
                    │      User / Client    │
                    └──────────┬───────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │   Application Layer  │
                    │                      │
                    │ Authentication       │
                    │ File Management      │
                    │ Access Control        │
                    │ Sharing & Approval    │
                    │ Activity Tracking     │
                    └──────────┬───────────┘
                               │
                    ┌──────────┴───────────┐
                    ▼                      ▼
          ┌──────────────────┐    ┌──────────────────┐
          │ Relational       │    │ File/Object      │
          │ Database         │    │ Storage          │
          │                  │    │                  │
          │ Users            │    │ Actual files     │
          │ Files            │    │ File contents    │
          │ Permissions      │    │                  │
          │ Versions         │    └──────────────────┘
          │ Groups           │
          │ Audit Logs       │
          └──────────────────┘
```

The database primarily handles **structured metadata and relationships**, while the actual file contents can be maintained separately from the relational data.

---

## 🗃️ Database Design

A major objective of CloudVault is to demonstrate how a relational database can be used to build a structured file-management system.

The database models relationships between entities such as:

```text
User
 │
 ├── owns ───────────────► File
 │                         │
 │                         ├── has ──► File Version
 │                         │
 │                         └── belongs to ──► Folder
 │
 ├── belongs to ─────────► Group
 │                         │
 │                         └── has ──► Permissions
 │
 ├── creates ────────────► Share
 │
 └── performs ───────────► Activity
```

### Core Database Concepts Demonstrated

* Primary Keys
* Foreign Keys
* Entity Relationships
* Referential Integrity
* Normalization
* One-to-One Relationships
* One-to-Many Relationships
* Many-to-Many Relationships
* Constraints
* Transactions
* Querying and filtering
* Audit logging

---

## 🔑 Access Control Model

CloudVault uses a combination of **role-based and group-based access control**.

A simplified model is:

```text
User
 │
 ├──────────► Role
 │              │
 │              └──► Permissions
 │
 └──────────► Group
                │
                └──► Group Permissions
                         │
                         ▼
                       Files
```

This allows access to be controlled without assigning every permission individually to every user.

---

## 🔄 File Versioning

Instead of overwriting a file's history, CloudVault can maintain multiple versions associated with the same logical file.

```text
                    File
                     │
        ┌────────────┼────────────┐
        ▼            ▼            ▼
    Version 1    Version 2    Version 3
      │              │            │
    Older         Modified      Current
```

This provides a structured way to maintain file history and supports recovery of previous versions.

---

## 📝 Activity & Audit Logging

Important operations can be recorded in an activity log.

Example:

```text
User: John
Action: FILE_UPLOAD
File: project_report.pdf
Timestamp: 2026-09-19 10:42:18
```

Possible activities include:

* Login
* File upload
* File download
* File update
* File deletion
* File sharing
* Permission changes
* Approval actions

This creates an auditable record of activity within the system.

---

## 🎨 Design

CloudVault follows a **modern-minimal dashboard design** focused on readability and information density.

### Design characteristics

* Light and dark themes
* Cobalt-based accent system
* Responsive layouts
* Minimal visual clutter
* Dashboard-oriented interface
* Consistent typography
* Accessible contrast
* Responsive navigation

The interface uses:

| Purpose            | Technology     |
| ------------------ | -------------- |
| Display typography | Space Grotesk  |
| UI/body text       | IBM Plex Sans  |
| Technical data     | JetBrains Mono |

The design system also defines reusable spacing, typography, color, elevation, responsive behavior, and interaction rules.

---

## 🛠️ Technology Stack

> Update this section if your final implementation uses different technologies.

### Frontend

* HTML
* CSS
* JavaScript

### Backend

* Application server / backend logic

### Database

* **MySQL**

### Storage

* File/object storage for actual file contents

### Development

* Git
* GitHub

---

## 📂 Project Structure

```text
CloudVault/
│
├── app/
│   ├── css/
│   ├── js/
│   └── ...
│
├── database/
│   ├── schema/
│   ├── queries/
│   └── ...
│
├── design.md
├── README.md
└── CloudVault.zip
```

> The exact structure may vary depending on the current implementation.

---

## ⚙️ Getting Started

### 1. Clone the Repository

```bash
git clone https://github.com/Y4DUN4ND4N/CloudVault.git
cd CloudVault
```

### 2. Set Up the Database

Install **MySQL** and create a database for CloudVault.

```sql
CREATE DATABASE cloudvault;
```

Import the project's database schema and required SQL files.

### 3. Configure the Application

Configure the database connection using your local MySQL credentials.

Example:

```text
Database Host: localhost
Database Port: 3306
Database Name: cloudvault
Database User: your_username
Database Password: your_password
```

### 4. Start the Application

Start the backend/application server according to the project's configured environment.

Then open the application in your browser.

---

## 🔒 Security Considerations

CloudVault is designed around controlled access to stored information.

Important security considerations include:

* Authentication before accessing protected resources
* Authorization checks for file operations
* Role-based permissions
* Group-based access control
* Controlled file sharing
* Activity logging
* Secure database queries
* Input validation
* Protection against unauthorized file access

> **Note:** CloudVault is an academic project and should not be considered production-ready security software without additional security testing and hardening.

---

## 🎯 Project Objectives

The primary objectives of CloudVault are:

1. Design a centralized file-management system.
2. Apply relational database concepts to a real-world application.
3. Implement structured user and access management.
4. Maintain file metadata using a relational database.
5. Demonstrate relationships between users, files, groups, permissions, and versions.
6. Implement file version tracking.
7. Provide controlled file-sharing workflows.
8. Maintain an auditable record of important system activities.
9. Develop a responsive and usable management interface.

---

## 📚 DBMS Concepts Used

CloudVault demonstrates several important DBMS concepts:

```text
                    ┌────────────────────┐
                    │    CloudVault DB   │
                    └─────────┬──────────┘
                              │
       ┌──────────────────────┼──────────────────────┐
       ▼                      ▼                      ▼
  Data Modeling         Normalization          Relationships
       │                      │                      │
       ▼                      ▼                      ▼
  ER Diagram            Reducing Redundancy    PK / FK
       │                                             │
       └──────────────────┬──────────────────────────┘
                          ▼
                  Integrity Constraints
                          │
                          ▼
                     Transactions
                          │
                          ▼
                     Audit Logging
```

---

## 🌟 Why CloudVault?

CloudVault demonstrates how a traditional DBMS can be applied to a practical software system rather than being limited to simple CRUD operations.

The project combines:

**Database Design + File Management + Access Control + Versioning + Auditing**

into a single application.

---

## 🔮 Future Enhancements

Potential future improvements include:

* End-to-end file encryption
* Cloud object-storage integration
* Advanced search and filtering
* File sharing through secure links
* Email notifications
* Two-factor authentication
* Fine-grained permission policies
* Database backup and recovery
* File integrity verification using checksums
* Real-time collaboration
* Improved administrative analytics
* API-based integrations

---

## 👥 Project

**CloudVault — Database-Driven Secure File Storage & Management System**

**Course:** 23CSE202 — Database Management Systems

**Repository:**
[GitHub — Y4DUN4ND4N/CloudVault](https://github.com/Y4DUN4ND4N/CloudVault?utm_source=chatgpt.com)

---

## 📄 License

This project was developed for academic purposes as part of the **23CSE202 DBMS Project**.

---

<p align="center">
  <b>CloudVault</b><br>
  Secure. Organized. Controlled.
</p>
