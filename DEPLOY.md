# Deployment Guide for Portfolio Website

This guide assumes you are deploying to a standard shared hosting environment (cPanel) or a VPS with a LAMP stack.

## Prerequisites

1.  **Domain Name**: (e.g., `yourname.com`)
2.  **Hosting Account**: Supporting PHP (7.4+) and MySQL.
3.  **FTP Client**: (e.g., FileZilla) or Access to File Manager in cPanel.

## Step 1: Export Your Database

1.  Open **XAMPP Control Panel** and start MySQL.
2.  Go to `http://localhost/phpmyadmin`.
3.  Select your database (`portfolio_db`) from the sidebar.
4.  Click the **Export** tab in the top menu.
5.  Keep the default settings (Quick, SQL format) and click **Export**.
6.  Save the `.sql` file to your computer.

## Step 2: Set Up Database on Server

1.  Log in to your hosting Control Panel (e.g., cPanel).
2.  Go to **MySQL Databases**.
3.  **Create a New Database**:
    *   Name it something relevant (e.g., `jeeva_portfolio`).
    *   *Note the full database name provided (often prefixed like `username_jeeva_portfolio`).*
4.  **Create a New MySQL User**:
    *   Create a username and a strong password.
    *   *Save these details! You will need them.*
5.  **Add User to Database**:
    *   Select the user and database you just created.
    *   Grant **All Privileges** and save changes.
6.  Go to **phpMyAdmin** in your hosting panel.
7.  Select your new database.
8.  Click **Import**, select the `.sql` file you exported in Step 1, and click **Go**.

## Step 3: Upload Files

1.  Open your FTP client (e.g., FileZilla).
2.  Connect to your server using the FTP details provided by your host.
3.  Navigate to the public web folder (usually `public_html` or `www`).
4.  Upload all files from your local project folder `d:\xampp\htdocs\jeevanantham_33\` to the server.
    *   **Exclude**: `.git` folder, `deploy.md`, `task.md` (optional).
    *   **Include**: `assets`, `index.php`, `process_contact.php`, `setup_images.php`, `db_connect.php`.

## Step 4: Configure Database Connection

Since we updated `db_connect.php` to use environment variables, you have two options:

### Option A: Set Environment Variables (Recommended for Security)
If your host supports setting Environment Variables via cPanel (often under "PHP-FPM Settings" or "Application Manager"):
*   `DB_HOST`: `localhost` (usually)
*   `DB_USER`: *Your cPanel database username*
*   `DB_PASS`: *Your cPanel database password*
*   `DB_NAME`: *Your cPanel database name*

### Option B: Edit `db_connect.php` Directly (Easier for Shared Hosting)
If you can't set environment variables:
1.  Open `db_connect.php` on the server (via File Manager or edit before uploading).
2.  Replace the fallback values with your server details:
    ```php
    $servername = "localhost";
    $username = "username_from_cpanel";
    $password = "your_strong_password";
    $dbname = "username_database_name";
    ```

## Step 5: Test the Site

1.  Visit your domain (e.g., `www.yourname.com`).
2.  Check if content loads correctly.
3.  Test the **Contact Form** to ensure the database connection is working (it should save messages to the `contacts` table).

## Troubleshooting

*   **Database Error**: Check credentials in `db_connect.php`. Ensure the user has "All Privileges".
*   **Images Broken**: Ensure file permissions are set to `644` for images and `755` for folders.
*   **404 Errors**: If you used URLs like `/jeevanantham_33/assets/...` in your code, you might need to find/replace them with `./assets/...` or `/assets/...` depending on if you are in a subdirectory or root.
