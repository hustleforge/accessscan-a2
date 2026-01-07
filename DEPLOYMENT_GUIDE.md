# AccessScan AI - Complete Deployment Guide

## 🎯 Get Your App Selling in 30 Minutes!

Follow these steps to deploy your complete accessibility scanning SaaS with Stripe payments.

---

## ✅ STEP 1: Set Up Supabase Database (5 minutes)

1. Go to your Supabase dashboard: https://supabase.com/dashboard/project/gdtpziynhssqytcnzvbi

2. Click **SQL Editor** in the left sidebar

3. Click **New Query**

4. Copy and paste this SQL script:

```sql
BEGIN;

-- Create subscriptions table
CREATE TABLE IF NOT EXISTS app_b0bc1_subscriptions (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  user_id UUID REFERENCES auth.users NOT NULL UNIQUE,
  tier TEXT NOT NULL DEFAULT 'free' CHECK (tier IN ('free', 'pro', 'enterprise')),
  stripe_customer_id TEXT,
  stripe_subscription_id TEXT,
  status TEXT DEFAULT 'active' CHECK (status IN ('active', 'canceled', 'past_due')),
  current_period_end TIMESTAMP WITH TIME ZONE,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT TIMEZONE('utc'::text, NOW()) NOT NULL,
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT TIMEZONE('utc'::text, NOW()) NOT NULL
);

-- Create scan history table
CREATE TABLE IF NOT EXISTS app_b0bc1_scan_history (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  user_id UUID REFERENCES auth.users NOT NULL,
  url TEXT NOT NULL,
  score INTEGER NOT NULL,
  total_issues INTEGER NOT NULL,
  critical_issues INTEGER NOT NULL,
  serious_issues INTEGER NOT NULL,
  moderate_issues INTEGER NOT NULL,
  minor_issues INTEGER NOT NULL,
  scan_data JSONB NOT NULL,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT TIMEZONE('utc'::text, NOW()) NOT NULL
);

-- Create indexes
CREATE INDEX IF NOT EXISTS subscriptions_user_idx ON app_b0bc1_subscriptions(user_id);
CREATE INDEX IF NOT EXISTS subscriptions_stripe_customer_idx ON app_b0bc1_subscriptions(stripe_customer_id);
CREATE INDEX IF NOT EXISTS scan_history_user_idx ON app_b0bc1_scan_history(user_id);
CREATE INDEX IF NOT EXISTS scan_history_created_idx ON app_b0bc1_scan_history(created_at DESC);

-- Enable RLS
ALTER TABLE app_b0bc1_subscriptions ENABLE ROW LEVEL SECURITY;
ALTER TABLE app_b0bc1_scan_history ENABLE ROW LEVEL SECURITY;

-- RLS Policies for subscriptions
CREATE POLICY "allow_users_read_own_subscription" ON app_b0bc1_subscriptions 
  FOR SELECT TO authenticated 
  USING ((select auth.uid()) = user_id);

CREATE POLICY "allow_users_update_own_subscription" ON app_b0bc1_subscriptions 
  FOR UPDATE TO authenticated 
  USING ((select auth.uid()) = user_id);

-- RLS Policies for scan history
CREATE POLICY "allow_users_read_own_scans" ON app_b0bc1_scan_history 
  FOR SELECT TO authenticated 
  USING ((select auth.uid()) = user_id);

CREATE POLICY "allow_users_insert_own_scans" ON app_b0bc1_scan_history 
  FOR INSERT TO authenticated 
  WITH CHECK ((select auth.uid()) = user_id);

COMMIT;
```

5. Click **Run** (or press Ctrl+Enter)

6. You should see "Success. No rows returned" ✅

---

## 🔧 STEP 2: Configure Environment Variables (2 minutes)

Still in your Supabase dashboard:

1. Click **Settings** (gear icon) in the left sidebar
2. Click **Edge Functions**
3. Scroll to **Environment Variables** section
4. Add these 3 variables:

| Variable Name | Value |
|--------------|-------|
| `STRIPE_SECRET_KEY` | `mk_1SWoadJJQmBv69JrvVaJzkQy` |
| `APP_b0bc1_STRIPE_WEBHOOK_SECRET` | `whsec_DgJkOrYQ5fTalriIpduZiRFLFAvLwoph` |
| `APP_b0bc1_STRIPE_PRO_PRICE_ID` | `price_1SdFsvJJQmBv69JrmyHbp020` |

