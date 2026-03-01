# 📚 Kantri Lawyer — Digital Learning & Book Sales Platform

A full-stack Next.js web application for online legal education. Supports video courses, eBooks, live classes, physical book sales, user dashboards, and an admin panel.

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Frontend | Next.js 16, React 19, Framer Motion |
| Backend | Next.js API Routes (Node.js) |
| Database | PostgreSQL (via Prisma ORM) |
| Auth | Custom JWT-based (bcrypt password hashing) |
| Payments | Razorpay |
| Styling | Vanilla CSS (no Tailwind) |
| File Uploads | Local disk (`/public/uploads/`) — see notes below |

---

## ✅ System Requirements

- **Node.js** v18.x or v20.x (recommended: v20 LTS)
- **PostgreSQL** v14 or higher
- **npm** v9+

---

## 🚀 Quick Setup (Step by Step)

### Step 1 — Clone / Unzip the Project
```bash
# If you have it as a zip, unzip it, then:
cd "didgital learning and book sales platform"
```

### Step 2 — Install Dependencies
```bash
npm install
```

### Step 3 — Set Up Environment Variables
Copy the example env file and fill in your values:
```bash
cp .env.example .env
```
Then edit `.env` with your actual database URL, Razorpay keys, etc.  
See the `.env.example` file for all required variables with descriptions.

### Step 4 — Set Up the Database
```bash
# Push the schema to your PostgreSQL database
npm run db:push

# Seed the admin user (IMPORTANT — do this once)
npm run db:seed
```

**Default Admin Credentials (change after first login!):**
- Email: `admin@kantrilawyer.com`
- Password: `admin@kantri2026`

### Step 5 — Build for Production
```bash
npm run build
```

### Step 6 — Start the Production Server
```bash
npm start
# App runs on http://localhost:3000
```

---

## 📁 Project Structure

```
├── app/                    # Next.js App Router pages & API routes
│   ├── (pages)/           # Public pages (home, courses, ebooks, etc.)
│   ├── admin/             # Admin panel
│   ├── dashboard/         # User dashboard
│   └── api/               # All backend API routes
├── components/            # Reusable React components
├── context/               # React Context (Auth, Cart)
├── lib/                   # Utility functions (auth, db, prisma)
├── prisma/
│   ├── schema.prisma      # Database schema
│   └── seed.js            # Admin user seed script
├── public/
│   └── uploads/           # Uploaded images (local disk)
├── .env.example           # Environment variable template
└── README.md
```

---

## 🌐 Hosting Options

This app can be hosted on **any platform that supports Node.js**:

### Option A — Traditional VPS (AWS EC2, Azure VM, DigitalOcean, etc.)
1. Install Node.js 20 and PostgreSQL on the server
2. Clone/upload the project
3. Follow the Quick Setup steps above
4. Use **PM2** to keep the app running:
   ```bash
   npm install -g pm2
   pm2 start npm --name "kantri" -- start
   pm2 startup && pm2 save
   ```
5. Use **Nginx** as a reverse proxy on port 80/443
6. Install **Certbot** for free SSL

### Option B — Vercel (Easiest for Next.js)
1. Push code to a GitHub repository
2. Connect the repo to Vercel
3. Add all environment variables in Vercel's dashboard
4. Deploy — Vercel handles everything automatically
> ⚠️ **Note:** Image uploads to local disk won't persist on Vercel. You'll need to switch the upload API to use cloud storage (S3, Azure Blob, Cloudinary).

### Option C — Docker
A `Dockerfile` can be added on request. The app is compatible with Docker/containers.

---

## ⚠️ Important Notes for Hosting Team

### 1. Image Uploads (Read This!)
Currently, uploaded images are stored at `/public/uploads/images/` (local disk).

- ✅ **Works fine** on traditional VPS/EC2/Azure VM
- ❌ **Will NOT persist** on serverless platforms (Vercel, AWS Lambda) or containers that reset on deploy

**If using serverless/containers:** Replace `app/api/upload/image/route.js` with an S3/Azure Blob/Cloudinary upload implementation.

### 2. Database Migrations
On first deploy, run:
```bash
npx prisma migrate deploy  # if using migrations
# OR
npx prisma db push          # simpler alternative
node prisma/seed.js         # creates the admin account
```

### 3. Prisma Client Generation
The `postinstall` script in `package.json` runs `prisma generate` automatically after `npm install`. No manual step needed.

### 4. Required Open Ports
- Port **3000** — Next.js app (or use Nginx reverse proxy on 80/443)
- Port **5432** — PostgreSQL (keep internal/private, not exposed to internet)

### 5. Change the Admin Password
After first login with the seeded credentials, immediately change the admin password via the Admin Panel → Profile settings.

---

## 🔑 Environment Variables Summary

All variables are documented in `.env.example`. The minimum required to run the app:

| Variable | Required | Description |
|---|---|---|
| `DATABASE_URL` | ✅ REQUIRED | PostgreSQL connection string |
| `NEXT_PUBLIC_RAZORPAY_KEY` | For payments | Razorpay publishable key |
| `RAZORPAY_SECRET` | For payments | Razorpay secret key |
| `SMTP_HOST` | For email | Mail server hostname |
| `SMTP_USER` | For email | Mail server username |
| `SMTP_PASS` | For email | Mail server password |
| `SMTP_FROM` | For email | Sender email address |
| `NODE_ENV` | Recommended | Set to `production` |

---

## 📦 NPM Scripts

| Command | Description |
|---|---|
| `npm run dev` | Start development server |
| `npm run build` | Build for production |
| `npm start` | Start production server |
| `npm run db:push` | Sync DB schema (no migration files) |
| `npm run db:seed` | Create the admin user |
| `npm run db:studio` | Open Prisma Studio (DB GUI) |

---

## 🧪 Testing the Setup

After deploying, verify these pages work:
- `https://yourdomain.com/` — Homepage
- `https://yourdomain.com/courses` — Course listing
- `https://yourdomain.com/admin` — Admin panel (login with seeded credentials)
- `https://yourdomain.com/api/data?type=courses` — Should return JSON (empty array if no courses added yet)

---

## 📞 Support

For questions about the codebase, contact the development team.
