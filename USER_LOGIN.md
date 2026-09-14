# User Login & Password Security

The tracker now has a built-in multi-user login system.

## First login
1. Open the website.
2. You will be taken to **Create Admin Account** if no user exists.
3. Create your administrator username and password.
4. You will be logged in automatically.

## Create additional users
1. Log in as an administrator.
2. Open **🔐 Users** in the navigation.
3. Create a username, password and role.
4. Give the user the website URL. They can log in with their own account.

## Password storage
Passwords are **not stored as plain text**. The app uses Werkzeug's secure password hashing and stores only the resulting hash in the `app_users` table inside `profiles.db`.

If a user forgets a password, an administrator can set a new password from **User Management**.

## Important for Render Free
Render Free uses an ephemeral filesystem. That means SQLite databases, including the user database, can be lost after some service restarts or redeployments. Do not rely on the Free plan as the permanent storage location for important business records or user accounts. For production use, migrate the database to PostgreSQL or use a paid persistent disk.
