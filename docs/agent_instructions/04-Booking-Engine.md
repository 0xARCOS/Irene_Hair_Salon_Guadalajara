# Booking Engine & Integrations

## 1. Booking Wizard Flow
The Booking Wizard should be a multi-step modal or dedicated page:
1. **Select Service**: Fetch from `services` table.
2. **Select Date**: Calendar UI.
3. **Select Time Slot**: Fetch available slots dynamically.
4. **Confirm**: Submit booking.

## 2. Server Action: Fetching Available Slots
Create a Next.js Server Action in `src/actions/booking.ts`.
- **Inputs**: `date` (string), `duration_mins` (number).
- **Logic**:
  1. Define business hours (e.g., 9 AM - 6 PM).
  2. Call Google Calendar API (via `googleapis` npm package) using a Service Account to fetch `busy_slots` for the specified date.
  3. Query the Supabase `appointments` table for existing bookings on that date.
  4. Merge GCal busy slots and DB appointments.
  5. Calculate and return `available_slots` (intervals of `duration_mins` within business hours that do not overlap with busy blocks).

## 3. Server Action: Creating Appointment
- **Inputs**: `service_id`, `start_time`.
- **Logic**:
  1. Authenticate user via Supabase Server Client.
  2. Re-verify the requested time slot is still open (prevent race conditions).
  3. Create an Event in Google Calendar. Obtain `gcal_event_id`.
  4. Insert record into `appointments` table with `status='confirmed'` and `gcal_event_id`.

## 4. Environment Variables
Ensure the following exist in `.env.local`:
```
NEXT_PUBLIC_SUPABASE_URL=...
NEXT_PUBLIC_SUPABASE_ANON_KEY=...
GOOGLE_CLIENT_EMAIL=...
GOOGLE_PRIVATE_KEY=...
GOOGLE_CALENDAR_ID=...
```
