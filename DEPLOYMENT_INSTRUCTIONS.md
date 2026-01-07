# AccessScan AI - Vercel Deployment Instructions

## 🚀 Quick Deployment Guide

### Prerequisites
- A Vercel account (sign up at https://vercel.com if you don't have one)
- Your project files ready (already done ✅)

---

## Method 1: Deploy via Vercel Dashboard (Recommended)

### Step 1: Prepare Your Code
Your code is already ready in `/workspace/shadcn-ui`. You need to:

1. **Download your project files:**
   - Click the "Share" button in the top-right corner
   - Click "Export" to download all files as a ZIP
   - Extract the ZIP file on your computer

2. **Or push to GitHub:**
   ```bash
   # In your local terminal (not here)
   git init
   git add .
   git commit -m "Initial commit - AccessScan AI"
   git branch -M main
   git remote add origin https://github.com/YOUR_USERNAME/accessscan-ai.git
   git push -u origin main
   ```

### Step 2: Import to Vercel

1. Go to https://vercel.com/new
2. Sign in with GitHub (or your preferred method)
3. **If using GitHub:**
   - Click "Import Git Repository"
   - Select your repository
   - Click "Import"
4. **If using ZIP file:**
   - Click "Deploy" → "Upload files"
   - Drag and drop your extracted project folder

### Step 3: Configure Project Settings

Vercel will auto-detect the settings, but verify:
- **Framework Preset:** Vite
- **Build Command:** `pnpm run build`
- **Output Directory:** `dist`
- **Install Command:** `pnpm install`

### Step 4: Add Environment Variables

Click "Environment Variables" and add these TWO variables:

```
Name: VITE_SUPABASE_URL
Value: https://gdtpziynhssqytcnzvbi.supabase.co

Name: VITE_SUPABASE_ANON_KEY
Value: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6ImdkdHB6aXluaHNzcXl0Y256dmJpIiwicm9sZSI6ImFub24iLCJpYXQiOjE3NjU0ODE2MjMsImV4cCI6MjA4MTA1NzYyM30.ZXPy4-lN6UjPJioQA6ftmRf1D6x-qpZOnl5BUouNuoA
```

**Important:** Make sure to add these for ALL environments (Production, Preview, Development)

### Step 5: Deploy!

1. Click "Deploy"
2. Wait 2-3 minutes for the build
3. Your app will be live at `https://your-project-name.vercel.app`

---

## Method 2: Deploy via Vercel CLI (Alternative)

If you prefer command-line deployment:

1. **Install Vercel CLI:**
   ```bash
   npm i -g vercel
   ```

2. **Login:**
   ```bash
   vercel login
   ```

3. **Deploy:**
   ```bash
   cd /workspace/shadcn-ui
   vercel
   ```

4. **Follow the prompts:**
   - Set up and deploy? Yes
   - Which scope? (Select your account)
   - Link to existing project? No
   - Project name? accessscan-ai
   - Directory? ./
   - Override settings? No

5. **Add environment variables:**
   ```bash
   vercel env add VITE_SUPABASE_URL production
   # Paste: https://gdtpziynhssqytcnzvbi.supabase.co
   
   vercel env add VITE_SUPABASE_ANON_KEY production
   # Paste: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6ImdkdHB6aXluaHNzcXl0Y256dmJpIiwicm9sZSI6ImFub24iLCJpYXQiOjE3NjU0ODE2MjMsImV4cCI6MjA4MTA1NzYyM30.ZXPy4-lN6UjPJioQA6ftmRf1D6x-qpZOnl5BUouNuoA
   ```

6. **Redeploy with env vars:**
   ```bash
   vercel --prod
   ```

---

## 📋 Post-Deployment Checklist

After your site is live, complete these steps:

### 1. Update Stripe Success URL
- Go to your Stripe Dashboard
- Navigate to the checkout session settings
- Update success URL to: `https://your-vercel-domain.vercel.app/success?session_id={CHECKOUT_SESSION_ID}`

### 2. Update Supabase Redirect URLs
- Go to Supabase Dashboard → Authentication → URL Configuration
- Add your Vercel domain to "Site URL": `https://your-vercel-domain.vercel.app`
- Add to "Redirect URLs": `https://your-vercel-domain.vercel.app/**`

### 3. Test Everything
- ✅ Visit your live site
- ✅ Test website scanning with different URLs
- ✅ Sign up for a new account
- ✅ Test Pro upgrade with test card: `4242 4242 4242 4242`
- ✅ Verify Pro badge appears after payment
- ✅ Test sign out and sign in

### 4. Switch to Live Mode (When Ready)
Currently using Stripe test mode. To accept real payments:
- Get your live Stripe keys from Stripe Dashboard
- Update environment variables in Supabase:
  - `STRIPE_SECRET_KEY` (live key)
  - `APP_4051991953_STRIPE_PRO_PRICE_ID` (live price ID)
- Update webhook endpoint in Stripe to use live mode

---

## 🎯 What You Need From Me

To complete the deployment, I need you to:

1. **Choose your deployment method:**
   - Method 1 (Dashboard) - Easier, visual interface
   - Method 2 (CLI) - Faster, command-line

2. **After deployment, share:**
   - Your Vercel domain URL (e.g., `https://accessscan-ai.vercel.app`)
   - So I can help you verify everything is working correctly

---

## 🆘 Troubleshooting

**Build fails?**
- Check that environment variables are set correctly
- Verify the build command is `pnpm run build`

**App loads but features don't work?**
- Verify environment variables are added to ALL environments
- Check browser console for errors

**Stripe checkout fails?**
- Verify webhook URL is updated in Stripe Dashboard
- Check that success URL includes your Vercel domain

**Need help?**
- Just ask me! I'm here to help troubleshoot any issues.

---

## 📊 Your Current Setup

✅ **Frontend:** Ready to deploy (1,111.74 kB)
✅ **Backend:** Supabase Edge Functions deployed
✅ **Database:** PostgreSQL table created
✅ **Payment:** Stripe configured (test mode)
✅ **Authentication:** Supabase Auth enabled

**All systems ready for production! 🚀**