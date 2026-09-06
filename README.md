# Viral Product Store

A fast, lightweight product store with Razorpay payment integration.

## Quick Start

### 1. Deploy Website
- Push this repo to GitHub
- Enable GitHub Pages (Settings → Pages → Deploy from main branch)
- Your site goes live at `https://yourusername.github.io/viral-product-store`

### 2. Get Razorpay Keys
- Sign up at [Razorpay](https://razorpay.com)
- Go to Dashboard → Settings → API Keys
- Copy your **Key ID** (Live mode)
- Replace `YOUR_RAZORPAY_KEY_ID` in `index.html`

### 3. Setup Webhook (Google Sheets Integration)

#### Step A: Create Zapier Zap
1. Go to [zapier.com](https://zapier.com) → Create Zap
2. Trigger: Webhooks by Zapier → Catch Hook
3. Copy the webhook URL provided

#### Step B: Add Webhook to Razorpay
1. Razorpay Dashboard → Account & Settings → Webhooks
2. Add New Webhook
3. Paste Zapier URL
4. Check: `payment.captured`
5. Save

#### Step C: Connect to Google Sheets
1. In Zapier, Action: Google Sheets → Create Spreadsheet Row
2. Create a sheet named "Store Orders" with columns:
   - Payment ID
   - Customer Name
   - Phone Number
   - Email
   - Amount Paid
   - Status
3. Map Razorpay fields to sheet columns
4. Turn Zap ON

## Customization

### Update Product Info
Edit in `index.html`:
- Line 22: Product image URL
- Line 24: Product title
- Line 25: Price in ₹
- Line 26: Description
- Line 31: Razorpay Key ID
- Line 34: Store name
- Line 35: Product description
- Line 36: Logo URL

## Performance
- Load time: <1s
- Mobile optimized
- No dependencies except Razorpay
- Works offline after first load

## Support
For issues, see Razorpay docs or contact support@razorpay.com