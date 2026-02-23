# Tolerance Stack-Up Analysis Tool

Advanced tolerance analysis tool with Monte Carlo simulation, RSS analysis, and clearance fit evaluation.

## Quick Deploy to Vercel

### Method 1: Using Vercel CLI (Recommended)

1. **Install Vercel CLI**
   ```bash
   npm install -g vercel
   ```

2. **Navigate to your project folder**
   ```bash
   cd /path/to/your/folder
   ```

3. **Deploy**
   ```bash
   vercel
   ```
   
   Follow the prompts:
   - Set up and deploy? **Y**
   - Which scope? Select your account
   - Link to existing project? **N**
   - Project name? Press Enter (or choose a name)
   - In which directory is your code? **./`**
   - Auto-detected settings: **Y**

4. **Done!** You'll get a URL like `https://your-project.vercel.app`

### Method 2: Using Vercel Dashboard (Easiest)

1. **Go to [vercel.com](https://vercel.com)** and sign up/login

2. **Click "Add New Project"**

3. **Choose one of these options:**

   **Option A: Upload files directly**
   - Click "Upload" tab
   - Drag and drop your `tolerance-stackup.html` file
   - Click "Deploy"
   
   **Option B: From Git repository**
   - Push your files to GitHub/GitLab/Bitbucket
   - Import the repository in Vercel
   - Vercel will auto-deploy

4. **Configure (if needed)**
   - Root Directory: `./`
   - No build command needed (static HTML)
   - No install command needed

5. **Deploy!** Your site will be live at `https://your-project.vercel.app`

### Method 3: GitHub Integration (Best for Updates)

1. **Create a GitHub repository**
   ```bash
   git init
   git add tolerance-stackup.html README.md
   git commit -m "Initial commit"
   git branch -M main
   git remote add origin https://github.com/yourusername/tolerance-analysis.git
   git push -u origin main
   ```

2. **Connect to Vercel**
   - Go to [vercel.com/new](https://vercel.com/new)
   - Import your GitHub repository
   - Click "Deploy"

3. **Automatic deployments**
   - Every push to `main` branch auto-deploys
   - Pull requests get preview URLs

## Project Structure

```
tolerance-analysis/
├── tolerance-stackup.html    # Main application (standalone)
├── README.md                 # This file
└── vercel.json              # Optional configuration
```

## Optional: Vercel Configuration

Create a `vercel.json` file for custom settings:

```json
{
  "cleanUrls": true,
  "trailingSlash": false,
  "rewrites": [
    { "source": "/", "destination": "/tolerance-stackup.html" }
  ]
}
```

This makes your homepage load the tool directly at the root URL.

## Custom Domain (Optional)

1. Go to your project in Vercel Dashboard
2. Click "Settings" → "Domains"
3. Add your custom domain
4. Update DNS records as instructed

## Features

- ✅ Worst-Case tolerance analysis
- ✅ RSS (Root Sum Square) statistical method
- ✅ Monte Carlo simulation with multiple distributions
- ✅ Clearance fit analysis (hole/shaft)
- ✅ Process capability indicators (Cp, Cpk)
- ✅ Save/load configurations
- ✅ Interactive visualizations
- ✅ No build step required - pure HTML/CSS/JS

## Browser Support

- Chrome/Edge: ✅ Full support
- Firefox: ✅ Full support
- Safari: ✅ Full support
- Mobile browsers: ✅ Responsive design

## Local Development

No build process needed! Simply open `tolerance-stackup.html` in your browser.

## Troubleshooting

**Issue: Page shows 404**
- Make sure your HTML file is in the root directory
- Check that the file is named correctly
- Verify vercel.json rewrites if using custom configuration

**Issue: Fonts not loading**
- The page uses Google Fonts (CDN)
- Ensure you have internet connection
- Fonts will fallback to system fonts if CDN fails

**Issue: Need HTTPS**
- Vercel automatically provides HTTPS
- All deployments get SSL certificates

## Updates

To update your deployed site:

**If using CLI:**
```bash
vercel --prod
```

**If using Git:**
```bash
git add .
git commit -m "Update analysis tool"
git push
```
Vercel will auto-deploy!

## Support

For Vercel-specific issues, visit: https://vercel.com/docs

---

Built with vanilla HTML, CSS, and JavaScript - no framework required!
