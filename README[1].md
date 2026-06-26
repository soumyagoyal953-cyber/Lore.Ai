# ✨ Lore.Ai

**Lore.Ai** is a student-safe AI chat assistant inspired by ChatGPT, created with love by a 13-year-old developer, **Soumya Goyal**.

It includes:
- 🤖 ChatGPT-style chat interface
- 📝 Chat history with SQLite persistence
- 🔐 Working login and signup (JWT + bcrypt)
- 🎨 Text-to-image generation (5 free per 24h)
- 🎬 Text-to-video and photo-to-video generation (2 free per 24h)
- 🎙️ Voice chat (Web Speech API)
- 📎 Photo attachments in chat
- ⭐ Subscription plans (Free, Pro, Ultra)
- 🛡️ Student-safety content filters

## 🚀 Quick Start

```bash
cd astra-ai
npm install
npm start
```

Then open [http://localhost:3000](http://localhost:3000) in your browser.

### Default Test Account
- **Email:** `soumya@lore.ai`
- **Password:** `soumya123`

You can also create a new account from the signup page.

## 🛠️ Development

```bash
npm run dev
```

This runs the server with `nodemon` so it restarts automatically when you edit files.

## 💳 Real Payments

Lore.Ai supports both **Stripe** and **Razorpay**:

- **Stripe Checkout** for card/subscription payments.
- **Razorpay** for UPI, cards, and netbanking (best for India).

See [`DEPLOYMENT.md`](DEPLOYMENT.md) for the full step-by-step setup.

## 🚀 Deploy Online

See [`DEPLOYMENT.md`](DEPLOYMENT.md) for deploying to **Render** (free) with a persistent SQLite database and real payments.

## 📁 Project Structure

```
astra-ai/
├── server/
│   ├── server.js          # Express entry point
│   ├── database.js        # SQLite schema & connection
│   ├── middleware/auth.js # JWT verification
│   └── routes/
│       ├── auth.js        # Login & signup
│       ├── chat.js        # Chat sessions & messages
│       ├── media.js       # Image/video generation & uploads
│       └── subscriptions.js # Plans & upgrades
├── public/                # Frontend SPA
│   ├── index.html
│   ├── css/styles.css
│   └── js/app.js
├── .env                   # Environment variables
├── package.json
└── README.md
```

## 🔑 Connecting Real AI APIs

The media generation currently uses **mock responses** so the app works immediately without API keys.

To connect real AI services, edit the media routes in `server/routes/media.js`:

### Images
Replace the mock SVG writer with a call to:
- **DALL-E 3** (OpenAI)
- **Stable Diffusion** (Stability AI, Replicate)
- **Midjourney** (via API wrapper)

### Videos
Replace the mock video URLs with a call to:
- **Runway Gen-3**
- **Pika Labs**
- **Kling AI**
- **Replicate** (models like `stabilityai/stable-video-diffusion`)

You can also add a real chat model by replacing the `generateMockResponse` function in `server/routes/chat.js` with an API call to **OpenAI GPT-4o**, **Google Gemini**, **Claude**, or any other LLM API.

## 💳 Real Payments

The upgrade endpoint in `server/routes/subscriptions.js` currently updates the plan without payment verification. For production, integrate:
- **Stripe** (international)
- **Razorpay** (India)
- **PayPal**

Verify the payment before calling `UPDATE users SET plan = ?`.

## 🔒 Security Notes for Production

- Change `JWT_SECRET` in `.env` to a long, random string.
- Use HTTPS in production.
- Add rate limiting (e.g., `express-rate-limit`).
- Add email verification for signup.
- Store uploaded files in cloud storage (S3, Cloudinary) instead of local disk.
- Validate and sanitize all user inputs.
- Add CSRF protection if using cookies instead of Bearer tokens.

## 🧑‍💻 Created By

**Soumya Goyal** — a 13-year-old developer who built this with love. ❤️
