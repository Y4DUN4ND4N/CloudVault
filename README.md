# ☁️ CloudVault

### Secure file storage and management for teams
**Store. Share. Organize.**

## 👥 Team

| Member | Roll number |
| --- | --- |
| Patteboyina Yadunandan | AM.SC.U4CSE25341 |
| Karan Sanjay | AM.SC.U4CSE25325 |
| Shijakeet Mukharjee | AM.SC.U4CSE25345 |
| Sreedev Nair | AM.SC.U4CSE25351 |


CloudVault is a web-based file management system built for students, faculty, and project teams. It combines a searchable file library with folders, version history, tags, individual and group permissions, public share links, recoverable deletion, storage quotas, and activity tracking.

The project demonstrates relational database design with **MySQL**. A **Flask** backend enforces application rules, while file contents are stored separately using local content-addressed storage or an optional S3-compatible object store.

> **Current demo setup:** SQLite + local file storage. MySQL and S3 are supported configuration options, but require their own services and credentials.

---

## ✨ Features

- **File and folder management** — Upload, download, rename, move, copy, and organise files in nested folders.
- **Version history** — Keep sequential file versions, download historical versions, and restore an older version as a new current version.
- **Individual permissions** — Share files with specific users as Editors or Viewers.
- **Groups** — Create groups, manage membership, and share files with group members.
- **Controlled public links** — Create revocable links with optional expiry dates and maximum download counts.
- **Tags and search** — Add reusable tags and search accessible files by filename, owner, or tag.
- **Deleted-file recovery** — Soft-delete files, restore them, or permanently delete them from the Deleted view.
- **Duplicate detection** — Use SHA-256 checksums to identify identical content and reuse stored objects.
- **Storage quotas** — Track each user's logical storage usage and enforce quota limits.
- **Activity history** — Record uploads, sharing, modifications, deletion, restoration, and downloads.
- **Admin controls and reports** — View usage and activity summaries, manage user roles, and adjust storage quotas.
- **Responsive interface** — Dashboard, reports/groups, admin page, and light/dark theme support.

## 🧰 Tech stack

| Layer | Technology |
| --- | --- |
| Frontend | HTML, CSS, JavaScript |
| Backend/API | Python, Flask |
| Database | MySQL schema; SQLite for local demo |
| File content storage | Local filesystem or optional S3-compatible object storage |
| Object storage SDK | boto3 |
| MySQL driver | PyMySQL |
| Testing | Python `unittest`; JavaScript syntax checks with Node.js |

---

## 🏗️ Architecture

```text
                 Browser UI
          HTML / CSS / JavaScript
                     |
                     v
                Flask API
                     |
          +----------+-----------+
          |                      |
          v                      v
    Relational database     Content storage
     MySQL / SQLite          Local / S3
          |                      |
          v                      v
 Users, groups, files,      Actual file bytes
 versions, permissions,
 tags, share links, logs
```

The database stores structured metadata and relationships. File contents are stored separately and referenced by storage paths. SHA-256 checksums are used as content keys, allowing identical bytes to reuse the same stored object while maintaining separate file and version records.

The relational design includes twelve tables:

`users`, `user_groups`, `group_members`, `folders`, `files`, `file_versions`, `tags`, `file_tags`, `file_user_permissions`, `file_group_permissions`, `share_links`, and `activity_logs`.

Primary keys, foreign keys, composite keys, and unique constraints maintain relationships and prevent duplicate records where required.

---

## 🚀 Run locally

### Requirements

- Python 3.10 or newer
- `pip` and Python virtual-environment support
- A modern web browser
- Node.js only if you want to run the JavaScript syntax checks

### Linux / macOS

From the extracted `CloudVault` directory:

```bash
python3 -m venv server/venv
server/venv/bin/python -m pip install -r server/requirements.txt
server/venv/bin/python server/app.py
```

### Windows PowerShell

```powershell
py -m venv server/venv
server\venv\Scripts\python.exe -m pip install -r server\requirements.txt
server\venv\Scripts\python.exe server\app.py
```

Then open **http://127.0.0.1:5050** in your browser. Keep the terminal open while using the application and stop the server with `Ctrl+C`.

Serve the frontend through Flask; opening the HTML files directly will not connect them to the API. No frontend build step or `npm install` is required.

### Demo accounts

The bundled SQLite demo database is intended to start with these accounts:

| Account | Email | Password | Role |
| --- | --- | --- | --- |
| Test | `test@college.edu` | `demo` | Student |
| Admin | `admin@college.edu` | `demo` | Admin |

These are demonstration credentials only. Passwords are stored as hashes. Register additional Student or Faculty accounts from the login page. Admin role assignment must be performed by an existing administrator or through an authorised database connection.

---

## 🔐 Roles and permissions

| Role | Capabilities |
| --- | --- |
| **Owner** | Full control of their file, including sharing, links, version management, rename/move, tags, deletion, recovery, and permanent deletion. |
| **Editor** | Download and copy files, upload/restore versions, rename files, and edit tags. Cannot manage sharing or delete the source file. |
| **Viewer** | Download current and historical versions and make an independent copy. Cannot modify the source file. |
| **Admin** | View system-wide reports and manage account roles and storage quotas. Admin status does not automatically grant ownership or editing access to other users' files. |

