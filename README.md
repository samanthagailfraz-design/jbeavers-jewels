# J Beaver’s Jewels

A complete, production-ready jewelry storefront with Stripe Checkout integration.

## What’s inside

**Frontend** (`public/index.html`)

- Editorial cream and burgundy aesthetic with gold accents
- Hero section with hand-drawn SVG illustrations
- 10 product catalog with category filters (rings, earrings, necklaces, bracelets)
- Featured trio section, scrolling marquee, atelier story
- Four-step craft process explainer
- Customer testimonials, FAQ accordion, newsletter signup
- Sliding cart drawer with quantity controls and remove buttons
- Toast notifications for add-to-bag feedback
- Mobile-responsive with hamburger menu
- Four policy modals (Terms, Privacy, Refunds, Shipping)
- Scroll-triggered fade-in animations

**Backend** (`server.js`)

- Node.js + Express
- Stripe Checkout integration (PCI compliant, hosted by Stripe, handles 3D Secure)
- Server-side price validation (prices defined on server, never trusted from client)
- Shipping address collection from 25+ countries
- Two shipping options: free ground or $25 express overnight
- Phone number collection
- Optional webhook endpoint for order fulfillment

**Other**

- Success page (`public/success.html`) shown after payment
- Cancel handling (toast notification on return)
- All Stripe-required policy pages built in

## Prerequisites

- Node.js 18 or higher
- A Stripe account (free at <https://stripe.com>)

## Setup (5 minutes)

### 1. Install

```bash
npm install
```

### 2. Get your Stripe keys

1. Go to <https://dashboard.stripe.com/register> and sign up
1. Verify your email
1. Go to **Developers > API keys**
1. Copy the **Secret key** (starts with `sk_test_`)

### 3. Configure

```bash
cp .env.example .env
```

Open `.env` and paste your secret key:

```
STRIPE_SECRET_KEY=sk_test_your_actual_key_here
DOMAIN=http://localhost:3000
```

### 4. Run

```bash
npm start
```

Open <http://localhost:3000>

### 5. Test a payment

Use Stripe’s test card at checkout:

- **Number**: 4242 4242 4242 4242
- **Expiry**: any future date (12/34)
- **CVC**: any 3 digits (123)
- **ZIP**: any (90210)

You can also test card declines, 3D Secure, etc. with cards from <https://docs.stripe.com/testing#cards>

## Going live (taking real payments)

### What Stripe needs from you as a USA sole proprietor

When you activate your account, Stripe will ask for:

- **Legal first and last name** (must match what the IRS has)
- **Date of birth**
- **Last 4 digits of SSN** (sometimes full SSN)
- **Home address** (where you live)
- **Business name (DBA)**: “J Beaver’s Jewels”
- **Business website URL** (your live domain)
- **Product description**: “Handcrafted fine jewelry sold direct to consumer”
- **Statement descriptor**: how charges appear on customer card statements (e.g. “JBEAVERSJEWELS”)
- **Bank account** (routing + account number) for payouts
- **A valid USA phone number** (you have +1 415 244 0583)

### Activation steps

1. Complete the business profile in your Stripe dashboard
1. Submit for review (usually approved within 1-2 business days)
1. Stripe issues live API keys
1. In your production `.env`, swap `sk_test_...` for `sk_live_...`
1. Set `DOMAIN` to your real https domain

### Stripe acceptable use for jewelry

Jewelry is fully supported by Stripe with no special restrictions. The four things Stripe checks for any e-commerce site are:

1. **Clear product information** ✓ (10 products with descriptions and prices)
1. **Contact details** ✓ (email, phone, hours visible)
1. **Refund / return policy** ✓ (modal in footer)
1. **Privacy policy and terms** ✓ (modals in footer)

All four are already built into the site.

## Deploying online

### Option 1: Render (easiest, free tier)

1. Push this folder to GitHub (or GitLab/Bitbucket)
1. Sign up at <https://render.com>
1. Click **New > Web Service** and connect your repo
1. Settings:
- Build command: `npm install`
- Start command: `node server.js`
- Environment: Node
1. Add environment variables: `STRIPE_SECRET_KEY` and `DOMAIN` (set DOMAIN to your Render URL like `https://jbeavers.onrender.com`)
1. Deploy

### Option 2: Railway

1. Push to GitHub
1. Sign up at <https://railway.app>
1. **New Project > Deploy from GitHub**
1. Add environment variables
1. Generate a domain in settings

### Option 3: Custom domain

After deploying to Render or Railway:

1. Buy a domain (Namecheap, Google Domains, etc.)
1. In your host’s settings, add the custom domain
1. Update DNS records as instructed
1. Update `DOMAIN` in your environment variables to the new https URL

## Customising

### Edit your contact details (the easy part)

All business details — email, phone, hours, founder name, year established, copyright year — live in **one file**:

```
public/config.js
```

Open it, change any value, save. That’s it. The same change updates the contact section, the success page, all 4 policy documents, and the footer copyright automatically.

The file is heavily commented so you know what each value does. The only rule is to keep the quotes `"..."` around each value and the commas at the end of each line.

After editing on GitHub, commit and push. Render/Railway will auto-redeploy in 1–2 minutes and your changes go live.

### Swap in real product photos

In `public/index.html`, find the `PRODUCTS` array. Replace each `svg` field with a real image:

```js
imageUrl: "https://yourcdn.com/photos/ember.jpg"
```

Then update `renderProducts()` to use `<img src="${p.imageUrl}">` instead of `${p.svg}`.

Also add `images: [imageUrl]` to `product_data` in `server.js` so the image shows on Stripe Checkout.

### Adjust prices

Prices live in **two places** and must match. Both use **cents**:

1. `public/index.html` (the `PRODUCTS` array, frontend display)
1. `server.js` (the `PRODUCTS` object, authoritative pricing used by Stripe)

### Adjust shipping

In `server.js`, the `shipping_options` array has two rates. Edit `amount` (in cents) and `display_name`.

### Add or remove products

Add an entry in both files using the same `id`.

## Webhooks (recommended for production)

Webhooks let you reliably fulfill orders, send confirmation emails, and update inventory.

1. Uncomment the `/webhook` route in `server.js`
1. In Stripe Dashboard: **Developers > Webhooks > Add endpoint**
1. Endpoint URL: `https://yourdomain.com/webhook`
1. Events to send: `checkout.session.completed`
1. Copy the signing secret into `.env` as `STRIPE_WEBHOOK_SECRET`

## Security checklist

- ✅ Secret keys are in `.env`, never in client-side code
- ✅ `.env` is in `.gitignore` (already done)
- ✅ Prices are validated server-side (clients can’t tamper)
- ✅ Stripe handles all card data (PCI compliant by default)
- ⚠️ Use HTTPS in production (Render and Railway provide this automatically)

## Business details on file

- **Name**: J Beaver’s Jewels
- **Email**: [samanthagailfraz@gmail.com](mailto:samanthagailfraz@gmail.com)
- **Phone**: +1 (415) 244 0583
- **Country**: USA
- **Entity**: Sole Proprietor
- **Hours**: 24 hours, 7 days a week

## Need help?

If you hit a snag with Stripe activation or deployment, the most common fix is making sure:

1. The business name on Stripe exactly matches what you file taxes under
1. The bank account holder name matches that name
1. Your website is publicly accessible with HTTPS
1. All four policy pages (Terms, Privacy, Refunds, Shipping) are reachable

All four pages are built into this site already.