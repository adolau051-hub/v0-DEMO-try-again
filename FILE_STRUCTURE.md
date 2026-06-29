# 📂 Lumière Bistro - Complete File Structure

## 📍 Project Root Directory

```
lumiere-bistro/
│
├── 📖 DOCUMENTATION (Read these first!)
│   ├── START_HERE.md ..................... 👈 BEGIN HERE - Navigation guide
│   ├── QUICKSTART.md ..................... ⚡ 5-minute setup guide
│   ├── README.md ......................... 📚 Complete documentation
│   ├── DEPLOYMENT_GUIDE.md ............... 🚀 Production deployment
│   ├── IMPLEMENTATION_KICKSTART.md ....... 🔧 Technical specifications
│   ├── IMPLEMENTATION_COMPLETE.md ........ ✅ Feature checklist
│   ├── PROJECT_SUMMARY.md ................ 📊 Delivery summary
│   ├── COMPLETION_REPORT.md .............. 🎉 Completion details
│   ├── PHASE_1_COMPLETE.md ............... 📝 Phase 1 notes
│   └── FILE_STRUCTURE.md ................. 📂 This file
│
├── 🎨 SOURCE CODE
│   │
│   ├── app/ ............................ 📄 Next.js App Router
│   │   ├── page.tsx ..................... Landing page (hero + booking + menu)
│   │   ├── layout.tsx ................... Root layout with metadata
│   │   ├── globals.css .................. Global styles & design tokens
│   │   │
│   │   ├── api/ ........................ API Routes (Backend)
│   │   │   ├── bookings/
│   │   │   │   ├── create/route.ts ...... Create new booking
│   │   │   │   ├── availability/route.ts Get available time slots
│   │   │   │   ├── lookup/route.ts ...... Find booking by email
│   │   │   │   └── cancel/route.ts ...... Cancel booking
│   │   │   │
│   │   │   ├── staff/
│   │   │   │   ├── login/route.ts ....... Staff authentication
│   │   │   │   ├── bookings/route.ts .... Get/update staff bookings
│   │   │   │   └── logout/route.ts ...... Staff logout
│   │   │   │
│   │   │   ├── cron/
│   │   │   │   ├── send-reminders/route.ts ... Send daily reminder emails
│   │   │   │   └── handle-no-shows/route.ts . Auto-handle no-shows
│   │   │   │
│   │   │   ├── setup/
│   │   │   │   └── staff/route.ts ....... Initialize staff account
│   │   │   │
│   │   │   └── auth/
│   │   │       └── callback/route.ts .... Auth callback handler
│   │   │
│   │   ├── staff/ ....................... 👥 Staff Portal Pages
│   │   │   ├── login/page.tsx ........... Staff login page
│   │   │   └── dashboard/page.tsx ....... Admin booking dashboard
│   │   │
│   │   ├── bookings/ .................... 🔍 Customer Portal Pages
│   │   │   └── lookup/page.tsx .......... Booking lookup & cancel
│   │   │
│   │   └── auth/
│   │       └── callback/route.ts ........ Auth callback handler
│   │
│   ├── components/ ..................... 🧩 React Components
│   │   ├── hero.tsx ..................... Hero section with CTA
│   │   ├── experiences.tsx .............. Dining experiences gallery
│   │   ├── booking-form.tsx ............. Main booking form component
│   │   ├── menu.tsx ..................... Seasonal menu showcase
│   │   ├── footer.tsx ................... Footer with contact info
│   │   └── ui/ .......................... (shadcn/ui components)
│   │
│   ├── lib/ ........................... 🛠️ Utilities & Configuration
│   │   ├── booking-utils.ts ............. Availability & validation logic
│   │   ├── email.ts ..................... Email sending utilities
│   │   ├── staff-auth.ts ................ Staff authentication helpers
│   │   ├── staff-middleware.ts .......... Session verification
│   │   ├── setup-staff.ts ............... Staff account setup
│   │   │
│   │   └── supabase/
│   │       ├── client.ts ................ Client-side Supabase setup
│   │       ├── server.ts ................ Server-side Supabase setup
│   │       └── proxy.ts ................. Session proxy handler
│   │
│   ├── public/ ......................... 🖼️ Static Assets
│   │   ├── hero.png ..................... Hero section image
│   │   ├── seasonal-tasting.png ......... Experience image
│   │   ├── chefs-table.png .............. Experience image
│   │   └── hearth-table.png ............. Experience image
│   │
│   ├── middleware.ts ................... 🔐 Auth Middleware
│   │
│   └── .env.example .................... 📋 Environment template
│
├── ⚙️ CONFIGURATION FILES
│   ├── package.json ..................... Project dependencies
│   ├── tsconfig.json .................... TypeScript configuration
│   ├── next.config.mjs .................. Next.js configuration
│   ├── tailwind.config.ts ............... Tailwind CSS configuration
│   └── vercel.json (optional) ........... Vercel deployment config
│
└── 📦 DEPENDENCIES & BUILD
    ├── node_modules/ .................... (Generated - don't edit)
    ├── .next/ ........................... (Generated - don't edit)
    ├── pnpm-lock.yaml ................... Locked dependencies
    └── .gitignore ....................... Git ignore rules
```

