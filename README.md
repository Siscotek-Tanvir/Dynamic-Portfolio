# Dynamic MERN Stack Developer Portfolio

[![MERN Stack](https://img.shields.io/badge/Stack-MERN-blue.svg)](https://reactjs.org/)
[![RTK Query](https://img.shields.io/badge/State-RTK%20Query-purple.svg)](https://redux-toolkit.js.org/rtk-query/overview)
[![TailwindCSS](https://img.shields.io/badge/Styling-TailwindCSS%203-38bdf8.svg)](https://tailwindcss.com/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

A high-performance, production-ready, fully dynamic Full Stack Developer Portfolio built with the **MERN** stack (MongoDB, Express, React, Node.js). 

Every single section on the frontend is 100% backend-driven and can be updated in real time by the administrator via the protected `/admin` dashboard.

---

## 🌟 Key Architectural Features

- **Strict RTK Query Communication**: Zero `axios`, `fetch`, or `createAsyncThunk` for data operations. Handled via Redux Toolkit Query with automatic cache invalidation, tag updates, and token rotation interceptors.
- **URL-Based Media (Zero Uploads / No Cloudinary / No Multer)**: All media items (profile photo, project thumbnails, company logos, peer avatars, blog covers, icons) are entered as direct URLs with instant live previews and graceful error fallbacks.
- **Robust Security & Defense-in-Depth**:
  - JWT token architecture (15m in-memory/localStorage access token + 7d HTTP-only refresh cookie).
  - Rate limiting on sensitive endpoints (`/auth/login` and `/contact`).
  - NoSQL injection prevention with `express-mongo-sanitize`.
  - Content Security Policies & header hardening with `helmet`.
  - Server-side payload validation via `express-validator` and client-side via `react-hook-form` + `zod`.
- **Modern UI & Responsive Polish**:
  - Light / Dark mode with system preference auto-detection and persistence.
  - Smooth micro-interactions and typewriter roles with Framer Motion.
  - Loading skeleton placeholders across all query states.
  - Dynamic SEO meta tags via `react-helmet-async`, `robots.txt`, and `sitemap.xml`.

---

## 📁 Repository Structure

```
dynamic-portfolio/
├── client/
│   ├── public/
│   │   ├── robots.txt
│   │   └── sitemap.xml
│   ├── src/
│   │   ├── app/
│   │   │   ├── api/apiSlice.js    <-- RTK Query baseQueryWithReauth & all endpoints
│   │   │   └── store.js           <-- configureStore with RTK Query middleware
│   │   ├── components/
│   │   │   ├── common/            <-- Button, Input, Textarea, Modal, ImagePreview, Skeleton...
│   │   │   ├── layout/            <-- Navbar, Footer, Sidebar, AdminLayout
│   │   │   └── sections/          <-- Hero, About, Skills, Projects, Experience, Testimonials, Blog, Contact
│   │   ├── features/              <-- authSlice, themeSlice
│   │   ├── hooks/                 <-- useAuth, useTheme
│   │   ├── pages/
│   │   │   ├── Home.jsx           <-- Public portfolio page
│   │   │   ├── Login.jsx          <-- Split-panel admin login
│   │   │   ├── NotFound.jsx       <-- 404 page
│   │   │   └── admin/             <-- Dashboard, Projects, Skills, Experience, Education, Testimonials, Messages, About, Settings
│   │   ├── routes/                <-- AppRoutes, ProtectedRoute
│   │   ├── utils/                 <-- imageFallback helpers
│   │   ├── App.jsx
│   │   └── main.jsx
│   ├── package.json
│   ├── tailwind.config.js
│   └── vite.config.js
│
├── server/
│   ├── src/
│   │   ├── config/                <-- db.js, env.js
│   │   ├── controllers/           <-- auth, about, skill, project, experience, education, testimonial, blog, contact, settings
│   │   ├── middlewares/           <-- authMiddleware, adminMiddleware, errorHandler, rateLimiter, validate
│   │   ├── models/                <-- User, About, Skill, Project, Experience, Education, Testimonial, Blog, Contact, Settings
│   │   ├── routes/                <-- API route handlers
│   │   ├── seed/seedData.js       <-- Complete database seeder
│   │   ├── utils/                 <-- ApiError, ApiResponse, asyncHandler, generateTokens, sendEmail
│   │   ├── validators/            <-- Express-validator validation schemas
│   │   ├── app.js
│   │   └── server.js
│   └── package.json
│
├── README.md
├── .gitignore
└── package.json (root orchestration)
```

---

## ⚙️ Environment Configuration

### Backend (`server/.env`)
```ini
PORT=5000
NODE_ENV=development
MONGO_URI=mongodb://localhost:27017/dynamic-portfolio
JWT_ACCESS_SECRET=portfolio_jwt_access_secret_super_secure_key_2026_xyz
JWT_REFRESH_SECRET=portfolio_jwt_refresh_secret_super_secure_key_2026_xyz
JWT_ACCESS_EXPIRY=15m
JWT_REFRESH_EXPIRY=7d
CLIENT_URL=http://localhost:5173
EMAIL_HOST=smtp.mailtrap.io
EMAIL_PORT=2525
EMAIL_USER=
EMAIL_PASS=
```

### Frontend (`client/.env`)
```ini
VITE_API_BASE_URL=http://localhost:5000/api/v1
```

---

## 🚀 How to Run the Application

### 1. Install Dependencies
In the root directory or per folder:
```bash
# In server directory:
cd server
npm install

# In client directory:
cd ../client
npm install
```

### 2. Seed Database
Make sure your MongoDB server is running (e.g. locally on port 27017 or a MongoDB Atlas URI configured in `server/.env`).
```bash
cd server
npm run seed
```
This populates:
- 1 Admin account: `admin@portfolio.com` / `Admin@123`
- Full profile bio and social links
- 8 Technical skills with icons
- 4 Featured projects
- 3 Career experience records
- 2 Education entries
- 3 Client endorsements
- 2 Blog publications
- Site settings & logos

### 3. Start Development Servers
Run backend:
```bash
cd server
npm run dev
```
Run frontend:
```bash
cd client
npm run dev
```

Visit the application:
- **Public Portfolio**: [http://localhost:5173](http://localhost:5173)
- **Admin Dashboard**: [http://localhost:5173/admin](http://localhost:5173/admin)
- **Admin Login**: [http://localhost:5173/login](http://localhost:5173/login)

---

## 🔐 Admin Authentication Details
- **Email**: `admin@portfolio.com`
- **Password**: `Admin@123`

---

## 🌐 API Reference Overview

Base path: `/api/v1`

| Resource | Method | Endpoint | Auth Required | Description |
|---|---|---|---|---|
| **Auth** | POST | `/auth/login` | No | Login and receive access token + refresh cookie |
| | POST | `/auth/logout` | No | Clear refresh token |
| | POST | `/auth/refresh-token` | No | Renew access token on 401 |
| | GET | `/auth/me` | Yes | Get active user profile |
| **About** | GET | `/about` | No | Fetch about data |
| | PUT | `/about` | Yes (Admin) | Update about section |
| **Skills** | GET | `/skills` | No | Get public skills list |
| | POST | `/skills` | Yes (Admin) | Create a new skill |
| | PUT | `/skills/:id` | Yes (Admin) | Update skill |
| | DELETE| `/skills/:id` | Yes (Admin) | Remove skill |
| **Projects**| GET | `/projects` | No | Filter and fetch projects |
| | POST | `/projects` | Yes (Admin) | Create new project |
| | PUT | `/projects/:id`| Yes (Admin) | Update project |
| | DELETE| `/projects/:id`| Yes (Admin) | Delete project |
| **Experience**| GET | `/experience`| No | List career timeline |
| | POST | `/experience`| Yes (Admin) | Add experience record |
| | PUT | `/experience/:id`| Yes (Admin) | Update experience record |
| | DELETE| `/experience/:id`| Yes (Admin) | Delete experience record |
| **Contact** | POST | `/contact` | No (Rate limited) | Send direct message |
| | GET | `/contact` | Yes (Admin) | View all messages |
| | PATCH| `/contact/:id/status` | Yes (Admin) | Update status |
| | DELETE| `/contact/:id` | Yes (Admin) | Delete message |
| **Stats** | GET | `/admin/stats` | Yes (Admin) | Aggregated counters & recent inquiries |

---

## 🖼️ How Images Work (Zero Upload Architecture)
No server disk space, Multer, or external cloud storage buckets are used.
Every image attribute accepts direct public URLs (Unsplash, CDN, company assets). 
When entering a URL in the Admin panel:
1. React Hook Form validates the string with Zod's `.url()`.
2. The `<ImagePreview />` component renders a live preview.
3. If the link fails or becomes invalid, an `onError` event handler immediately falls back to a graceful placeholder graphic, preventing broken image icons or disrupted layouts.

---

## 🚀 Production Deployment

- **Frontend**: Connect the `client` directory to [Vercel](https://vercel.com) or [Netlify](https://netlify.com) with build command `npm run build` and output directory `dist`. Set `VITE_API_BASE_URL` to your production API URL.
- **Backend**: Connect the `server` directory to [Render](https://render.com) or [Railway](https://railway.app). Set environment variables (`MONGO_URI`, `JWT_ACCESS_SECRET`, `JWT_REFRESH_SECRET`, `CLIENT_URL`).
- **Database**: Host on MongoDB Atlas free/dedicated M0+ cluster.

---

## 📄 License
MIT License. Built for production-ready MERN demonstration.
# Dynamic-Portfolio
