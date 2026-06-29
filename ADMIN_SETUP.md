# Admin Portal Setup Guide

## Quick Start with Demo Account

The easiest way to get started is to use the demo credentials:

**Demo Admin Credentials:**
- **Username:** `admin@lumiere-bistro.co.uk`
- **Password:** `Demo123!`

### Setting Up the Demo Account

1. **Initialize the demo account** by calling this endpoint:
   ```bash
   curl -X POST http://localhost:3000/api/setup/demo
   ```

2. **Access the admin portal:**
   - Go to http://localhost:3000
   - Look for the ⚙️ button in the bottom-right corner
   - Click it to open the admin login modal
   - Enter the demo credentials above

3. **You should now see:**
   - Dashboard with 12 sample bookings
   - Real-time booking management interface
   - Demo customer data

## Custom Admin Setup

If you want to create a custom admin account with your own credentials:

```bash
curl -X POST http://localhost:3000/api/setup/staff \
  -H "Content-Type: application/json" \
  -d '{
    "username": "your-email@restaurant.com",
    "password": "YourSecurePassword123!"
  }'
```

**Requirements:**
- Username can be any email address
- Password must be at least 8 characters long

## Admin Portal Features

### Access
- Located in the **bottom-right corner** as a ⚙️ icon
- Click to open the login modal
- Login with your credentials

### Security Features
- **3 failed attempts lockout:** After 3 wrong password attempts, the account is locked for 1 hour
- **Session management:** Sessions expire after 7 days
- **HTTPS only:** Use HTTPS in production to ensure secure transmission
- **HTTP-only cookies:** Session data is stored in secure, HTTP-only cookies

### Dashboard Capabilities
Once logged in, you can:

1. **View All Confirmed Bookings**
   - See customer names, emails, and phone numbers
   - View party size, dietary restrictions, and special notes
   - See booking date and time

2. **Manage Booking Status**
   - Mark bookings as "Arrived"
   - Mark bookings as "No-Show" (with auto-cancellation after 15-minute grace period)
   - Cancel bookings manually

3. **Customer Details**
   - Full contact information
   - Special requests and dietary needs
   - Booking history

## Demo Data

The system comes with 12 sample bookings including:
- Mix of different experience types
- Various party sizes (2-8 people)
- Dietary restrictions and special occasions
- Realistic booking dates and times

Perfect for testing the admin interface before going live!

## Resetting Admin Credentials

To reset the admin password:

```bash
curl -X POST http://localhost:3000/api/setup/staff \
  -H "Content-Type: application/json" \
  -d '{
    "username": "admin@lumiere-bistro.co.uk",
    "password": "NewPassword123!"
  }'
```

This will update the existing admin account with the new password.

## Rate Limiting & Security

### Failed Login Attempts
- **1st attempt:** Normal attempt
- **2nd attempt:** Warning message
- **3rd attempt:** Account locked for 1 hour

After 1 hour of lockout:
- The lock automatically expires
- You can attempt to login again
- Failed attempt counter resets to 0

### Production Recommendations

Before deploying to production:

1. **Change demo credentials** immediately
2. **Use strong passwords** (16+ characters with mixed case, numbers, symbols)
3. **Enable HTTPS** on your domain
4. **Set up email notifications** for failed login attempts
5. **Monitor admin activity** regularly
6. **Rotate admin passwords** every 90 days

## Troubleshooting

### "Invalid credentials" after setup
- Double-check your username and password
- Ensure at least 8 characters in password
- Try resetting with the setup endpoint above

### Account locked message
- Wait 1 hour before trying again
- Or contact system administrator to manually unlock
- Reset by calling the setup endpoint with new password

### Can't access admin portal
- Ensure you're logged in (cookies enabled)
- Clear browser cache and cookies
- Try incognito/private browsing mode
- Check that session hasn't expired (7 days)

## API Endpoints

### Setup Endpoints
```
POST /api/setup/demo           → Initialize with demo credentials
POST /api/setup/staff          → Create/update custom admin account
```

### Admin Endpoints (requires authentication)
```
GET  /api/staff/bookings       → Fetch all confirmed bookings
PATCH /api/staff/bookings      → Update booking status
POST /api/staff/logout         → Logout current session
```

## Questions?

For more information, see:
- **START_HERE.md** - Overall project guide
- **QUICKSTART.md** - Quick setup instructions
- **README.md** - Full documentation
