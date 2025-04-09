# Teacher Evaluation Database Schema

This repository contains the SQL schema for a teacher evaluation platform. The database is designed to manage teacher evaluations, session transcripts, weekly reports, user and admin accounts, and uploaded files in a structured and relational format.

---

## Entity-Relationship Overview

The schema consists of the following entities:

### 1. **User**
Stores information about users (teachers/admins).

| Column        | Type             | Description                     |
|---------------|------------------|---------------------------------|
| user_id       | CHAR(10)         | Primary Key                     |
| username      | VARCHAR(50)      | Unique username                 |
| password_hash | VARCHAR(255)     | Encrypted password              |
| email         | VARCHAR(100)     | User email                      |
| school_id     | CHAR(10)         | FK to `School`                  |
| is_admin      | TINYINT(1)       | Admin status flag               |
| created_at    | TIMESTAMP        | Account creation timestamp      |
| updated_at    | TIMESTAMP        | Last update timestamp           |

---

### 2. **School**
Represents schools in a district.

| Column      | Type         | Description        |
|-------------|--------------|--------------------|
| school_id   | CHAR(10)     | Primary Key        |
| name        | VARCHAR(100) | School name        |
| district_id | CHAR(10)     | FK to `District`   |
| created_at  | TIMESTAMP    | Creation time      |
| updated_at  | TIMESTAMP    | Last update time   |

---

### 3. **District**
Stores district-level data and linked rubrics.

| Column      | Type         | Description        |
|-------------|--------------|--------------------|
| district_id | CHAR(10)     | Primary Key        |
| name        | VARCHAR(100) | District name      |
| rubric_id   | CHAR(10)     | FK to `Rubric`     |
| created_at  | TIMESTAMP    | Creation time      |
| updated_at  | TIMESTAMP    | Last update time   |

---

### 4. **Rubric**
Defines evaluation rubrics for teachers.

| Column      | Type         | Description         |
|-------------|--------------|---------------------|
| rubric_id   | CHAR(10)     | Primary Key         |
| name        | VARCHAR(100) | Rubric title        |
| description | TEXT         | Rubric details      |
| created_at  | TIMESTAMP    | Creation time       |
| updated_at  | TIMESTAMP    | Last update time    |

---

### 5. **Session**
Stores teacher observation or evaluation sessions.

| Column        | Type     | Description        |
|---------------|----------|--------------------|
| session_id    | CHAR(10) | Primary Key        |
| transcription | TEXT     | Transcript content |
| user_id       | CHAR(10) | FK to `User`       |
| created_at    | TIMESTAMP | Timestamp          |

---

### 6. **Report**
Holds evaluation report data generated from sessions and weekly reports.

| Column           | Type     | Description           |
|------------------|----------|-----------------------|
| report_id        | CHAR(10) | Primary Key           |
| session_id       | CHAR(10) | FK to `Session`       |
| weekly_report_id | CHAR(10) | FK to `Weekly_Report` |
| report_file      | TEXT     | File content or path  |
| created_at       | TIMESTAMP | Timestamp             |
| updated_at       | TIMESTAMP | Timestamp             |

---

### 7. **Weekly_Report**
Links users to weekly evaluations.

| Column            | Type     | Description         |
|-------------------|----------|---------------------|
| weekly_report_id  | CHAR(10) | Primary Key         |
| user_id           | CHAR(10) | FK to `User`        |
| created_at        | TIMESTAMP | Timestamp           |
| updated_at        | TIMESTAMP | Timestamp           |

---

### 8. **File**
Handles file uploads (e.g., supporting documents or audio).

| Column     | Type         | Description           |
|------------|--------------|-----------------------|
| file_id    | CHAR(36)     | Primary Key (UUID)    |
| fileName   | VARCHAR(255) | Name of the file      |
| fileUrl    | TEXT         | URL or path           |
| uploadedBy | CHAR(36)     | FK to `User` (uploader) |
| created_at | TIMESTAMP    | Upload time           |

---

## Relationships Summary

- A **User** belongs to a **School** and can upload multiple **Files**, author **Sessions**, and submit **Weekly Reports**.
- Each **Session** generates **Reports**, which may be linked to **Weekly Reports**.
- **Schools** belong to **Districts**, which use **Rubrics** for evaluations.
- **Files** are associated with Users and stored for review or recordkeeping.

