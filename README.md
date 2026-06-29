# Lumière Bistro - Restaurant Booking System

A modern, full-stack restaurant booking platform built with Next.js 16, React 19, Supabase, and Tailwind CSS. Experience fine dining reservation management with real-time availability, automated confirmations, and a staff dashboard.

## Features

### Public-Facing Website
- **Responsive Design** - Beautiful, mobile-first design with warm aesthetic
- **Hero Section** - Striking hero image and brand messaging
- **Experiences Gallery** - Three dining experience options with descriptions and pricing
- **Menu Showcase** - Browse seasonal menu across starters, mains, and desserts
- **Live Availability** - Real-time availability calculation based on party size and date
- **Smart Booking Form** - Reserve a table with dietary restrictions and special notes
- **Email Confirmations** - Instant booking confirmations sent to customers

### Staff Admin Dashboard
- **Secure Login** - Password-protected staff portal
- **Booking Management** - View all reservations and update status
- **Status Tracking** - Mark bookings as arrived, no-show, or cancelled
- **Contact Information** - Quick access to customer details

### Backend Features
- **Database** - Supabase PostgreSQL with Row Level Security
- **Real-time Calculations** - Availability logic with 30-minute slot intervals
- **Email System** - SMTP-based confirmation and reminder emails
- **Cron Jobs** - Automated reminder emails and no-show handling
- **Party Size Limits** - Auto-reject groups >6 people with contact option
- **Grace Period** - 15-minute no-show grace period after booking time

## Tech Stack

- **Framework:** Next.js 16 with App Router
- **UI Library:** React 19 with Framer Motion
- **Styling:** Tailwind CSS 4
- **Database:** Supabase (PostgreSQL)
- **Authentication:** Custom session-based staff auth
- **Email:** Nodemailer with SMTP
- **Icons:** Lucide React
- **Deployment:** Vercel

## Project Structure

```
app/
├── page.tsx                          # Main landing page
├── layout.tsx                        # Root layout
├── globals.css                       # Global styles & design tokens
├── api/
│   ├── bookings/
│   │   ├── create/route.ts          # Create booking
│   │   ├── availability/route.ts    # Get available slots
│   │   ├── lookup/route.ts          # Find booking by email
│   │   └── cancel/route.ts          # Cancel booking
│   ├── staff/
│   │   ├── login/route.ts           # Staff authentication
│   │   ├── bookings/route.ts        # Get/update bookings
│   │   └── logout/route.ts          # Staff logout
│   ├── cron/
│   │   ├── send-reminders/route.ts  # Send reminder emails
│   │   └── handle-no-shows/route.ts # Auto-cancel no-shows
│   └── setup/
│       └── staff/route.ts           # Initialize staff account
├── staff/
│   ├── login/page.tsx               # Staff login page
│   └── dashboard/page.tsx           # Admin dashboard
├── bookings/
│   └── lookup/page.tsx              # Booking lookup & cancel
└── auth/
    └── callback/route.ts            # Auth callback handler

components/
├── hero.tsx                         # Hero section
├── experiences.tsx                  # Experiences gallery
├── booking-form.tsx                 # Main booking form
├── menu.tsx                         # Menu showcase
└── footer.tsx                       # Footer

lib/
├── booking-utils.ts                 # Availability & validation logic
├── email.ts                         # Email sending utilities
├── staff-auth.ts                    # Staff authentication
├── staff-middleware.ts              # Session verification
├── supabase/
│   ├── client.ts                    # Supabase client setup
│   ├── server.ts                    # Supabase server setup
│   └── proxy.ts                     # Session proxy
└── setup-staff.ts                   # Staff setup utilities
```

## Getting Started

### Prerequisites
- Node.js 18+
- pnpm or npm
- Supabase account
- SMTP email provider (Gmail recommended)

### Local Development

1. **Clone Repository**
   ```bash
   git clone <repository>
   cd lumiere-bistro
   ```

2. **Install Dependencies**
   ```bash
   pnpm install
   ```

3. **Set Up Environment Variables**
   Create `.env.local`:
   ```env
   NEXT_PUBLIC_SUPABASE_URL=your_supabase_url
   NEXT_PUBLIC_SUPABASE_ANON_KEY=your_supabase_key
   SMTP_HOST=smtp.gmail.com
   SMTP_PORT=587
   SMTP_USER=your-email@gmail.com
   SMTP_PASSWORD=your_app_password
   SMTP_FROM=reservations@lumiere-bistro.co.uk
   NEXT_PUBLIC_APP_URL=http://localhost:3000
   CRON_SECRET=your_secure_random_string
   ```

