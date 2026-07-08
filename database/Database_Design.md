# Database Design
## Student Lost and Found Management System

## Entities

1. Users
2. Categories
3. Items
4. Claims
5. Notifications
6. AdminActions

## Relationships

- One user can report many items.
- One user can submit many claims.
- One category can contain many items.
- One item can have many claims.
- One user can receive many notifications.
- One administrator can perform many admin actions.

## Normalization

The database is designed in Third Normal Form (3NF) to reduce redundancy and improve consistency.
