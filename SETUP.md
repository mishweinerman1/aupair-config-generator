# Complete Setup Guide

This guide walks you through setting up the Au Pair Config Generator with automatic GitHub saving.

## Overview

The system has 3 parts:
1. **Config Generator Website** (Netlify) - Web form for users to create configs
2. **Private Config Storage** (GitHub) - Private repo where configs are saved
3. **Scraper** (Local) - Runs on your computer to scrape Au Pair data

## Part 1: Create GitHub Personal Access Token

You need this token for the website to save configs to GitHub.

1. **Go to GitHub Token Settings**:
   - Visit: https://github.com/settings/tokens/new
   - Or navigate: GitHub → Settings → Developer settings → Personal access tokens → Tokens (classic)

2. **Configure Token**:
   - **Note**: `AuPair Config Generator`
   - **Expiration**: Choose your preference (recommend 90 days or no expiration)
   - **Select scopes**: Check **`repo`** (Full control of private repositories)
     - This gives access to read/write to your private repos
   - Click **"Generate token"** at bottom

3. **Save Token**:
   - Copy the token (starts with `ghp_...`)
   - ⚠️ **Save it somewhere safe** - you won't see it again!
   - Example: `ghp_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`

## Part 2: Deploy Config Generator to Netlify

### Option A: Deploy via Netlify UI (Easiest)

1. **Sign in to Netlify**:
   - Go to https://netlify.com
   - Sign in with GitHub (or create account)

2. **Import Repository**:
   - Click **"Add new site"** → **"Import an existing project"**
   - Choose **"Deploy with GitHub"**
   - Authorize Netlify to access your repos
   - Select: `mishweinerman1/aupair-config-generator`

3. **Configure & Deploy**:
   - Build command: (leave empty)
   - Publish directory: `public`
   - Click **"Deploy site"**

4. **Your Site is Live!**:
   - Netlify assigns a URL like: `https://random-name-12345.netlify.app`
   - Note this URL - you'll share it with users

### Option B: Deploy via Netlify CLI

```bash
# Install Netlify CLI
npm install -g netlify-cli

# Navigate to project
cd /Users/mishelleweinerman/aupair-config-generator

# Login to Netlify
netlify login

# Deploy to production
netlify deploy --prod
```

## Part 3: Configure the Website

1. **Visit Your Netlify URL**:
   - Open the URL from Part 2 (e.g., `https://random-name-12345.netlify.app`)

2. **Click "Admin Settings"**:
   - This reveals the admin configuration panel

3. **Enter GitHub Token**:
   - Paste the token from Part 1
   - Repository should already show: `mishweinerman1/aupair-configs`
   - Click **"Save Settings"**

4. **Settings Saved**:
   - The token is saved in your browser's localStorage
   - It will persist even if you close the browser
   - ⚠️ **Important**: This is YOUR browser only - users won't see the token

## Part 4: Share with Users

Now you can share the Netlify URL with the person who needs to submit configs.

**What they do**:
1. Visit the URL you share
2. Fill out the form with their credentials
3. Click "Download Config File"
4. Config automatically saves to your private GitHub repo

**What you do**:
1. Go to https://github.com/mishweinerman1/aupair-configs
2. See the newly uploaded config file
3. Download it
4. Run the scraper with it (see Part 5)

## Part 5: Running the Scraper

Once you have a config file from GitHub:

```bash
# 1. Navigate to configs repo and pull latest
cd /Users/mishelleweinerman/aupair-configs
git pull

# 2. Copy the config to scraper directory
# Replace YYYY-MM-DD with actual filename
cp config-2026-01-14T15-30-00-username.json \
   /Users/mishelleweinerman/my-crawler/src/aupair-config.json

# 3. Run the scraper
cd /Users/mishelleweinerman/my-crawler
node src/aupair-scraper.js

# 4. Find results
# Look for CSV files in your home directory:
ls ~/*.csv
```

## Troubleshooting

### "Failed to save to GitHub"

**Possible causes**:
- Token not configured (click Admin Settings)
- Token expired (create new one)
- Token doesn't have `repo` scope (recreate with correct scope)
- Private repo doesn't exist (check https://github.com/mishweinerman1/aupair-configs)

**Solution**:
1. Create a new token with `repo` scope
2. Click "Admin Settings" and update the token
3. Try submitting again

### "GitHub token not configured"

You forgot to set up the admin settings.
1. Click "Admin Settings"
2. Enter your GitHub token
3. Click "Save Settings"

### Token visible in browser console?

Yes, this is expected. Since it's client-side, the token is visible in the browser console. This is why:
- You should only share the URL with trusted people
- The configs repo is private
- The token should have minimal permissions (only `repo` scope)

If you're worried, you can:
- Use fine-grained tokens scoped only to the `aupair-configs` repo
- Rotate the token regularly
- Delete the token after use

### Want to change the repository?

1. Create a new private repo
2. Update the repo name in Admin Settings
3. The token needs access to that repo

## Security Best Practices

1. ✅ **Keep configs repo PRIVATE** - Never make it public
2. ✅ **Only share Netlify URL with trusted users** - Anyone with URL can submit
3. ✅ **Use minimal token scope** - Only `repo` access needed
4. ✅ **Rotate tokens periodically** - Create new token every 3-6 months
5. ✅ **Delete old configs** - Clean up configs from GitHub after scraping
6. ✅ **Never commit tokens to code** - Token stored in localStorage only

## Advanced: Fine-Grained Token (More Secure)

Instead of classic token, use fine-grained token with access only to `aupair-configs` repo:

1. Go to: https://github.com/settings/personal-access-tokens/new
2. Token name: `AuPair Config Generator`
3. Repository access: **Only select repositories** → Choose `aupair-configs`
4. Permissions:
   - Contents: **Read and write**
5. Generate token
6. Use this token in Admin Settings

This is more secure because the token can only access that one repo.

## Questions?

If anything isn't working:
1. Check browser console (F12) for error messages
2. Verify token has correct permissions
3. Verify private repo exists and you have access
4. Try creating a new token

## Summary

Once set up:
- **Users** → Visit Netlify URL → Fill form → Submit
- **You** → Check GitHub → Download config → Run scraper
- **Results** → CSV files with candidate data

That's it! 🎉
