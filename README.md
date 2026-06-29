# Premium Hair Salon - Booking & CRM System

This project is a lightweight, cost-effective landing page and booking system tailored for a service-based business like a Hair Salon. It utilizes a serverless architecture with Next.js (App Router) and Supabase, removing the need for a standalone backend server while maintaining high performance and security.

## 🌟 Project Overview

The system is divided into three primary user experiences:
1. **Landing Page (`/`)**: A visually stunning, modern dark-themed showcase of the salon's services, pricing, and aesthetic.
2. **Client Dashboard (`/dashboard`)**: A private portal where clients can view their upcoming appointments and track their treatment phases.
3. **Admin Console (`/admin`)**: A restricted panel for staff members to manage daily operations, track revenue, view the schedule, and update appointment statuses.

### Tech Stack
* **Frontend Framework**: Next.js (App Router)
* **Language**: TypeScript
* **Styling**: Tailwind CSS + shadcn/ui + custom HSL aesthetic tokens
* **Database & Auth**: Supabase (PostgreSQL, Row Level Security)
* **Integrations**: Google Calendar API (via Server Actions)

---

## 🚀 How to Integrate Actual Backends

Currently, the project is equipped with a **Preview Mode** that utilizes mock data when the `.env.local` contains dummy keys. To deploy this to production or connect real data, follow these steps:

### 1. Supabase (Database & Authentication) Setup
1. **Create a Project**: Go to [Supabase](https://supabase.com/), sign up, and create a new project.
2. **Apply Schema**: Navigate to the **SQL Editor** in your Supabase dashboard. Open the local file `supabase/schema.sql` found in this repository, copy its contents, and run it. This will:
   * Create the necessary tables (`profiles`, `services`, `appointments`, `client_crm`).
   * Set up PostgreSQL Triggers to auto-create client profiles on signup.
   * Enable Row Level Security (RLS) to ensure clients can only see their own data while staff can see everything.
3. **Get API Keys**: Navigate to **Project Settings > API**.
   * Copy the **Project URL**.
   * Copy the **anon `public` key**.
4. **Update Environment**: Open `web/.env.local` and replace the dummy keys:
   ```env
   NEXT_PUBLIC_SUPABASE_URL="YOUR_SUPABASE_PROJECT_URL"
   NEXT_PUBLIC_SUPABASE_ANON_KEY="YOUR_SUPABASE_ANON_KEY"
   ```

### 2. Google Calendar Setup
The system syncs appointments directly to a Google Calendar to prevent double-booking.
1. **Google Cloud Console**: Create a new project in the [Google Cloud Console](https://console.cloud.google.com/).
2. **Enable API**: Go to "APIs & Services" -> "Library" and enable the **Google Calendar API**.
3. **Create Service Account**: 
   * Go to "Credentials" -> "Create Credentials" -> "Service Account".
   * Once created, go to the "Keys" tab, add a new key, and select **JSON**. This will download a file containing your `client_email` and `private_key`.
4. **Share Calendar**: Open your actual Google Calendar in the browser. Go to settings for the specific calendar you want to use, and **Share** it with the `client_email` of your service account (giving it "Make changes to events" permissions).
5. **Get Calendar ID**: In the same Google Calendar settings, scroll down to find the **Calendar ID** (usually an email format or `primary`).
6. **Update Environment**: Open `web/.env.local` and add the keys:
   ```env
   GOOGLE_CLIENT_EMAIL="your-service-account-email@...gserviceaccount.com"
   GOOGLE_PRIVATE_KEY="-----BEGIN PRIVATE KEY-----\nYourKeyHere\n-----END PRIVATE KEY-----\n"
   GOOGLE_CALENDAR_ID="your-calendar-id"
   ```
*(Note: Ensure your `GOOGLE_PRIVATE_KEY` string includes actual `\n` characters so Next.js parses it correctly).*

---

## 💻 General Usage

### Running Locally
To run the development server:
```bash
cd web
npm install
npm run dev
```
Open `http://localhost:3000` to view the application.

### Managing Staff Access
By default, new users sign up with a `client` role. To grant an employee access to the `/admin` portal:
1. Go to your Supabase dashboard.
2. Open the **Table Editor** and select the `profiles` table.
3. Find the user's row, and change their `role` column from `client` to `staff`.
4. The user can now access `http://localhost:3000/admin`.

---

## 🛠 Maintenance & Best Practices

1. **Adding New Services**:
   Because the system is dynamic, you can add new hair treatments simply by inserting a new row into the `services` table in Supabase. The landing page and booking engine will automatically fetch and display them.
   
2. **UI/UX Updates**:
   * **Colors**: Global theme colors are controlled via HSL variables in `web/src/app/globals.css`. Modifying the `--primary` variable will update the Champagne/Gold accents across the entire site instantly.
   * **Components**: New components can be added using shadcn/ui. Run `npx shadcn@latest add [component-name]` in the `/web` directory.

3. **Database Migrations**:
   If you need to change the database schema in the future (e.g., adding a "rewards points" system), do not edit the live database directly. Instead:
   * Write the alter statements in a new `.sql` file in `supabase/migrations/`.
   * Apply them to Supabase so you have a trackable history of schema changes.

4. **Security Check**:
   Never expose your `GOOGLE_PRIVATE_KEY` or Supabase Service Role keys to the client. The Next.js `src/actions/` folder contains Server Actions, which are strictly executed securely on the server-side, protecting your business logic.
