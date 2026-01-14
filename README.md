# Au Pair Scraper Config Generator

A web-based configuration generator for the Au Pair in America candidate scraper with automatic GitHub saving.

## Features

- 🔒 **Secure Storage**: Automatically saves configs to private GitHub repository
- 📝 **Easy Config Generation**: User-friendly form to create scraper configuration
- ⚡ **Auto-Save to GitHub**: Configs automatically committed to private repo
- 💾 **Backup Download**: Fallback to local download if GitHub save fails
- 🎨 **Clean Interface**: Beautiful, responsive design that works on all devices
- 🔐 **Admin Panel**: Token configuration stored in browser localStorage

## Live Demo

Once deployed to Netlify, share the URL with users who need to generate their config files.

## How It Works

### For Admin (One-Time Setup):
1. Create GitHub Personal Access Token with `repo` scope
2. Deploy website to Netlify
3. Configure admin settings with GitHub token
4. Share Netlify URL with users

### For Users:
1. Visit the hosted website
2. Fill in their Au Pair credentials and search preferences
3. Click "Download Config File"
4. Config automatically saves to your private GitHub repo

### For You (Running Scraper):
1. Check the private GitHub repo for new configs
2. Download the config file
3. Run the scraper locally with that config
4. Get CSV results

**See [SETUP.md](SETUP.md) for complete step-by-step setup instructions.**

## Deployment to Netlify

### Option 1: Deploy via Netlify UI (Easiest)

1. **Connect to GitHub**:
   - Go to [netlify.com](https://www.netlify.com/)
   - Sign in or create an account
   - Click "Add new site" → "Import an existing project"
   - Choose "Deploy with GitHub"
   - Authorize Netlify to access your GitHub account

2. **Select Repository**:
   - Find and select `aupair-config-generator` repository
   - Click on it

3. **Configure Build Settings**:
   - **Build command**: Leave empty (static site)
   - **Publish directory**: `public`
   - Click "Deploy site"

4. **Done!**:
   - Your site will be live at `https://random-name-12345.netlify.app`
   - You can customize the domain in Site settings → Domain management

### Option 2: Deploy via Netlify CLI

```bash
# Install Netlify CLI globally
npm install -g netlify-cli

# Navigate to project directory
cd /Users/mishelleweinerman/aupair-config-generator

# Login to Netlify
netlify login

# Deploy
netlify deploy --prod
```

### Option 3: Deploy via Git Push

Netlify automatically deploys when you push to your GitHub repository if you've connected it in the Netlify UI.

## Custom Domain (Optional)

After deployment, you can set up a custom domain:

1. Go to Site settings → Domain management
2. Click "Add custom domain"
3. Follow the DNS configuration instructions

## Local Development

To test locally before deploying:

```bash
# Serve the public folder with any static server
cd aupair-config-generator/public

# Option 1: Using Python
python3 -m http.server 8000

# Option 2: Using Node.js
npx serve

# Then open: http://localhost:8000
```

## Security Notes

- **Client-Side Only**: All processing happens in the browser using JavaScript
- **No Backend**: No server-side code that could intercept credentials
- **HTTPS**: Netlify provides free SSL certificates automatically
- **No Analytics**: No tracking or data collection (unless you add it)

## File Structure

```
aupair-config-generator/
├── public/
│   └── index.html          # Complete web app (HTML, CSS, JS)
├── netlify.toml            # Netlify configuration
└── README.md               # This file
```

## What Users Need to Do

After downloading their config file:

1. **Place the config file**:
   ```bash
   # Move to scraper directory
   mv ~/Downloads/aupair-config.json /Users/mishelleweinerman/my-crawler/src/
   ```

2. **Run the scraper**:
   ```bash
   cd /Users/mishelleweinerman/my-crawler
   node src/aupair-scraper.js
   ```

3. **Find results**:
   - CSV files saved in home directory
   - Format: `aupair_candidates_YYYY-MM-DDTHH-MM-SS.csv`

## Updating the Site

To make changes:

1. Edit `public/index.html`
2. Commit and push to GitHub
3. Netlify automatically redeploys (if auto-deploy is enabled)

Or deploy manually:
```bash
netlify deploy --prod
```

## Troubleshooting

**Site not deploying?**
- Check that `public` folder exists and contains `index.html`
- Verify `netlify.toml` is in the root directory
- Check Netlify deploy logs for errors

**Form not working?**
- Check browser console for JavaScript errors
- Ensure you're using a modern browser (Chrome, Firefox, Safari, Edge)

**Download not working?**
- Some browsers block automatic downloads - check your download settings
- Try a different browser if the issue persists

## License

MIT

## Support

For issues with:
- **The config generator website**: Open an issue in this repository
- **The scraper itself**: Contact the scraper maintainer
