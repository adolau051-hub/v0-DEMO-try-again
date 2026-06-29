# Lumière Bistro - Deployment Guide

## Environment Variables

Create a `.env.local` file in the root of your project with the following variables:

### Supabase Configuration
```
NEXT_PUBLIC_SUPABASE_URL=your_supabase_url
NEXT_PUBLIC_SUPABASE_ANON_KEY=your_supabase_anon_key
```

### Email Configuration (SMTP)
Configure email sending for booking confirmations and staff notifications:

```
SMTP_HOST=smtp.gmail.com
SMTP_PORT=587
SMTP_SECURE=false
SMTP_USER=your-email@gmail.com
SMTP_PASSWORD=your_app_password
SMTP_FROM=reservations@lumiere-bistro.co.uk
```

**Gmail Setup:**
1. Enable 2-Factor Authentication on your Gmail account
2. Generate an App Password at https://myaccount.google.com/apppasswords
3. Use the 16-character password as `SMTP_PASSWORD`

### Application Configuration
```
NEXT_PUBLIC_APP_URL=https://your-domain.com
CRON_SECRET=your_secure_random_string
```

### Optional: Vercel Environment Variables
If using Vercel for deployment, add these variables in your Vercel project settings.

## Initial Setup

### 1. Create Staff Account
Make a POST request to initialize the staff account (do this once):

```bash
curl -X POST http://localhost:3000/api/setup/staff \
  -H "Content-Type: application/json" \
  -d '{"password": "your_secure_password_min_8_chars"}'
```

Response:
```json
{
  "success": true,
  "message": "Staff account created"
}
```

Keep this password secure - staff will use it to log in at `/staff/login`.

### 2. Configure Restaurant Settings
Log in to the staff dashboard at `/staff/login` and the system will use the default configuration from `restaurant_config` table:

- **Hours:** 17:00 - 23:00 (5 PM - 11 PM)
- **Max Covers:** 40 per night
- **Booking Advance:** Up to 30 days
- **Seating Interval:** 30 minutes
- **Staff Email:** reservations@lumiere-bistro.co.uk

To customize, update the `restaurant_config` table directly in Supabase.

## Deployment to Vercel

### Prerequisites
- Vercel account
- GitHub repository with the project code
- All environment variables configured

### Steps

1. **Connect Repository to Vercel**
   - Go to https://vercel.com/new
   - Select your GitHub repository
   - Choose Next.js as the framework

2. **Set Environment Variables**
   - In Vercel project settings, go to Environment Variables
   - Add all variables from `.env.local`
   - Make sure `NEXT_PUBLIC_*` variables are available to the browser

3. **Deploy**
   - Vercel will automatically build and deploy
   - Your app will be live at `https://your-project.vercel.app`

4. **Update Supabase Redirect URLs**
   - Log in to Supabase dashboard
   - Go to Authentication > URL Configuration
   - Add your Vercel domain to Authorized redirect URLs

## Setting Up Cron Jobs for Reminders

### Option 1: Vercel Cron (Recommended for Vercel deployments)

Create `vercel.json` in the project root:

```json
{
  "crons": [
    {
      "path": "/api/cron/send-reminders",
      "schedule": "0 10 * * *"
    },
    {
      "path": "/api/cron/handle-no-shows",
      "schedule": "0 23 * * *"
    }
  ]
}
```

This will:
- Send reminder emails at 10 AM UTC daily
- Handle no-shows at 11 PM UTC daily

### Option 2: External Cron Service

Use a service like cron-job.org:

1. Create a trigger for `/api/cron/send-reminders`
   - Schedule: Daily at 10:00 UTC
   - Method: POST
   - Headers: `Authorization: Bearer {CRON_SECRET}`

2. Create a trigger for `/api/cron/handle-no-shows`
   - Schedule: Daily at 23:00 UTC
   - Method: POST
   - Headers: `Authorization: Bearer {CRON_SECRET}`

## Database

### Supabase Schema

The application uses the following tables:

- **experiences** - Dining experience options
- **restaurant_config** - Restaurant settings and hours
- **bookings** - Guest reservations
- **staff_accounts** - Staff login credentials

All tables have Row Level Security (RLS) enabled for data protection.

## Testing

### Public Booking Flow
1. Visit homepage
2. Scroll to "Reserve Your Table" section
3. Fill in booking details
4. Confirm booking
5. Check email for confirmation

### Staff Dashboard
1. Visit `/staff/login`
2. Enter staff password
3. View and manage all bookings
4. Mark bookings as arrived, no-show, or cancel

### Booking Lookup
1. Visit `/bookings/lookup`
2. Enter email address
3. View booking details
4. Cancel if needed

## Troubleshooting

### Email Not Sending
- Check SMTP configuration in environment variables
- Verify Gmail has 2FA enabled and app password is correct
- Check Vercel logs for error details

### Bookings Not Showing
- Verify Supabase connection and URL/key are correct
- Check that bookings are created with `status = 'confirmed'`
- Ensure current date is within booking advance window

### Staff Login Failing
- Verify `CRON_SECRET` is set
- Check that staff account was initialized via `/api/setup/staff`
- Clear browser cookies and try again

## Monitoring

- Check Vercel Analytics for performance metrics
- Monitor Supabase usage in the Supabase dashboard
- Review email logs for delivery issues
- Check staff dashboard regularly for incoming bookings

## Support

For issues or questions, contact Vercel support or consult the Supabase documentation.