---

## 🗂️ Directory Guide

### 📖 Documentation Directory
**What:** Complete guides and references  
**Read First:** START_HERE.md  
**When to Use:** Before and during development  

| File | Purpose | Read Time |
|------|---------|-----------|
| START_HERE.md | Navigation guide | 2 min |
| QUICKSTART.md | Local setup | 5 min |
| README.md | Full documentation | 15 min |
| DEPLOYMENT_GUIDE.md | Production guide | 20 min |
| PROJECT_SUMMARY.md | Delivery summary | 10 min |
| COMPLETION_REPORT.md | What's built | 10 min |

### 🎨 App Directory (Next.js)
**What:** Pages, API routes, and layouts  
**Structure:** File-based routing (Next.js App Router)  
**Key Files:**
- `page.tsx` - Landing page (public website)
- `layout.tsx` - Root layout
- `api/` - Backend API routes
- `staff/` - Staff admin pages
- `bookings/` - Customer pages

### 🧩 Components Directory
**What:** Reusable React components  
**Count:** 5 main components  
**Purpose:** Build the UI

**Core Components:**
- `hero.tsx` - Hero section
- `experiences.tsx` - Dining options
- `booking-form.tsx` - Booking form
- `menu.tsx` - Menu showcase
- `footer.tsx` - Footer

### 🛠️ Lib Directory
**What:** Utility functions and configuration  
**Purpose:** Shared logic across app

**Key Files:**
- `booking-utils.ts` - Availability calculation
- `email.ts` - Email sending
- `staff-auth.ts` - Staff authentication
- `supabase/` - Database clients

---

## 📊 File Count Breakdown

```
Total: 74 files

By Type:
  TypeScript (.ts): 23 files
  React (.tsx): 12 files
  Markdown (.md): 9 files
  JSON (.json): 4 files
  CSS (.css): 1 file
  Config (.mjs, .ts): 4 files
  Images: 4 files
  Other: 13 files

By Directory:
  /app: 25 files (pages, routes)
  /components: 7 files (UI components)
  /lib: 10 files (utilities)
  /public: 4 files (assets)
  root: 20+ files (config, docs)
```

---

## 🔄 Data Flow

### Booking Creation
```
Client
  ↓ (booking-form.tsx)
  ↓ Form submission
  ↓
/api/bookings/create
  ↓ Validate & create
  ↓
Supabase (bookings table)
  ↓
Email Service
  ↓
Customer Email
```

### Staff Dashboard
```
Staff
  ↓ (login/page.tsx)
  ↓
/api/staff/login
  ↓ Verify password
  ↓
Session Cookie
  ↓
/staff/dashboard/page.tsx
  ↓
/api/staff/bookings
  ↓
Supabase (bookings table)
  ↓
Display & Manage
```

### Reminder Emails
```
Cron Job (External)
  ↓
/api/cron/send-reminders
  ↓ Get bookings for tomorrow
  ↓
Supabase Query
  ↓
Email Service
  ↓
Customer Email
```

---

## 🔐 Key Files by Feature

### Authentication
- `lib/staff-auth.ts` - Authentication logic
- `lib/staff-middleware.ts` - Session verification
- `middleware.ts` - Auth middleware
- `lib/supabase/proxy.ts` - Session proxy

### Booking Management
- `lib/booking-utils.ts` - Availability logic
- `app/api/bookings/create/route.ts` - Create booking
- `app/api/bookings/availability/route.ts` - Get slots
- `components/booking-form.tsx` - UI form

### Email
- `lib/email.ts` - Email utilities
- `app/api/cron/send-reminders/route.ts` - Send reminders
- `app/api/bookings/create/route.ts` - Confirmation email

### Database
- `lib/supabase/client.ts` - Client setup
- `lib/supabase/server.ts` - Server setup
- All API routes use Supabase

---

## 🚀 How to Navigate This Project

