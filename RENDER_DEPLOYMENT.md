# Render Deployment Guide

## 🚀 Complete Deployment Setup for Render

### Prerequisites
- GitHub repository: https://github.com/alGo-explO/Capstone_Sem3.git
- Render account (https://render.com)
- Neon PostgreSQL database

---

## 📦 Backend Deployment (Web Service)

### Step 1: Create Backend Web Service on Render

1. Go to Render Dashboard → New → Web Service
2. Connect your GitHub repository: `alGo-explO/Capstone_Sem3`
3. Configure the service:

**Basic Settings:**
- **Name**: `capstone-backend` (or your preferred name)
- **Region**: Choose closest to your users
- **Branch**: `main`
- **Root Directory**: `backend`
- **Runtime**: `Node`

**Build & Deploy Settings:**
- **Build Command**: 
  ```bash
  npm install && npx prisma generate && npx prisma migrate deploy
  ```

- **Start Command**: 
  ```bash
  node server.js
  ```

### Step 2: Backend Environment Variables

Add these in Render Dashboard → Environment:

```env
DATABASE_URL=postgresql://<user>:<password>@<host>.neon.tech/<dbname>?sslmode=require&channel_binding=require

JWT_SECRET=your-very-secure-random-jwt-secret-key-here-make-it-long-and-random-at-least-32-chars

GEMINI_API_KEY=your-google-gemini-api-key-here

NODE_ENV=production

PORT=10000
```

**⚠️ IMPORTANT**: 
- Change `JWT_SECRET` to a secure random string (use a password generator)
- Keep your actual `DATABASE_URL` and `GEMINI_API_KEY` secure

---

## 🎨 Frontend Deployment (Static Site)

### Step 1: Create Frontend Static Site on Render

1. Go to Render Dashboard → New → Static Site
2. Connect your GitHub repository: `alGo-explO/Capstone_Sem3`
3. Configure the service:

**Basic Settings:**
- **Name**: `capstone-frontend` (or your preferred name)
- **Region**: Choose closest to your users
- **Branch**: `main`
- **Root Directory**: `frontend`

**Build & Deploy Settings:**
- **Build Command**: 
  ```bash
  npm install && npm run build
  ```

- **Publish Directory**: 
  ```
  dist
  ```

### Step 2: Frontend Environment Variables

Add this in Render Dashboard → Environment:

```env
VITE_API_URL=https://capstone-sem3-12k8.onrender.com
```

---

## 📋 Build Logs to Monitor

### Backend Build Log (Expected Output)
```
==> Cloning from https://github.com/alGo-explO/Capstone_Sem3...
==> Checking out commit xxxxx in branch main
==> Running build command 'npm install && npx prisma generate && npx prisma migrate deploy'...

> npm install
added 150 packages in 15s

> npx prisma generate
✔ Generated Prisma Client

> npx prisma migrate deploy
Applying migration `20251118095607_init`
Applying migration `20251202164036_add_books_model`
Applying migration `20251202165132_enhanced_reading_hub`
Applying migration `20251203101003_add_book_notes_review_rating_fixed`

The following migration(s) have been applied:
migrations/
  └─ 20251118095607_init/
  └─ 20251202164036_add_books_model/
  └─ 20251202165132_enhanced_reading_hub/
  └─ 20251203101003_add_book_notes_review_rating_fixed/

==> Build successful 🎉
==> Starting service with 'node server.js'...
Server running on port 10000
```

### Frontend Build Log (Expected Output)
```
==> Cloning from https://github.com/alGo-explO/Capstone_Sem3...
==> Checking out commit xxxxx in branch main
==> Running build command 'npm install && npm run build'...

> npm install
added 132 packages in 8s

> npm run build
vite v5.0.8 building for production...
✓ 150 modules transformed.
dist/index.html                   0.45 kB
dist/assets/index-xxxxx.css      12.34 kB
dist/assets/index-xxxxx.js      145.67 kB

✓ built in 5.23s

==> Build successful 🎉
==> Deploying...
==> Deploy successful! Live at https://capstone-frontend.onrender.com
```

---

## 🔧 Alternative Build Commands (If Issues Occur)

### Backend Alternative Build Commands:

**Option 1** (Recommended - with migrations):
```bash
npm install && npx prisma generate && npx prisma migrate deploy
```

**Option 2** (If migrations fail - push schema directly):
```bash
npm install && npx prisma generate && npx prisma db push
```

**Option 3** (Minimal - no migrations):
```bash
npm install && npx prisma generate
```

### Frontend Alternative Build Commands:

**Option 1** (Standard):
```bash
npm install && npm run build
```

**Option 2** (Clean install):
```bash
rm -rf node_modules && npm install && npm run build
```

---

## ✅ Post-Deployment Verification

### 1. Test Backend Health
```bash
curl https://capstone-sem3-12k8.onrender.com/health
```
Expected response: `{"status":"ok"}`

### 2. Test Backend API
```bash
curl https://capstone-sem3-12k8.onrender.com/api/books
```

### 3. Test Frontend
Open in browser: `https://capstone-frontend.onrender.com`

---

## 🐛 Troubleshooting

### Backend Issues:

**Build fails at Prisma migrate:**
- Check DATABASE_URL is correct
- Try using `npx prisma db push` instead of `npx prisma migrate deploy`

**Server won't start:**
- Check all environment variables are set
- Verify PORT is set to 10000
- Check build logs for errors

**Database connection errors:**
- Verify DATABASE_URL includes `?sslmode=require`
- Check Neon database is active
- Ensure IP restrictions are disabled on Neon

### Frontend Issues:

**Build fails:**
- Check if VITE_API_URL is set correctly
- Try clearing cache: Add `rm -rf node_modules` before npm install

**API calls fail:**
- Verify VITE_API_URL matches your backend URL
- Check CORS settings in backend
- Ensure backend is deployed and running

**Blank page:**
- Check browser console for errors
- Verify dist folder was created during build
- Check publish directory is set to `dist`

---

## 📝 Environment Variables Summary

### Backend (.env for Render):
```env
DATABASE_URL=postgresql://<user>:<password>@<host>.neon.tech/<dbname>?sslmode=require&channel_binding=require
JWT_SECRET=your-secure-secret-key-min-32-characters
GEMINI_API_KEY=your-google-gemini-api-key-here
NODE_ENV=production
PORT=10000
```

### Frontend (.env for Render):
```env
VITE_API_URL=https://capstone-sem3-12k8.onrender.com
```

---

## 🔄 Redeployment

To redeploy after code changes:

1. Push to GitHub:
   ```bash
   git add .
   git commit -m "Your changes"
   git push origin main
   ```

2. Render will auto-deploy (if auto-deploy is enabled)
   OR manually trigger deploy from Render Dashboard

---

## 📞 Support

If you encounter issues:
1. Check Render logs in Dashboard → Logs
2. Verify all environment variables are set
3. Check GitHub repository is connected
4. Ensure build commands are correct

---

## 🎯 Quick Checklist

- [ ] Backend service created on Render
- [ ] Backend environment variables set (5 variables)
- [ ] Backend build command configured
- [ ] Backend deployed successfully
- [ ] Frontend static site created on Render
- [ ] Frontend environment variable set (VITE_API_URL)
- [ ] Frontend build command configured
- [ ] Frontend deployed successfully
- [ ] Backend health check passes
- [ ] Frontend loads in browser
- [ ] Can create account and login
- [ ] Can add books and view library

---

**Your URLs:**
- Backend: `https://capstone-sem3-12k8.onrender.com`
- Frontend: `https://capstone-frontend.onrender.com` (replace with your actual frontend URL)
