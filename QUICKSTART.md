# Lumière Bistro - Quick Start Guide

## 🚀 Get Up and Running in 5 Minutes

### Step 1: Prerequisites
- Supabase account (free tier available)
- Gmail account with 2FA enabled
- Node.js 18+

### Step 2: Clone & Install
```bash
git clone <your-repo>
cd lumiere-bistro
pnpm install
```

### Step 3: Create `.env.local`
```env
# Supabase (from your Supabase dashboard)
NEXT_PUBLIC_SUPABASE_URL=https://your-project.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=your-anon-key

# Gmail SMTP (create an App Password: https://myaccount.google.com/apppasswords)
SMTP_HOST=smtp.gmail.com
SMTP_PORT=587
SMTP_SECURE=false
SMTP_USER=your-email@gmail.com
SMTP_PASSWORD=your-16-char-app-password
SMTP_FROM=your-email@gmail.com

# Application
NEXT_PUBLIC_APP_URL=http://localhost:3000
CRON_SECRET=your_random_secure_string_32_chars
```

### Step 4: Initialize Staff Account
```bash
curl -X POST http://localhost:3000/api/setup/staff \
  -H "Content-Type: application/json" \
  -d '{"password": "YourStaffPassword123"}'
```

### Step 5: Start Development
```bash
pnpm dev
```

Then open http://localhost:3000 in your browser.

---

## 🧪 Test the System

### Test Customer Booking (1 min)
1. Go to http://localhost:3000
2. Scroll to "Reserve Your Table"
3. Fill in details, pick a date/time
4. Click "Complete Booking"
5. Check your email for confirmation

### Test Staff Dashboard (1 min)
1. Go to http://localhost:3000/staff/login
2. Enter your staff password
3. See your booking appear in the table
4. Click "Arrived" to update status

### Test Booking Lookup (1 min)
1. Go to http://localhost:3000/bookings/lookup
2. Enter the email from your test booking
3. View and cancel if desired

---

## 📋 Database Setup

The database schema is **automatically created** when the Supabase integration runs. All tables and policies are configured.

If you need to manually verify:
1. Log into Supabase dashboard
2. Go to SQL Editor
3. You should see: `experiences`, `bookings`, `restaurant_config`, `staff_accounts` tables

---

## 🚢 Deploy to Vercel (10 min)

### 1. Connect Repository
```bash
vercel link
```

### 2. Add Environment Variables
```bash
vercel env add NEXT_PUBLIC_SUPABASE_URL
vercel env add NEXT_PUBLIC_SUPABASE_ANON_KEY
vercel env add SMTP_HOST
vercel env add SMTP_PORT
vercel env add SMTP_SECURE
vercel env add SMTP_USER
vercel env add SMTP_PASSWORD
vercel env add SMTP_FROM
vercel env add NEXT_PUBLIC_APP_URL
vercel env add CRON_SECRET
```

### 3. Deploy
```bash
vercel deploy --prod
```

Your site is live! 🎉

---

## 🤔 Troubleshooting

### Email Not Sending?
- ✓ Check Gmail 2FA is enabled
- ✓ Verify App Password (not regular password)
- ✓ Check SMTP credentials in `.env.local`
- ✓ Run: `pnpm dev` and check terminal logs

### Bookings Not Showing?
- ✓ Verify Supabase URL and key are correct
- ✓ Check browser DevTools Console for errors
- ✓ Make sure date/time are valid (next 30 days only)

### Staff Login Not Working?
- ✓ Verify you ran the setup command
- ✓ Use the exact password from setup
- ✓ Clear browser cookies: Cmd+Shift+Delete (Mac) or Ctrl+Shift+Delete (Windows)

### Build Errors?
- ✓ Delete `node_modules`: `rm -rf node_modules`
- ✓ Reinstall: `pnpm install`
- ✓ Rebuild: `pnpm build`

---

## 📚 Full Documentation

- **README.md** - Complete overview
- **DEPLOYMENT_GUIDE.md** - Production setup
- **IMPLEMENTATION_COMPLETE.md** - What's built

---

## 🎯 Next Steps After Testing

1. ✅ Test bookings work (you did this!)
2. ✅ Deploy to Vercel (10 min)
3. ✅ Set up reminder emails (cron jobs)
4. ✅ Update contact info in footer
5. ✅ Customize restaurant hours/capacity
6. ✅ Launch! 🚀

---

## 📞 Need Help?

1. Check DEPLOYMENT_GUIDE.md troubleshooting section
2. Review console logs: `pnpm dev` and check terminal
3. Check browser DevTools Network tab for API errors
4. Verify all environment variables are set correctly

---

**You're all set!** Your booking system is ready. Happy hosting! 🍽️