Click **Save** after adding each one.

---

## ⚡ STEP 3: Deploy Edge Functions (15 minutes)

### Option A: Using Supabase CLI (Recommended - Faster)

1. Install Supabase CLI:
```bash
npm install -g supabase
```

2. Login to Supabase:
```bash
supabase login
```

3. Link your project:
```bash
supabase link --project-ref gdtpziynhssqytcnzvbi
```

4. Deploy all 3 functions:
```bash
cd /workspace/shadcn-ui
supabase functions deploy app_b0bc1_stripe_checkout
supabase functions deploy app_b0bc1_stripe_webhook
supabase functions deploy app_b0bc1_scan_website
```

### Option B: Manual Upload via Dashboard

For each function:

1. Go to **Edge Functions** in Supabase dashboard
2. Click **Create Function**
3. Name it exactly as shown below
4. Copy the code from the corresponding file in `supabase/functions/[function-name]/index.ts`
5. Click **Deploy**

**Function 1:** `app_b0bc1_stripe_checkout`
- File: `supabase/functions/app_b0bc1_stripe_checkout/index.ts`

**Function 2:** `app_b0bc1_stripe_webhook`
- File: `supabase/functions/app_b0bc1_stripe_webhook/index.ts`

**Function 3:** `app_b0bc1_scan_website`
- File: `supabase/functions/app_b0bc1_scan_website/index.ts`

---

## 💳 STEP 4: Configure Stripe Webhook (3 minutes)

1. Go to https://dashboard.stripe.com/test/webhooks

2. Click **Add endpoint**

3. Enter this URL:
```
https://gdtpziynhssqytcnzvbi.supabase.co/functions/v1/app_b0bc1_stripe_webhook
```

4. Select these events:
   - ✅ `checkout.session.completed`
   - ✅ `customer.subscription.updated`
   - ✅ `customer.subscription.deleted`

5. Click **Add endpoint**

---

## 🌐 STEP 5: Deploy Frontend (5 minutes)

1. In MGX platform, click the **Publish** button (top-right corner)

2. Your app will be live at the generated URL!

3. Share the link and start getting customers! 🎉

---

## 🎊 YOU'RE DONE! Your App is Live!

### What You Have Now:

✅ **User Authentication** - Sign up, login, logout
✅ **Accessibility Scanning** - Real axe-core WCAG 2.1 AA compliance checking
✅ **Stripe Payments** - Pro subscriptions at $49/month
✅ **Scan Limits** - Free: 5 scans/month, Pro: 100 scans/month
✅ **Scan History** - All scans saved to database
✅ **Automatic Billing** - Webhooks handle subscription updates

### Test Your Payment Flow:

1. Create a test account on your deployed site
2. Click "Upgrade to Pro"
3. Use Stripe test card: `4242 4242 4242 4242`
4. Expiry: Any future date
5. CVC: Any 3 digits
6. ZIP: Any 5 digits

---

## 📱 Next Steps (Optional):

### Add More Features:
- Dashboard page showing scan history
- Email notifications for scan results
- Team collaboration features
- API access for Pro users

### Go Live with Real Payments:
1. Switch Stripe from test mode to live mode
2. Update the `STRIPE_SECRET_KEY` in Supabase to your live key
3. Update webhook endpoint to use live webhook secret
4. You're accepting real payments! 💰

---

## 🆘 Need Help?

If you run into any issues:
1. Check Supabase logs: Dashboard → Edge Functions → Logs
2. Check Stripe webhook logs: Dashboard → Webhooks → [Your endpoint]
3. Ask me for help! Just describe what's not working.

---

## 🚀 Marketing Tips to Get Your First Customers:

1. **Post on Reddit** - r/webdev, r/accessibility, r/SaaS
2. **Post on Twitter/X** - Tag #accessibility #WCAG #a11y
3. **ProductHunt Launch** - Great for initial traction
4. **Reach out to agencies** - Web design agencies need this tool
5. **SEO** - Blog about web accessibility compliance

**You've built something valuable! Now go sell it!** 💪