# 🚀 Deploy Lore.Ai Online + Accept Real Payments

This guide will walk you through deploying **Lore.Ai** to **Render** (free hosting) and connecting real payments through **Stripe** and **Razorpay**.

---

## 1. Deploy on Render (Free)

### Option A: One-click with Render Blueprint (Recommended)

1. Push your `astra-ai` folder to **GitHub**.
2. Go to [Render Dashboard](https://dashboard.render.com/) → **Blueprints** → **New Blueprint Instance**.
3. Connect your GitHub repo.
4. Render will read `render.yaml` and create a free web service with a persistent disk.
5. After deploy, your app will be live at `https://lore-ai.onrender.com`.

### Option B: Manual New Web Service

1. In Render Dashboard, click **New → Web Service**.
2. Connect your GitHub repo and select the `astra-ai` folder.
3. Settings:
   - **Runtime:** Node
   - **Build Command:** `npm install`
   - **Start Command:** `npm start`
   - **Plan:** Free
4. Add environment variables:
   - `NODE_ENV=production`
   - `PORT=10000`
   - `JWT_SECRET` → generate a long random string
   - `FRONTEND_URL=https://your-app-name.onrender.com`
   - `ALLOW_DIRECT_UPGRADE=false`
5. Add a **Disk**:
   - Name: `astra-data`
   - Mount Path: `/opt/render/project/src/data`
   - Size: 1 GB
6. Click **Create Web Service**.

---

## 2. Connect Stripe Payments

### 2.1 Create a Stripe account

1. Go to [stripe.com](https://stripe.com) and sign up.
2. Switch to **Test mode** while developing.

### 2.2 Get your API keys

- Go to **Developers → API keys**.
- Copy **Publishable key** and **Secret key**.

### 2.3 Create Stripe prices for subscriptions

1. Go to **Products → Add product**.
2. Create **Astra Pro** and **Astra Ultra** as recurring products (monthly).
3. Copy the **Price IDs** (they look like `price_xxxxxxxxxx`).

### 2.4 Set Stripe environment variables in Render

In your Render service settings, add:

```
STRIPE_SECRET_KEY=sk_test_xxxxxxxxxx
STRIPE_PUBLISHABLE_KEY=pk_test_xxxxxxxxxx
STRIPE_PRICE_PRO=price_xxxxxxxxxx
STRIPE_PRICE_ULTRA=price_xxxxxxxxxx
STRIPE_WEBHOOK_SECRET=whsec_xxxxxxxxxx
```

### 2.5 Set up Stripe webhook

1. In Stripe Dashboard, go to **Developers → Webhooks → Add endpoint**.
2. Endpoint URL: `https://your-app-name.onrender.com/api/payments/stripe/webhook`
3. Select event: `checkout.session.completed`
4. Copy the **Webhook signing secret** and add it as `STRIPE_WEBHOOK_SECRET` in Render.
5. Redeploy the service after adding all variables.

### 2.6 Test Stripe payment

- Use Stripe test card: `4242 4242 4242 4242`, any future date, any CVC, any ZIP.
- After payment, your plan will be upgraded automatically.

---

## 3. Connect Razorpay Payments (Best for India)

### 3.1 Create a Razorpay account

1. Go to [razorpay.com](https://razorpay.com) and sign up.
2. Use **Test mode** while developing.

### 3.2 Get your API keys

- Go to **Settings → API Keys**.
- Generate and copy **Key ID** and **Key Secret**.

### 3.3 Set Razorpay environment variables in Render

```
RAZORPAY_KEY_ID=rzp_test_xxxxxxxxxx
RAZORPAY_KEY_SECRET=xxxxxxxxxx
```

### 3.4 Test Razorpay payment

- Use Razorpay test UPI: `success@razorpay`.
- Or test card: `5267 3181 8797 5449`, any future expiry, any CVV.
- After payment, your plan is upgraded instantly.

---

## 4. Important Settings

### Change `FRONTEND_URL`

Set it to your actual deployed URL:

```
FRONTEND_URL=https://your-app-name.onrender.com
```

### Disable direct upgrades in production

```
ALLOW_DIRECT_UPGRADE=false
```

This forces users to pay before upgrading.

### Secure JWT secret

Generate a long random string. Do not use the default one from `.env`.

---

## 5. Switch from Mock AI to Real AI

After deployment, replace the mock responses in:

- `server/routes/chat.js` → connect OpenAI GPT-4o, Google Gemini, or Claude.
- `server/routes/media.js` → connect DALL-E, Stable Diffusion, Runway, Pika, etc.

These require their own API keys added as Render environment variables.

---

## 6. Custom Domain (Optional)

1. In Render, go to your web service → **Settings → Custom Domains**.
2. Add your domain and follow DNS instructions.
3. Update `FRONTEND_URL` to your custom domain.
4. Update Stripe webhook URL and Razorpay callback URLs.

---

## 7. Monitoring & Logs

- Render Dashboard shows live logs.
- Stripe Dashboard shows all payments and webhook events.
- Razorpay Dashboard shows transactions and settlements.

---

Need help? Open the README or check the Render docs at [render.com/docs](https://render.com/docs).
