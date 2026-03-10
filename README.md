# 🌾 RootAI — Deployment Guide

Deploy your platform in under 10 minutes. Your API key stays secret on the server — never exposed to users.

---

## How It Works (Architecture)

```
User's Browser  →  /api/chat (your Vercel/Netlify server)  →  Anthropic API
                        ↑
               API key lives here, hidden
```

The frontend (`public/index.html`) calls `/api/chat` — your own server.
Your server adds the secret API key and forwards to Anthropic.
Users never see the key.

---

## Option A: Deploy on Vercel (Recommended — fastest)

### Step 1 — Get your Anthropic API key
1. Go to https://console.anthropic.com
2. Click **API Keys** → **Create Key**
3. Copy the key (starts with `sk-ant-...`) — save it somewhere safe

### Step 2 — Put this project on GitHub
1. Go to https://github.com → click **New repository**
2. Name it `rootai` → click **Create repository**
3. Upload all files by dragging them onto the page, or use Git:
```bash
git init
git add .
git commit -m "RootAI platform"
git remote add origin https://github.com/YOUR_USERNAME/rootai.git
git push -u origin main
```

### Step 3 — Deploy on Vercel
1. Go to https://vercel.com → **Sign up free** (use GitHub login)
2. Click **Add New Project**
3. Select your `rootai` repository → click **Import**
4. Click **Deploy** (no changes needed)

### Step 4 — Add your API key
1. In Vercel dashboard → your project → **Settings** → **Environment Variables**
2. Click **Add New**:
   - **Name:** `ANTHROPIC_API_KEY`
   - **Value:** paste your `sk-ant-...` key
   - **Environment:** check all three (Production, Preview, Development)
3. Click **Save**
4. Go to **Deployments** → click the three dots on your latest deploy → **Redeploy**

### Step 5 — Your site is live! 🎉
Vercel gives you a URL like `https://rootai-xyz.vercel.app`
The chatbot will work for anyone, anywhere in the world.

---

## Option B: Deploy on Netlify

### Step 1 — Get your Anthropic API key
Same as above — https://console.anthropic.com

### Step 2 — Deploy on Netlify
1. Go to https://netlify.com → **Sign up free**
2. Click **Add new site** → **Deploy manually**
3. Drag and drop your entire project folder onto the page
4. Netlify deploys in ~30 seconds

### Step 3 — Add your API key
1. In Netlify dashboard → your site → **Site configuration** → **Environment variables**
2. Click **Add a variable**:
   - **Key:** `ANTHROPIC_API_KEY`
   - **Values:** paste your `sk-ant-...` key
3. Click **Save**
4. Go to **Deploys** → click **Trigger deploy** → **Deploy site**

### Step 4 — Live! 🎉
Netlify gives you a URL like `https://rootai-abc123.netlify.app`

---

## Project File Structure

```
rootai/
├── public/
│   └── index.html          ← The entire frontend (all CSS, JS, content)
├── api/
│   └── chat.js             ← Vercel serverless function (secure proxy)
├── netlify/
│   └── functions/
│       └── chat.js         ← Netlify function (same logic, different format)
├── vercel.json             ← Vercel routing config
├── netlify.toml            ← Netlify routing config
└── README.md               ← This file
```

---

## Custom Domain (Optional)

### Vercel:
Settings → Domains → Add your domain (e.g. `rootai.in`) → follow DNS instructions

### Netlify:
Site configuration → Domain management → Add custom domain

---

## Costs

| Service | Free Tier | Paid |
|---|---|---|
| Vercel | 100GB bandwidth/month, unlimited deploys | $20/mo for more |
| Netlify | 100GB bandwidth/month, 125K function calls | $19/mo for more |
| Anthropic API | Pay per use | ~$0.003 per conversation |

For a rural education platform with moderate traffic, the free tier of Vercel/Netlify is more than enough. Anthropic API costs are very low — roughly **₹0.25 per full conversation**.

---

## Troubleshooting

**Chat says "trouble connecting"**
→ Check your API key is saved correctly in environment variables
→ Make sure you redeployed after adding the key

**Page loads but chat doesn't work**
→ Open browser DevTools (F12) → Console tab → look for red errors
→ Check the Network tab for failed `/api/chat` requests

**"Method not allowed" error**
→ Make sure `vercel.json` is in the root folder (not inside `public/`)

**Want to test locally?**
```bash
npm install -g vercel
vercel dev
# Then visit http://localhost:3000
```
Set your key locally:
```bash
export ANTHROPIC_API_KEY=sk-ant-your-key-here
```

---

## Restricting to Your Domain (Security)

In `api/chat.js`, change this line:
```js
res.setHeader('Access-Control-Allow-Origin', '*');
```
To your actual domain:
```js
res.setHeader('Access-Control-Allow-Origin', 'https://rootai.in');
```
This prevents other websites from using your API key.

---

## Need Help?

Open an issue on your GitHub repo or contact the platform team.