Permissions are checked by the server, not just by the interface. Access can be granted directly to a user or through a group. A file remains owned by an individual user; groups provide shared access rather than owning files.

---

## 🗂️ Main application areas

- **Dashboard / My Files** — Browse your files and folders, upload content, and manage your library.
- **Shared with me / Shared by me** — Find files shared with you or inspect files you have shared.
- **Tagged** — Browse accessible files that have tags.
- **Deleted** — Restore soft-deleted files or permanently remove them.
- **File details** — View Versions, Access, Share links, and Activity.
- **Reports & Groups** — Review usage, duplicate savings, links, activity, and groups; manage group membership.
- **Admin** — Review system totals and update account roles or quotas.

---

## 🧪 Tests

Run the backend tests from the extracted project directory:

```bash
python -m unittest discover -s server/tests -v
```

If you created a virtual environment, use its Python executable instead. For example:

```bash
server/venv/bin/python -m unittest discover -s server/tests -v
```

Optional JavaScript syntax checks:

```bash
node --check app/js/db.js
node --check app/js/dashboard.js
node --check app/js/reports.js
```

The tests cover access-control rules, folder ownership, group sharing and revocation, tags, versioning and restoration, duplicate content reuse, quotas, deleted-file recovery, share-link expiry/download limits, admin validation, password migration, and S3 storage behavior. S3 behavior is tested with SDK stubs; this does not mean a live S3 bucket is configured.

---

## ⚙️ Optional MySQL backend

The project includes `server/schema.mysql.sql` for MySQL and `server/schema.sql` for the SQLite demo.

Create a database and apply the MySQL schema:

```bash
mysql -u your_user -p -e "CREATE DATABASE cloudvault CHARACTER SET utf8mb4;"
mysql -u your_user -p cloudvault < server/schema.mysql.sql
```

Set the connection variables before starting Flask:

```bash
export DATABASE_BACKEND=mysql
export MYSQL_HOST=127.0.0.1
export MYSQL_PORT=3306
export MYSQL_DATABASE=cloudvault
export MYSQL_USER=your_user
export MYSQL_PASSWORD='your_database_password'
export CLOUDVAULT_SECRET='your_persistent_random_session_signing_key'
```

For Windows PowerShell, set environment variables using `$env:NAME = 'value'`.

MySQL mode requires a running MySQL service and valid credentials. It does not import the bundled SQLite demo accounts or reset the database automatically. Register an account, then grant the first administrator role using an authorised database connection:

```sql
UPDATE users SET role='Admin' WHERE email='your-admin-email';
```

---

## ☁️ Optional S3-compatible object storage

Set the storage backend and bucket before starting the app:

```bash
export STORAGE_BACKEND=s3
export S3_BUCKET=your-private-bucket
export AWS_DEFAULT_REGION=your-region
```

Provide credentials through the normal AWS SDK credential chain. Set `S3_ENDPOINT_URL` if using an S3-compatible provider with a custom endpoint. Create the bucket separately and grant the application permission to inspect, upload, and retrieve objects.

Objects use checksum-based keys and are downloaded through the application; the bucket does not need to be public. Switching storage backends does not migrate existing objects, so keep each database's referenced objects available. A live S3 bucket is not included with this project.

---

## 📁 Project structure

```text
CloudVault/
├── README.md
├── FEATURE_AUDIT.md
├── design.md
├── app/
│   ├── login.html
│   ├── dashboard.html
│   ├── reports.html
│   ├── admin.html
│   ├── css/
│   └── js/
├── server/
│   ├── app.py                 # Flask API and application rules
│   ├── database.py            # SQLite/MySQL database adapter
│   ├── storage.py             # Local/S3 file storage
│   ├── schema.sql             # SQLite schema
│   ├── schema.mysql.sql       # MySQL schema
│   ├── seed.py                # Explicit demo reset script
│   ├── requirements.txt
│   ├── cloudvault.db          # Bundled SQLite demo database
│   ├── uploads/               # Local uploaded content
│   └── tests/
└── resources/
    ├── CloudVault_DB_Design.docx
    ├── 03. DB Design.pdf
    └── Group D15 Project Abstract.pdf
```

---

## ⚠️ Notes and limitations

- The included package is a coursework/demo application, not a hardened production deployment. Use HTTPS, a production WSGI server, secure credentials, and a persistent session signing key before deployment.
- The default local setup uses SQLite and local storage. MySQL and S3 are optional backends, not preconfigured live services.
- Historical version metadata cannot recreate a content object that is missing from storage.
- Logical quota usage includes retained historical versions and soft-deleted files. Permanent deletion reduces the logical usage, but unreferenced content objects are not automatically garbage-collected.
- Public links download the file's current version. A localhost link is only reachable from the machine running the app unless the application is deployed to a reachable host.
- Folder creation and navigation are supported; folder rename, move, and deletion controls are not included.
- The application does not include email invitations, password reset, content preview, version merging, or automatic object garbage collection.

---

**23CSE202 — Database Management Systems**  
*Amrita School of Computing, Amritapuri*
