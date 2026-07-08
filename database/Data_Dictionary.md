
# Data Dictionary

## 1. USERS Table

Stores all students and administrators.

| Column        | Data Type               | Constraints                                           |
| ------------- | ----------------------- | ----------------------------------------------------- |
| user_id       | INT                     | PK, AUTO_INCREMENT                                    |
| full_name     | VARCHAR(100)            | NOT NULL                                              |
| email         | VARCHAR(150)            | NOT NULL, UNIQUE                                      |
| password_hash | VARCHAR(255)            | NOT NULL                                              |
| phone_number  | VARCHAR(20)             | NULL                                                  |
| role          | ENUM('Student','Admin') | DEFAULT 'Student'                                     |
| is_active     | BOOLEAN                 | DEFAULT TRUE                                          |
| created_at    | TIMESTAMP               | DEFAULT CURRENT_TIMESTAMP                             |
| updated_at    | TIMESTAMP               | DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP |

### Primary Key

* `user_id`

---

## 2. CATEGORIES Table

Avoids repeating category names in every item.

| Column        | Data Type    | Constraints        |
| ------------- | ------------ | ------------------ |
| category_id   | INT          | PK, AUTO_INCREMENT |
| category_name | VARCHAR(50)  | UNIQUE, NOT NULL   |
| description   | VARCHAR(255) | NULL               |

### Primary Key

* `category_id`

### Example Categories

* Electronics
* Documents
* Keys
* Bags
* Clothing
* Wallet
* Books
* Others

---

## 3. ITEMS Table

Stores both Lost and Found reports.

| Column      | Data Type                                 | Constraints                                           |
| ----------- | ----------------------------------------- | ----------------------------------------------------- |
| item_id     | INT                                       | PK, AUTO_INCREMENT                                    |
| reported_by | INT                                       | FK → users.user_id                                    |
| category_id | INT                                       | FK → categories.category_id                           |
| item_name   | VARCHAR(150)                              | NOT NULL                                              |
| description | TEXT                                      | NOT NULL                                              |
| image_path  | VARCHAR(255)                              | NULL                                                  |
| report_type | ENUM('Lost','Found')                      | NOT NULL                                              |
| status      | ENUM('Lost','Found','Claimed','Returned') | DEFAULT 'Lost'                                        |
| location    | VARCHAR(150)                              | NOT NULL                                              |
| event_date  | DATE                                      | NOT NULL                                              |
| created_at  | TIMESTAMP                                 | DEFAULT CURRENT_TIMESTAMP                             |
| updated_at  | TIMESTAMP                                 | DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP |

### Primary Key

* `item_id`

### Foreign Keys

* `reported_by → users.user_id`
* `category_id → categories.category_id`

---

## 4. CLAIMS Table

Stores ownership requests.

| Column            | Data Type                             | Constraints               |
| ----------------- | ------------------------------------- | ------------------------- |
| claim_id          | INT                                   | PK, AUTO_INCREMENT        |
| item_id           | INT                                   | FK → items.item_id        |
| claimant_id       | INT                                   | FK → users.user_id        |
| claim_description | TEXT                                  | NOT NULL                  |
| claim_status      | ENUM('Pending','Approved','Rejected') | DEFAULT 'Pending'         |
| reviewed_by       | INT                                   | FK → users.user_id, NULL  |
| reviewed_at       | DATETIME                              | NULL                      |
| created_at        | TIMESTAMP                             | DEFAULT CURRENT_TIMESTAMP |

### Primary Key

* `claim_id`

### Foreign Keys

* `item_id → items.item_id`
* `claimant_id → users.user_id`
* `reviewed_by → users.user_id`

---

## 5. NOTIFICATIONS Table

Stores system notifications.

| Column          | Data Type    | Constraints               |
| --------------- | ------------ | ------------------------- |
| notification_id | INT          | PK, AUTO_INCREMENT        |
| user_id         | INT          | FK → users.user_id        |
| title           | VARCHAR(100) | NOT NULL                  |
| message         | TEXT         | NOT NULL                  |
| is_read         | BOOLEAN      | DEFAULT FALSE             |
| created_at      | TIMESTAMP    | DEFAULT CURRENT_TIMESTAMP |

### Primary Key

* `notification_id`

### Foreign Key

* `user_id → users.user_id`

---

## 6. ADMIN_ACTIONS Table

Stores audit logs.

| Column             | Data Type    | Constraints               |
| ------------------ | ------------ | ------------------------- |
| action_id          | INT          | PK, AUTO_INCREMENT        |
| admin_id           | INT          | FK → users.user_id        |
| item_id            | INT          | FK → items.item_id        |
| action_type        | VARCHAR(100) | NOT NULL                  |
| action_description | TEXT         | NULL                      |
| created_at         | TIMESTAMP    | DEFAULT CURRENT_TIMESTAMP |

### Primary Key

* `action_id`

### Foreign Keys

* `admin_id → users.user_id`
* `item_id → items.item_id`

---

# Relationships

## Users → Items

One user can report many items.

```text
Users (1) -------- (M) Items
```

Foreign Key:

* `items.reported_by`

---

## Categories → Items

One category can contain many items.

```text
Categories (1) -------- (M) Items
```

Foreign Key:

* `items.category_id`

---

## Users → Claims

One user can create many claims.

```text
Users (1) -------- (M) Claims
```

Foreign Key:

* `claims.claimant_id`

---

## Items → Claims

One item may have multiple ownership claims.

```text
Items (1) -------- (M) Claims
```

Foreign Key:

* `claims.item_id`

---

## Users → Notifications

One user can receive many notifications.

```text
Users (1) -------- (M) Notifications
```

Foreign Key:

* `notifications.user_id`

---

## Users (Admin) → AdminActions

One administrator can perform many actions.

```text
Users (1) -------- (M) AdminActions
```

Foreign Key:

* `admin_actions.admin_id`

---

## Items → AdminActions

One item can have multiple administrative actions.

```text
Items (1) -------- (M) AdminActions
```

Foreign Key:

* `admin_actions.item_id`

---

# Complete Relationship Map

```text
Users
│
├──< Items
│
├──< Claims
│
├──< Notifications
│
└──< AdminActions

Categories
│
└──< Items

Items
│
├──< Claims
│
└──< AdminActions
```

---

# Foreign Key Summary

| Child Table   | Foreign Key | Parent Table |
| ------------- | ----------- | ------------ |
| Items         | reported_by | Users        |
| Items         | category_id | Categories   |
| Claims        | item_id     | Items        |
| Claims        | claimant_id | Users        |
| Claims        | reviewed_by | Users        |
| Notifications | user_id     | Users        |
| AdminActions  | admin_id    | Users        |
| AdminActions  | item_id     | Items        |

---

## Database Overview

This database design supports a **Lost and Found Management System** for educational institutions. It provides functionality for:

* User management for students and administrators
* Lost and found item reporting
* Ownership claim processing
* Notification management
* Administrative auditing and tracking
* Category-based item organization

| ReportType | ENUM | Lost or Found |
| Status | ENUM | Item status |
