# Zapier + Google Sheets Integration Guide

## Complete Step-by-Step Setup

This guide will connect your instahype store to Google Sheets via Zapier for automatic order tracking.

---

## Step 1: Create Zapier Zap

### 1.1 Go to Zapier
- Visit [zapier.com](https://zapier.com)
- Sign up or log in
- Click **Create Zap**

### 1.2 Set Up Trigger
1. Search for **"Webhooks by Zapier"**
2. Select **"Webhooks by Zapier"**
3. Choose Event: **"Catch Hook"**
4. Click **Continue**
5. You'll see a screen with a URL like:
   ```
   https://hooks.zapier.com/hooks/catch/xxxxx/yyyy/
   ```
6. **COPY THIS URL** - You need it for Razorpay

---

## Step 2: Add Webhook to Razorpay

### 2.1 Log into Razorpay
- Go to [dashboard.razorpay.com](https://dashboard.razorpay.com)
- Sign in with your account

### 2.2 Navigate to Webhooks
1. Click your **Account & Settings** (top right)
2. In left menu, click **Webhooks**
3. Click **Add New Webhook**

### 2.3 Configure Webhook
1. **Webhook URL:** Paste the Zapier URL you copied
2. **Events:** Check ONLY these:
   - ✅ `payment.captured` (when payment succeeds)
   - ✅ `payment.failed` (when payment fails)
3. Click **Save Webhook**

### 2.4 Test Webhook (Optional)
- Razorpay will show "Recent Attempts"
- You should see a successful test

---

## Step 3: Create Google Sheet

### 3.1 Create New Sheet
1. Go to [sheets.google.com](https://sheets.google.com)
2. Click **+ Blank** to create new sheet
3. Name it: **"instashop Orders"**

### 3.2 Add Headers (Row 1)
In the first row, create these column headers:

| A | B | C | D | E | F | G | H | I |
|---|---|---|---|---|---|---|---|---|
| Order ID | Customer Name | Phone | Email | Product | Price (₹) | Address | Status | Payment ID |

**Example:**
- A1: `Order ID`
- B1: `Customer Name`
- C1: `Phone`
- D1: `Email`
- E1: `Product`
- F1: `Price (₹)`
- G1: `Address`
- H1: `Status`
- I1: `Payment ID`

### 3.3 Share with Zapier
1. Click **Share** (top right)
2. Copy the shareable link
3. Make sure "Anyone with the link" can **Edit**

---

## Step 4: Connect Zapier to Google Sheets

### 4.1 Back in Zapier
1. Return to your Zapier zap
2. You should still be on the "Catch Hook" trigger screen
3. Click **Test Trigger** or **Continue**
4. Zapier will wait for a webhook

### 4.2 Set Up Action
1. Click **+ Add Action**
2. Search for **"Google Sheets"**
3. Select **"Google Sheets"**
4. Choose Action: **"Create Spreadsheet Row"**
5. Click **Continue**

### 4.3 Authorize Google Account
1. Click **Connect** 
2. Select your Google account
3. Allow Zapier access to your sheets
4. Click **Allow**

### 4.4 Map the Data
1. **Spreadsheet:** Select **"instashop Orders"**
2. **Worksheet:** Select **"Sheet1"**
3. **Create Rows:** Toggle ON

Now map each column:

| Sheet Column | Razorpay Data |
|---|---|
| Order ID | `payload.id` (Payment ID) |
| Customer Name | `payload.customer_details.name` |
| Phone | `payload.customer_details.contact` |
| Email | `payload.customer_details.email` |
| Product | (You'll need to track this separately) |
| Price | `payload.amount` ÷ 100 (converts paise to rupees) |
| Address | (You'll need to track this separately) |
| Status | `"Completed"` (text) |
| Payment ID | `payload.id` |

**Note:** Since Razorpay webhooks don't include product/address, you'll need to:
- Keep admin.html for complete order details
- Use Google Sheets for payment confirmations only
- Or use the alternative method below

---

## Alternative: Enhanced Integration (Recommended)

To capture ALL data (product, address, etc.), modify `index.html` to send data to Zapier directly:

### In the `validateAndPay()` function, add before Razorpay:

```javascript
// Send order data to Zapier webhook
const zapierData = {
    orderId: new Date().getTime(),
    customerName: name,
    phone: phone,
    email: email,
    product: currentProduct.name,
    price: currentProduct.price,
    address: address,
    status: 'pending',
    timestamp: new Date().toLocaleString('en-IN')
};

fetch('YOUR_ZAPIER_WEBHOOK_URL', {
    method: 'POST',
    body: JSON.stringify(zapierData),
    headers: {'Content-Type': 'application/json'}
}).catch(err => console.log('Webhook sent'));
```

Replace `YOUR_ZAPIER_WEBHOOK_URL` with the Zapier URL from Step 1.

---

## Step 5: Test the Integration

### 5.1 Make a Test Purchase
1. Go to store: https://drnijith-del.github.io/viral-product-store/
2. Click **Buy Now** on any product
3. Fill in checkout form:
   - Name: Test User
   - Phone: 9999999999
   - Address: Test Address
4. Click **Proceed to Payment**
5. Use test card: **4111 1111 1111 1111**
   - Expiry: Any future date
   - CVV: 123
   - OTP: 123456

### 5.2 Check Google Sheet
1. Go to your Google Sheet
2. Refresh the page
3. You should see a new row with order data!

### 5.3 Check Zapier
1. In Zapier, you'll see "Request Found" in your trigger
2. The action should show "Success"

---

## Step 6: Publish Your Zap

### 6.1 Activate
1. Click **Publish Zap**
2. Toggle to **ON**
3. Confirm

### 6.2 Monitor
- In Zapier dashboard, you'll see all successful runs
- Check Google Sheet for all incoming orders

---

## Troubleshooting

### No data in Google Sheet?

**Check 1:** Verify webhook is firing
- Razorpay → Webhooks → Recent Attempts
- Look for successful requests

**Check 2:** Make a real payment
- Test mode payment might not trigger webhooks
- Use actual Razorpay test environment

**Check 3:** Check Zapier logs
- Go to Zapier task history
- See if action ran successfully

### Missing customer details?

Use the "Enhanced Integration" method above to send full order data to Zapier before payment.

### Payment not showing as "completed"?

Add this to admin.html to mark orders:
```javascript
orders[orders.length - 1].status = 'completed';
localStorage.setItem('instashop_orders', JSON.stringify(orders));
```

---

## What You Now Have

✅ **Automatic order capture** in Google Sheets  
✅ **Real-time notifications** for each payment  
✅ **Shareable spreadsheet** for team access  
✅ **CSV export** from Zapier  
✅ **Complete audit trail** of all sales  

---

## Next Steps

1. **Set up email notifications** in Zapier (tell yourself when order arrives)
2. **Create fulfillment workflow** (order → SMS → shipment)
3. **Automate shipping labels** (Zapier + EasyPost)
4. **Track inventory** (stock updates in Sheet)

---

**Your instahype + Zapier + Google Sheets integration is ready!** 🎉

For detailed Zapier help: [zapier.com/help](https://zapier.com/help)
