# Data Dictionary

## Users

| Column | Type | Description |
|--------|------|-------------|
| UserId | INT | Primary key |
| FullName | VARCHAR(100) | User full name |
| Email | VARCHAR(150) | Login email |
| PasswordHash | VARCHAR(255) | Hashed password |
| Phone | VARCHAR(20) | User contact |
| Role | ENUM | Student or Admin |

## Categories

| Column | Type | Description |
|--------|------|-------------|
| CategoryId | INT | Primary key |
| CategoryName | VARCHAR(100) | Category name |

## Items

| Column | Type | Description |
|--------|------|-------------|
| ItemId | INT | Primary key |
| UserId | INT | Reporter |
| CategoryId | INT | Category reference |
| ItemName | VARCHAR(150) | Name of item |
| Description | TEXT | Item description |
| ReportType | ENUM | Lost or Found |
| Status | ENUM | Item status |
