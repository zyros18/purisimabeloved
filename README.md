# CPC Dashboards

This XAMPP PHP application provides separate dashboards for students and teachers. It uses PHP and MySQL; it does not use `npm run dev`.

## Run locally

1. Start **Apache** and **MySQL** in the XAMPP Control Panel.
2. Make sure the application's database has been created and configured for this installation.
3. Open [http://localhost/login.php](http://localhost/login.php) and sign in with an account for the appropriate role.
4. After login, students are sent to `includes/student_dashboard.php` and teachers to `includes/teachers_dashboard.php`.

If this project is installed in a subfolder under `htdocs`, include that folder in the URL, for example `http://localhost/login.php`.

## Deploy to Render

1. Push the project to the GitHub branch you want to deploy.
2. In Render, create a **Web Service** from that repository. Select the **Docker** runtime and the repository's root `Dockerfile`.
3. The Docker image configures Apache to listen on port `10000`, Render's default web-service port. No `PORT` environment variable is needed.
4. Configure an external MySQL database. This application uses MySQL and cannot connect to Render PostgreSQL. Import `includes/database/cpc_penalty.sql` into the MySQL database.
5. Add these environment variables to the Render service using the connection details from your MySQL provider:
   - `CPC_DB_HOST`
   - `CPC_DB_PORT` (usually `3306`)
   - `CPC_DB_NAME`
   - `CPC_DB_USER`
   - `CPC_DB_PASSWORD`
6. Deploy the service. Render provides an HTTPS `onrender.com` URL and manages its TLS certificate. Add a custom domain in Render if desired, then configure the DNS records Render provides.

The login page displays a setup message if MySQL is not configured or reachable; sign-in will work after the database is available and its schema is imported.

## Student dashboard

File: `includes/student_dashboard.php`

The student dashboard is restricted to signed-in student accounts and provides these views:

- **Dashboard** - overview of the student's penalties and tuition balance.
- **Penalties** - the student's penalty records and payment status.
- **Tuition** - the student's tuition balance and payment information.

Use the dashboard navigation or add a view query parameter, such as `?view=penalties` or `?view=tuition`.

## Teacher dashboard

File: `includes/teachers_dashboard.php`

The teacher dashboard is restricted to signed-in teacher accounts and provides these views:

- **Dashboard** - summary and records for student penalties and payment requests.
- **Students** - student records and tuition balances.
- **Penalties** - create and manage penalties, and review payment requests.

Use the dashboard navigation or add a view query parameter, such as `?view=students` or `?view=penalties`.

## Notes

- Sign in through `login.php`; opening a dashboard without the required role redirects to login.
- Dashboard data comes from the configured MySQL database.
- This README documents the dashboard pages in `includes/`. The separate application in `cpc-system/` has its own setup guide.
- The root `.gitignore` excludes local environment files, generated dependencies/output, logs, and common editor/OS files. `cpc-system/` also has its own `.gitignore` for its `.env` and Composer `vendor/` dependencies.