4. **Initialize Staff Account**
   ```bash
   curl -X POST http://localhost:3000/api/setup/staff \
     -H "Content-Type: application/json" \
     -d '{"password": "your_password_min_8_chars"}'
   ```

5. **Start Development Server**
   ```bash
   pnpm dev
   ```

6. **Open in Browser**
   Visit http://localhost:3000

### First-Time Setup

- Visit http://localhost:3000 to see the landing page
- Fill out a test booking
- Log in to staff dashboard at `/staff/login` with your staff password
- View your booking in the admin panel

## Usage

### Making a Booking
1. Navigate to homepage
2. Scroll to "Reserve Your Table"
3. Enter personal details
4. Select experience and date
5. Choose available time slot
6. Add dietary restrictions or special notes
7. Click "Complete Booking"
8. Confirmation email will be sent

### Managing Bookings (Staff)
1. Go to `/staff/login`
2. Enter staff password
3. View all upcoming bookings
4. Click status buttons to mark as "Arrived", "No Show", or "Cancel"

### Looking Up Your Booking
1. Go to `/bookings/lookup`
2. Enter email address
3. View booking details
4. Cancel if needed

## Deployment

See [DEPLOYMENT_GUIDE.md](./DEPLOYMENT_GUIDE.md) for:
- Environment setup
- Vercel deployment
- Cron job configuration
- Production best practices

## Database Schema

### experiences
- id (UUID)
- name (TEXT)
- description (TEXT)
- price_gbp (DECIMAL)

### bookings
- id (UUID)
- experience_id (FK)
- customer_name (TEXT)
- customer_email (TEXT)
- customer_phone (TEXT)
- party_size (INTEGER)
- booking_date (DATE)
- booking_time (TIME)
- status ('confirmed', 'arrived', 'no_show', 'cancelled')
- dietary_restrictions (TEXT)
- special_notes (TEXT)

### restaurant_config
- max_covers_per_night (INTEGER)
- booking_advance_days (INTEGER)
- seating_slot_interval_minutes (INTEGER)
- opening_hour (TIME)
- closing_hour (TIME)
- staff_notification_email (TEXT)

### staff_accounts
- id (UUID)
- password_hash (TEXT)
- email (TEXT)
- last_login (TIMESTAMP)

## Design

- **Color Palette:** Warm earth tones (#8b6f47 primary, #d4a574 secondary, #faf6f1 background)
- **Typography:** Serif headings (font-serif), sans-serif body
- **Animations:** Smooth fade-ins and hover effects with Framer Motion
- **Responsive:** Mobile-first design, optimized for all screen sizes

## Performance

- Next.js 16 with Turbopack
- Image optimization with Next.js Image component
- Efficient database queries with Supabase
- CSS-in-JS with Tailwind CSS
- Web Vitals optimized

## Security

- Row Level Security (RLS) on all tables
- HTTP-only session cookies
- Bcryptjs password hashing
- CSRF protection via Next.js middleware
- Input validation and sanitization

## Email Integration

Sends emails for:
- Booking confirmations (immediately)
- Reminder emails (next day at 10 AM UTC)
- Staff notifications (when new booking made)
- Auto-cancel no-shows (11 PM UTC)

## Customization

### Change Restaurant Details
Edit values in Supabase `restaurant_config` table or `/lib/booking-utils.ts`

### Update Hours
Modify `opening_hour` and `closing_hour` in restaurant_config

### Change Seating Intervals
Update `seating_slot_interval_minutes` (default: 30 minutes)

### Customize Experiences
Add/edit dining experiences in Supabase `experiences` table

## Troubleshooting

**Bookings not saving?**
- Check Supabase connection
- Verify RLS policies are correct
- Check browser console for errors

**Emails not sending?**
- Verify SMTP credentials in env vars
- Check Gmail app password is correct
- Enable "Less secure app access" if needed

**Staff login not working?**
- Ensure staff account was created via `/api/setup/staff`
- Clear browser cookies
- Check CRON_SECRET is set

## Contributing

To contribute improvements:
1. Create a feature branch
2. Make your changes
3. Test thoroughly
4. Submit a pull request

## License

This project is proprietary to Lumière Bistro.

## Support

For issues or questions about deployment, see the [DEPLOYMENT_GUIDE.md](./DEPLOYMENT_GUIDE.md) or contact support.

---

Built with ❤️ using Vercel v0
