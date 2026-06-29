# Lumière Bistro Website - Implementation Kickstart

## Project Overview

**Objective:** Build a sophisticated restaurant booking website for Lumière Bistro with a public-facing booking interface and a password-protected staff admin dashboard.

**Tech Stack:**
- **Frontend:** Next.js 16 (App Router), React 19, TypeScript
- **Styling:** Tailwind CSS v4, Framer Motion (animations)
- **Database:** Supabase (PostgreSQL)
- **Backend:** Next.js API Routes (Server Actions)
- **Deployment:** Vercel
- **Hosting:** Vercel

**Key Constraint:** No customer user accounts—bookings are one-time guest submissions only.

---

## Database Schema

### Core Tables

#### `bookings`
| Column | Type | Notes |
|--------|------|-------|
| `id` | UUID | Primary key |
| `booking_date` | DATE | Date of booking |
| `booking_time` | TIME | Time slot (e.g., 17:00, 17:30, 18:00) |
| `party_size` | INTEGER | Number of guests |
| `customer_name` | VARCHAR | Guest name |
| `customer_email` | VARCHAR | Guest email |
| `customer_phone` | VARCHAR | Guest phone number |
| `experience_type` | VARCHAR | Experience selected (e.g., 'Standard', 'Chef's Table') |
| `special_notes` | TEXT | Optional dietary restrictions or special requests |
| `status` | VARCHAR | 'confirmed', 'cancelled', 'no-show', 'arrived' |
| `confirmation_token` | VARCHAR | Token for cancellation/modification link |
| `created_at` | TIMESTAMP | Booking creation time |
| `updated_at` | TIMESTAMP | Last modification time |

#### `staff_accounts`
| Column | Type | Notes |
|--------|------|-------|
| `id` | UUID | Primary key |
| `username` | VARCHAR | Staff login username |
| `password_hash` | VARCHAR | Hashed password |
| `email` | VARCHAR | Staff email for notifications |
| `role` | VARCHAR | 'admin' or 'staff' |
| `created_at` | TIMESTAMP | Account creation time |

#### `restaurant_config` (Settings table)
| Column | Type | Notes |
|--------|------|-------|
| `id` | UUID | Primary key |
| `opening_time` | TIME | e.g., 17:00 |
| `closing_time` | TIME | e.g., 23:00 |
| `slot_duration_minutes` | INTEGER | e.g., 30 |
| `max_capacity_per_night` | INTEGER | e.g., 40 |
| `max_party_size` | INTEGER | e.g., 6 (threshold for redirect) |
| `advance_booking_days` | INTEGER | e.g., 30 |
| `reminder_email_enabled` | BOOLEAN | Send reminder emails? |
| `staff_notification_email` | VARCHAR | Email to notify staff of bookings |
| `updated_at` | TIMESTAMP | Last config update |

#### `experiences` (Optional, for future expansion)
| Column | Type | Notes |
|--------|------|-------|
| `id` | UUID | Primary key |
| `name` | VARCHAR | e.g., 'Standard', 'Chef's Table' |
| `description` | TEXT | Experience description |
| `active` | BOOLEAN | Is this experience available for booking? |

---

## Feature Breakdown

### Public Features (Customer Facing)

#### 1. **Homepage / Landing Page**
- Hero section with high-quality bistro imagery
- Navigation: Home, Menu, Book Now, Contact
- Sections:
  - Hero banner (image + call-to-action)
  - About/Brand story section
  - Experiences/Dining options (carousel of 3-4 experience cards)
  - "By the Numbers" section (capacity, years, awards)
  - Testimonials carousel (rotating testimonial cards)
  - FAQ/Objections section (why book with us)
  - Footer with social media links (X/Twitter, Instagram, TikTok, Facebook—visual only, not linked)

#### 2. **Menu Page**
- Separate page displaying full menu
- No pricing
- Organized by course/section
- Responsive grid or list layout
- Link back to booking form

#### 3. **Booking Form (Modal/Page)**
- **Required Fields:**
  - Full name
  - Email address
  - Phone number
  - Booking date (date picker, max 30 days in advance)
  - Booking time (dropdown with available slots)
  - Party size (number input, 1–6; >6 triggers redirect)
  - Experience type (dropdown: Standard, Chef's Table, etc.)
  
- **Optional Fields:**
  - Dietary restrictions
  - Special notes/occasion
  
- **Logic:**
  - Display real-time availability based on bookings + restaurant capacity
  - Disable past dates and dates beyond 30 days
  - Disable fully booked time slots
  - Reject party size > 6 with message to email for group bookings
  - On submit: Create booking record, send confirmation email, show success message
  
- **Confirmation Email:**
  - Include booking details (date, time, party size, experience)
  - Provide link to view/modify/cancel booking (using `confirmation_token`)
  - Include staff phone number for urgent changes

#### 4. **Booking Confirmation/Details Page**
- Accessible via unique link (using `confirmation_token`)
- Display booking summary
- Options to:
  - Modify booking (date/time/party size if still available)
  - Cancel booking
  - Request special accommodations

---

### Staff/Admin Features

#### 5. **Admin Login Page**
- Simple password-based login (no user accounts required—single password for all staff)
- Session management with secure cookies

#### 6. **Admin Dashboard**
- **Bookings View:**
  - Calendar view of bookings by date
  - List view with filters (date range, status, party size)
  - Search by customer name or phone number
  
- **Booking Actions:**
  - View full booking details
  - Modify booking (date, time, party size)
  - Mark as 'arrived' or 'no-show'
  - Cancel booking with reason
  
- **Settings:**
  - Update restaurant capacity, hours, time slots
  - Manage staff notification email
  - View booking statistics

#### 7. **Email Notifications**
- **Booking Confirmation:** Sent to customer on booking creation
- **Reminder Email:** Optional, sent 24 hours before booking (if enabled)
- **Staff Notification:** Sent to configured staff email immediately after booking
- **Cancellation Notification:** Sent to staff when customer cancels

---

## API Endpoints

### Public API Routes

| Method | Endpoint | Purpose |
|--------|----------|---------|
| POST | `/api/bookings` | Create new booking |
| GET | `/api/bookings/[token]` | Retrieve booking by confirmation token |
| PUT | `/api/bookings/[token]` | Modify booking |
| DELETE | `/api/bookings/[token]` | Cancel booking |
| GET | `/api/availability` | Get available time slots for a given date |
| GET | `/api/menu` | Fetch menu items |
| POST | `/api/auth/login` | Staff login (returns session cookie) |
| POST | `/api/auth/logout` | Staff logout |

### Staff/Admin API Routes

| Method | Endpoint | Purpose |
|--------|----------|---------|
| GET | `/api/admin/bookings` | List all bookings (filtered) |
| GET | `/api/admin/bookings/[id]` | Get booking by ID |
| PUT | `/api/admin/bookings/[id]` | Update booking (status, notes) |
| GET | `/api/admin/settings` | Get restaurant config |
| PUT | `/api/admin/settings` | Update restaurant config |
| GET | `/api/admin/stats` | Booking statistics |

---

## Component Structure

### Pages & Layouts

```
app/
├── layout.tsx                    # Root layout with navigation
├── page.tsx                      # Homepage
├── menu/
│   └── page.tsx                 # Menu page
├── admin/
│   ├── layout.tsx               # Admin layout (requires auth)
│   ├── page.tsx                 # Admin dashboard
│   └── login/
│       └── page.tsx             # Admin login
└── api/
    ├── bookings/
    ├── availability/
    ├── menu/
    ├── auth/
    └── admin/
```

### Components

```
components/
├── public/
│   ├── Navbar.tsx
│   ├── Hero.tsx
│   ├── Experiences.tsx
│   ├── Stats.tsx
│   ├── Testimonials.tsx
│   ├── FAQ.tsx
│   ├── Footer.tsx
│   └── BookingForm.tsx
├── booking/
│   ├── BookingModal.tsx
│   ├── DatePicker.tsx
│   ├── TimeSlotSelector.tsx
│   └── ConfirmationMessage.tsx
├── admin/
│   ├── AdminHeader.tsx
│   ├── BookingsCalendar.tsx
│   ├── BookingsList.tsx
│   ├── BookingDetail.tsx
│   ├── SettingsPanel.tsx
│   └── LoginForm.tsx
└── ui/
    ├── Modal.tsx
    ├── Button.tsx
    ├── Card.tsx
    ├── Badge.tsx
    └── ... (other shadcn components)
```

---

## Design System

### Color Palette (Warm & Welcoming)

| Token | Color | Hex | Usage |
|-------|-------|-----|-------|
| **Primary** | Deep Burgundy | `#6B2C2C` | Buttons, accents, headings |
| **Accent** | Warm Gold | `#D4AF37` | Highlights, borders, special text |
| **Background** | Cream | `#FFF8F0` | Page background |
| **Dark Neutral** | Charcoal | `#2C2C2C` | Body text, dark backgrounds |
| **Light Neutral** | Soft White | `#FAFAFA` | Cards, light backgrounds |

### Typography

- **Headings:** Serif font (e.g., Playfair Display or Fraunces)
- **Body Text:** Sans-serif font (e.g., Inter, Poppins)
- **Font Sizes:**
  - H1: 48px / 64px (desktop)
  - H2: 36px / 48px (desktop)
  - H3: 24px / 32px (desktop)
  - Body: 16px / 18px

### Animations

- **Parallax on Scroll:** Hero section and testimonials carousel
- **Fade-in:** Sections as they come into view
- **Smooth Transitions:** Button hovers, modal overlays
- **Carousel Animation:** Testimonials rotate smoothly

### Responsive Breakpoints

- Mobile: < 768px
- Tablet: 768px – 1024px
- Desktop: > 1024px

---

## Implementation Phases

### Phase 1: Foundation (Database + Auth)
1. Set up Supabase database with schema
2. Implement staff login system (password hashing with bcrypt)
3. Implement session management with secure cookies
4. Create basic admin layout and authentication middleware

### Phase 2: Booking System (Core Feature)
1. Design and build booking form component
2. Implement availability calculation logic
3. Create booking API endpoints (POST, GET, PUT, DELETE)
4. Integrate email notifications (confirmation + staff alerts)
5. Create booking confirmation/details page

### Phase 3: Admin Dashboard
1. Build admin bookings list and calendar views
2. Implement booking modification/status updates
3. Create settings panel for restaurant config
4. Add filtering and search functionality

### Phase 4: Public Website
1. Design and build homepage sections (hero, experiences, stats, testimonials)
2. Create menu page
3. Integrate booking form into homepage
4. Add Framer Motion animations (parallax, fade-ins)
5. Build footer with social links

### Phase 5: Polish & Testing
1. Email template styling and testing
2. Mobile responsiveness fine-tuning
3. End-to-end booking flow testing
4. Performance optimization
5. Deployment to Vercel

---

## Key Implementation Details

### Booking Availability Logic

**Algorithm for available slots:**
1. Generate all time slots for the requested date (every 30 minutes, 17:00–23:00)
2. Query all confirmed bookings for that date
3. Calculate occupied seats per time slot
4. Mark slots with < remaining capacity as available
5. Disable slots outside the 30-day advance booking window
6. Disable past dates and times

**Capacity Management:**
- Total capacity: 40 seats per night
- Time slot duration: 30 minutes
- If a booking is 2 hours, it occupies 4 consecutive time slots

### Email Notifications

**Using Supabase + SendGrid or Resend:**
- Confirmation emails sent on booking creation
- Reminder emails sent 24 hours before booking (if enabled)
- Staff notification sent immediately after booking
- All emails include booking token for quick access

### No-Show Auto-Cancellation

**Grace Period Logic:**
- Check bookings 1 hour after scheduled time
- If no status update to 'arrived', automatically set to 'no-show'
- This can be handled via a scheduled job (e.g., Vercel Cron)

### Staff Authentication

**Simple Password Login:**
- Single password for all staff (stored as hash in `staff_accounts`)
- Session stored in secure HTTP-only cookie
- No individual user accounts needed for MVP

---

## External Dependencies

| Package | Purpose | Version |
|---------|---------|---------|
| `supabase` | Database & API | Latest |
| `@supabase/supabase-js` | Supabase client | Latest |
| `framer-motion` | Animations | ^12.0.0 |
| `react-hot-toast` | Toast notifications | Latest |
| `date-fns` | Date utilities | Latest |
| `bcryptjs` | Password hashing | Latest |
| `js-cookie` | Cookie management | Latest |

---

## Environment Variables

```
NEXT_PUBLIC_SUPABASE_URL=
NEXT_PUBLIC_SUPABASE_ANON_KEY=
SUPABASE_SERVICE_ROLE_KEY=
RESEND_API_KEY=
STAFF_PASSWORD_HASH=
RESTAURANT_ADMIN_EMAIL=
```

---

## Deployment Checklist

- [ ] Set up Supabase project and database schema
- [ ] Configure environment variables in Vercel
- [ ] Set up email service (Resend or SendGrid)
- [ ] Test booking flow end-to-end
- [ ] Test staff admin login and dashboard
- [ ] Verify email notifications are sent
- [ ] Test mobile responsiveness
- [ ] Deploy to Vercel
- [ ] Set up domain DNS if needed
- [ ] Test production booking flow

---

## Notes

- **No user accounts for customers:** Bookings are one-time submissions; customers interact via email links.
- **Fixed pricing:** No payment system; all experiences are free to book.
- **Testimonials are fake:** Use realistic-sounding quotes and names; no need for real customers.
- **Social media links:** Display social icons but don't link them anywhere (visual only for now).
- **Admin password:** Use a strong password; consider rotating periodically.
- **Scalability:** Current design supports up to ~100 bookings/day without optimization. If traffic increases, consider caching availability calculations.

---

**Status:** Ready for Phase 1 implementation
**Last Updated:** 2026-06-27
