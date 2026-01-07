# AccessScan AI 🚀

An AI-powered website accessibility scanner that helps you identify and fix WCAG compliance issues. Built with React, TypeScript, Supabase, and Stripe.

![AccessScan AI](https://img.shields.io/badge/Status-Production%20Ready-brightgreen)
![License](https://img.shields.io/badge/License-MIT-blue)

## 🌟 Features

- **Real-time Accessibility Scanning** - Analyze any website for WCAG compliance issues
- **AI-Powered Analysis** - Intelligent detection of accessibility problems
- **Detailed Reports** - Export scan results as JSON
- **Pro Subscription** - Unlimited scans with Stripe payment integration
- **User Authentication** - Secure sign-up/sign-in with Supabase Auth
- **Responsive Design** - Beautiful UI built with shadcn-ui and Tailwind CSS

## 🛠️ Tech Stack

- **Frontend:** React 18, TypeScript, Vite
- **UI Components:** shadcn-ui, Tailwind CSS
- **Backend:** Supabase (Auth, Database, Edge Functions)
- **Payment:** Stripe (Checkout, Webhooks)
- **Deployment:** Vercel

## 🚀 Quick Start

### Prerequisites

- Node.js 18+ and pnpm
- Supabase account
- Stripe account (test mode)
- Vercel account

### Local Development

1. **Clone the repository:**
   ```bash
   git clone https://github.com/hustleforge/accessscan-ai.git
   cd accessscan-ai
   ```

2. **Install dependencies:**
   ```bash
   pnpm install
   ```

3. **Set up environment variables:**
   Create a `.env` file:
   ```env
   VITE_SUPABASE_URL=https://gdtpziynhssqytcnzvbi.supabase.co
   VITE_SUPABASE_ANON_KEY=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6ImdkdHB6aXluaHNzcXl0Y256dmJpIiwicm9sZSI6ImFub24iLCJpYXQiOjE3NjU0ODE2MjMsImV4cCI6MjA4MTA1NzYyM30.ZXPy4-lN6UjPJioQA6ftmRf1D6x-qpZOnl5BUouNuoA
   ```

4. **Run development server:**
   ```bash
   pnpm run dev
   ```

5. **Open browser:**
   Navigate to `http://localhost:5173`

## 📦 Deployment to Vercel

### Step 1: Import Project

1. Go to [Vercel](https://vercel.com/new)
2. Sign in with GitHub
3. Click "Import Project"
4. Select `hustleforge/accessscan-ai`
5. Click "Import"

### Step 2: Configure Environment Variables

Add these in Vercel's Environment Variables section:

```
VITE_SUPABASE_URL=https://gdtpziynhssqytcnzvbi.supabase.co
VITE_SUPABASE_ANON_KEY=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6ImdkdHB6aXluaHNzcXl0Y256dmJpIiwicm9sZSI6ImFub24iLCJpYXQiOjE3NjU0ODE2MjMsImV4cCI6MjA4MTA1NzYyM30.ZXPy4-lN6UjPJioQA6ftmRf1D6x-qpZOnl5BUouNuoA
```

**Important:** Add these for ALL environments (Production, Preview, Development)

### Step 3: Deploy

1. Click "Deploy"
2. Wait 2-3 minutes for build completion
3. Your app will be live at `https://your-project.vercel.app`

### Step 4: Post-Deployment Configuration

After deployment, update these settings:

**Supabase:**
- Go to Authentication → URL Configuration
- Add Site URL: `https://your-vercel-domain.vercel.app`
- Add Redirect URL: `https://your-vercel-domain.vercel.app/**`

**Stripe:**
- Update success URL in checkout settings
- Webhook endpoint is already configured at: `https://gdtpziynhssqytcnzvbi.supabase.co/functions/v1/app_4051991953_stripe_webhook`

## 🧪 Testing

### Test Accessibility Scanner
1. Enter any website URL (e.g., `https://example.com`)
2. Click "Scan Website"
3. View detailed accessibility report

### Test Pro Upgrade
1. Click "Get Started" on Pro plan
2. Use Stripe test card: `4242 4242 4242 4242`
3. Expiry: Any future date
4. CVC: Any 3 digits
5. Complete payment
6. Verify Pro badge appears in navigation

## 📊 Project Structure

```
accessscan-ai/
├── src/
│   ├── components/ui/     # shadcn-ui components
│   ├── lib/
│   │   ├── supabase.ts    # Supabase client
│   │   ├── stripe.ts      # Stripe configuration
│   │   └── accessibility-scanner.ts  # Scanner logic
│   ├── pages/
│   │   ├── Index.tsx      # Homepage
│   │   ├── Results.tsx    # Scan results page
│   │   └── Success.tsx    # Payment success page
│   └── App.tsx            # Root component
├── supabase/functions/    # Edge functions
│   ├── app_4051991953_scan_website/
│   ├── app_4051991953_stripe_checkout/
│   └── app_4051991953_stripe_webhook/
├── public/                # Static assets
├── vercel.json           # Vercel configuration
└── package.json          # Dependencies
```

## 🔧 Backend Architecture

### Supabase Edge Functions

1. **Scan Website** (`app_4051991953_scan_website`)
   - Fetches target website HTML
   - Analyzes accessibility issues
   - Returns detailed report

2. **Stripe Checkout** (`app_4051991953_stripe_checkout`)
   - Creates Stripe checkout session
   - Handles Pro plan subscription

3. **Stripe Webhook** (`app_4051991953_stripe_webhook`)
   - Processes payment events
   - Updates user subscription status

### Database Schema

**Table:** `app_4051991953_subscriptions`
- `id` (uuid, primary key)
- `user_id` (uuid, references auth.users)
- `tier` (text: 'free' or 'pro')
- `stripe_customer_id` (text)
- `stripe_subscription_id` (text)
- `created_at` (timestamp)
- `updated_at` (timestamp)

## 🔐 Security

- Row Level Security (RLS) enabled on all tables
- Environment variables for sensitive keys
- Stripe webhook signature verification
- Supabase Auth for user management

## 📝 License

MIT License - feel free to use this project for your own purposes!

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## 📧 Support

For issues or questions, please open an issue on GitHub.

---

**Built with ❤️ using MetaGPT**