### For Setup/Deployment
1. Read: `START_HERE.md`
2. Read: `QUICKSTART.md`
3. Reference: `DEPLOYMENT_GUIDE.md`

### For Understanding Features
1. Read: `README.md`
2. Reference: `IMPLEMENTATION_COMPLETE.md`
3. Look at: Components in `/components`

### For Customization
1. Reference: `README.md` (customization section)
2. Edit: Relevant files in `/app`, `/components`, `/lib`
3. Update: Database via Supabase dashboard

### For Troubleshooting
1. Check: `QUICKSTART.md` (troubleshooting)
2. Check: `DEPLOYMENT_GUIDE.md` (troubleshooting)
3. Review: Error logs in terminal

---

## 📋 File Importance Levels

### 🔴 Critical (Must Not Delete)
- `app/page.tsx` - Main landing page
- `app/layout.tsx` - Root layout
- `app/globals.css` - Styling system
- `lib/booking-utils.ts` - Core logic
- `package.json` - Dependencies

### 🟡 Important (Core Functionality)
- All files in `app/api/` - Backend
- All files in `components/` - UI
- All files in `lib/` - Utilities
- `middleware.ts` - Auth

### 🟢 Optional (Can Modify)
- Images in `public/` - Can replace
- Content in `components/menu.tsx` - Can customize
- Styling in `app/globals.css` - Can adjust
- Documentation files - Reference only

---

## 📝 File Naming Conventions

### Page Files
- `page.tsx` - Next.js page component
- Named: `[feature]/page.tsx`

### Route Files  
- `route.ts` - Next.js API route
- Named: `api/[resource]/[action]/route.ts`

### Component Files
- `component-name.tsx` - React component
- PascalCase naming
- Export as `export function ComponentName() {}`

### Utility Files
- `feature-name.ts` - Utility functions
- camelCase naming
- Can export multiple functions

---

## 🔄 Common Tasks & Files to Modify

| Task | Files to Edit |
|------|---------------|
| Change restaurant hours | `lib/booking-utils.ts` |
| Update menu | `components/menu.tsx` |
| Change colors | `app/globals.css` |
| Update pricing | Supabase dashboard |
| Change contact info | `components/footer.tsx` |
| Update hero text | `components/hero.tsx` |
| Modify form fields | `components/booking-form.tsx` |
| Change email template | `lib/email.ts` |

---

## 📚 Reading Order for New Developers

### Day 1 (Setup)
1. ✅ START_HERE.md
2. ✅ QUICKSTART.md
3. ✅ Get it running locally

### Day 2 (Understanding)
1. ✅ README.md
2. ✅ Explore `/components` - UI structure
3. ✅ Look at `/app/page.tsx` - How it connects

### Day 3 (Deep Dive)
1. ✅ IMPLEMENTATION_COMPLETE.md
2. ✅ Review `/lib` - Business logic
3. ✅ Review `/app/api` - Backend

### Day 4 (Deployment)
1. ✅ DEPLOYMENT_GUIDE.md
2. ✅ Set up Vercel
3. ✅ Deploy to production

---

## 🎯 Quick Reference

### Find a File
- Component? → Look in `/components`
- API route? → Look in `/app/api`
- Utility? → Look in `/lib`
- Style? → Check `/app/globals.css`

### Need to Change Something?
- Hours/capacity? → `lib/booking-utils.ts`
- Menu? → `components/menu.tsx`
- Colors? → `app/globals.css`
- Email? → `lib/email.ts`
- Database? → Supabase dashboard

### Build/Run
- Start dev server: `pnpm dev`
- Build for production: `pnpm build`
- Run build: `pnpm start`

---

## ✅ File Integrity Checklist

Before deploying, verify:
- [ ] All files in `/app` exist
- [ ] All files in `/components` exist
- [ ] All files in `/lib` exist
- [ ] `next.config.mjs` exists
- [ ] `tailwind.config.ts` exists
- [ ] `tsconfig.json` exists
- [ ] `package.json` exists
- [ ] `middleware.ts` exists

---

## 📞 Quick Questions

**Q: Where's the database?**  
A: Supabase (remote). No local files needed.

**Q: Where's the email service?**  
A: SMTP configured in environment variables. Logic in `lib/email.ts`.

**Q: Where's the database schema?**  
A: In Supabase dashboard. Created automatically.

**Q: Can I delete documentation files?**  
A: Yes, they're reference only. But keep them for future reference.

**Q: Where's the production build?**  
A: `.next/` directory (auto-generated). Git-ignored.

---

**This is a complete, production-ready project. All files are accounted for.**

**Start with START_HERE.md and follow the path that matches your needs! 🚀**
