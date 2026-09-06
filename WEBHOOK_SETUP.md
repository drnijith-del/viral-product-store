# Webhook Setup Guide (Step 4)

## Complete Zapier + Razorpay + Google Sheets Integration

### Part A: Create Zapier Webhook

1. Go to [zapier.com](https://zapier.com)
2. Click **Create Zap**
3. **Trigger Setup:**
   - Search for and select **"Webhooks by Zapier"**
   - Choose Event: **"Catch Hook"**
   - Click **Continue**
4. Zapier will generate a unique URL like: `https://hooks.zapier.com/hooks/catch/xxxxx/yyyy/`
5. **Copy this URL** - you'll need it in the next step

### Part B: Add Webhook to Razorpay

1. Log into [Razorpay Dashboard](https://dashboard.razorpay.com)
2. Go to **Account & Settings** (top right)
3. Click **Webhooks** (left menu)
4. Click **Add New Webhook**
5. **Paste the Zapier URL** in the Webhook URL field
6. Under **Active Events**, check only: **`payment.captured`**
7. Click **Save**

### Part C: Test the Connection

1. Go back to Zapier and click **Test Trigger**
2. It will say "Waiting for data..."
3. Open your store: `https://drnijith-del.github.io/viral-product-store`
4. Click **Buy Now**
5. Use Razorpay's test card:
   - Card: `4111 1111 1111 1111`
   - Expiry: Any future date (e.g., `12/25`)
   - CVV: `123`
   - OTP: `123456`
6. Complete payment
7. Zapier should show: **"Request Found!"** with customer data

### Part D: Connect Google Sheets

1. **Create Google Sheet:**
   - Go to [sheets.google.com](https://sheets.google.com)
   - Click **+ Blank**
   - Rename to: **"Store Orders"**
   - In Row 1, create headers:
     - A1: `Payment ID`
     - B1: `Customer Name`
     - C1: `Phone Number`
     - D1: `Email`
     - E1: `Amount Paid`
     - F1: `Status`

2. **Connect in Zapier:**
   - In Zapier, click **Continue** after Test Trigger
   - For **Action**, search: **"Google Sheets"**
   - Choose: **"Create Spreadsheet Row"**
   - Click **Connect** and authorize your Google account
   - Select Spreadsheet: **"Store Orders"**
   - Select Worksheet: **"Sheet1"**

3. **Map the Fields:**
   - **Payment ID** → `id` (from Razorpay)
   - **Customer Name** → `customer_name` or `billing_name`
   - **Phone Number** → `contact`
   - **Email** → `email`
   - **Amount Paid** → `amount` (divide by 100 to get rupees)
   - **Status** → Set to: `"Pending"`

4. **Finish & Activate:**
   - Review the setup
   - Click **Publish Zap**
   - Toggle to **ON**

### Part E: Test End-to-End

1. Visit your store
2. Make another test payment
3. Check your Google Sheet
4. New row should appear automatically within 10 seconds

---

## Troubleshooting

**Webhook not firing?**
- Check Razorpay dashboard → Webhooks → Recent Attempts
- Verify payment status is "Captured" (not "Authorized")

**Data not appearing in Sheet?**
- Check Zapier task history for errors
- Verify column mapping is correct
- Re-authorize Google Sheets in Zapier

**Wrong data format?**
- Amount will be in paise (multiply by 100), divide by 100 in Sheet formula
- Use this formula: `=D2/100` if amount is in column D

Done! ✅ Your store is now fully automated.
