# Construction Dispute Interview Tool — Netlify Deployment Guide

This is an AI-powered construction dispute interview tool that runs on Netlify with a serverless backend.

## Project Structure

```
construction-dispute-netlify/
├── index.html                    # Frontend application
├── netlify.toml                  # Netlify configuration
├── netlify/
│   └── functions/
│       └── chat.js               # Serverless function that proxies to Anthropic API
├── package.json                  # Project metadata
└── README.md                      # This file
```

## Key Changes from Original

1. **API Endpoint**: The frontend now calls `/.netlify/functions/chat` instead of directly calling Anthropic
2. **No API Key Storage**: The Anthropic API key is stored securely as a Netlify environment variable (`ANTHROPIC_API_KEY`)
3. **Server-Side Proxy**: `netlify/functions/chat.js` handles all communication with Anthropic's API

## Prerequisites

- GitHub account
- Netlify account (free tier works)
- Node.js and npm (for local development)

## Local Development

1. **Install dependencies:**
   ```bash
   npm install -g netlify-cli
   npm install
   ```

2. **Set up environment variables:**
   Create a `.env` file in the project root:
   ```
   ANTHROPIC_API_KEY=sk-ant-api03-YOUR_KEY_HERE
   ```

3. **Run locally:**
   ```bash
   netlify dev
   ```
   Open `http://localhost:8888` in your browser

## Deployment to Netlify via GitHub

### Step 1: Create GitHub Repository

1. Go to [github.com/new](https://github.com/new)
2. Create a new repository named `construction-dispute-tool`
3. Clone it locally:
   ```bash
   git clone https://github.com/YOUR_USERNAME/construction-dispute-tool.git
   cd construction-dispute-tool
   ```
4. Copy all files from `construction-dispute-netlify` into this folder
5. Push to GitHub:
   ```bash
   git add .
   git commit -m "Initial commit: construction dispute interview tool"
   git push origin main
   ```

### Step 2: Connect GitHub to Netlify

1. Go to [netlify.com](https://netlify.com) and sign in
2. Click **"New site from Git"**
3. Select **GitHub** as your Git provider
4. Authenticate and authorize Netlify
5. Select your `construction-dispute-tool` repository
6. Click **Next** (build settings are pre-configured)

### Step 3: Add Environment Variables

1. In Netlify, go to your site → **Site Settings** → **Build & Deploy** → **Environment**
2. Click **Edit variables**
3. Add:
   - **Key**: `ANTHROPIC_API_KEY`
   - **Value**: `sk-ant-api03-YOUR_ACTUAL_API_KEY`
4. Click **Save**

### Step 4: Deploy

1. Click **Deploy Site**
2. Wait for the build to complete (should take ~1 minute)
3. Visit your deployed site URL (e.g., `https://construction-dispute-tool.netlify.app`)

## How It Works

### Frontend (index.html)
- User interviews are conducted through a chat interface
- All conversation data is stored in browser localStorage
- When a message is sent, the frontend POSTs to `/.netlify/functions/chat`

### Netlify Function (netlify/functions/chat.js)
- Receives requests from the frontend
- Extracts the `ANTHROPIC_API_KEY` from environment variables
- Forwards the request to Anthropic's API
- Returns the response to the frontend
- The Anthropic API key never leaves the server

## Environment Variables Reference

| Variable | Required | Example |
|----------|----------|---------|
| `ANTHROPIC_API_KEY` | Yes | `sk-ant-api03-...` |

## Features

- 💬 **Multi-turn Interview**: Guided conversation through construction dispute phases
- 🚩 **Red Flag Detection**: AI identifies potential legal violations
- ⏰ **Timeline Generation**: Automatically builds a chronological timeline
- 📊 **Summary Statistics**: Tracks interview progress and financial information
- 🎤 **Voice Input**: Browser-based speech-to-text (Chrome, Edge)
- 💾 **Session Export**: Download interview data as JSON for attorney review

## Development Workflow

When you make changes:

1. Edit files locally
2. Test with `netlify dev`
3. Commit and push to GitHub:
   ```bash
   git add .
   git commit -m "Your message"
   git push origin main
   ```
4. Netlify auto-deploys on every push to `main`

## Troubleshooting

### "ANTHROPIC_API_KEY not configured"
- Verify the environment variable is set in Netlify Site Settings
- Redeploy the site after adding the variable

### "API error: 401"
- Check that your API key is correct and hasn't expired
- Verify the key starts with `sk-ant-`

### Function not working locally
- Ensure you have `netlify-cli` installed: `npm install -g netlify-cli`
- Check `.env` file exists with correct API key
- Run `netlify dev` not `npm start`

## Security Notes

✅ **API key is secure** — Stored only as a Netlify environment variable  
✅ **No credentials in code** — The key never appears in your repository  
✅ **HTTPS only** — All traffic to Netlify is encrypted  
✅ **Session data local** — Interview data stays in the browser until exported  

## Support

For issues:
1. Check the browser console for JavaScript errors
2. Check Netlify function logs: Site → Functions → View logs
3. Verify `ANTHROPIC_API_KEY` in Netlify environment variables

## API Limits

- Anthropic rate limits apply (see [anthropic.com/pricing](https://www.anthropic.com/pricing))
- Each interview call uses ~400-600 tokens
- Each timeline generation uses ~500-1000 tokens

---

**Ready to deploy?** Follow the [Deployment to Netlify via GitHub](#deployment-to-netlify-via-github) section above.
