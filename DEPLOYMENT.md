# Quick Deployment Guide

## Step 1: Create GitHub Repository

```bash
cd /Users/sem.spanhaak/Developer/native-ha/oauth-callback
git init
git add .
git commit -m "Initial commit: OAuth callback page"
```

Then create a new repository on GitHub (e.g., `native-ha-oauth`) and push:

```bash
git remote add origin https://github.com/YOUR_USERNAME/native-ha-oauth.git
git branch -M main
git push -u origin main
```

## Step 2: Enable GitHub Pages

1. Go to your repository on GitHub
2. Click **Settings** → **Pages**
3. Under "Build and deployment":
   - Source: **Deploy from a branch**
   - Branch: **main**
   - Folder: **/ (root)**
4. Click **Save**
5. Wait a few minutes for deployment

## Step 3: Configure DNS (sems.dev)

Add a CNAME record in your DNS provider:

```
Type: CNAME
Name: auth
Value: YOUR_USERNAME.github.io
TTL: 3600
```

## Step 4: Wait for DNS & Enable HTTPS

1. Wait 5-30 minutes for DNS propagation
2. Go back to GitHub Pages settings
3. Check "Enforce HTTPS" once the DNS check passes
4. Your endpoint will be live at: `https://auth.sems.dev`

## Step 5: Configure Home Assistant

In Home Assistant, create an OAuth application:

**Settings** → **People** → **Your Profile** → Scroll to bottom

Or via configuration.yaml (advanced):

```yaml
http:
  use_x_forwarded_for: true
  trusted_proxies:
    - 127.0.0.1

# This allows external OAuth redirects
```

Then create an application credential with:
- **Client ID**: `https://native-ha`
- **Redirect URI**: `https://auth.sems.dev`

## Testing

1. Open the Native HA app
2. Enter your Home Assistant URL
3. Tap "Authenticate with OAuth"
4. Log in to Home Assistant
5. You should be redirected to `auth.sems.dev`
6. The page will automatically open your app with the auth code

## Troubleshooting

**DNS not resolving?**
- Wait longer (up to 24 hours in rare cases)
- Check with: `nslookup auth.sems.dev`

**Page not loading?**
- Make sure GitHub Pages is enabled
- Check the deployment status in the **Actions** tab

**App not opening?**
- Make sure the app is installed
- Check that Info.plist has the correct URL scheme

**"Invalid redirect URI" error?**
- Make sure the redirect URI in Home Assistant matches exactly: `https://auth.sems.dev`
- No trailing slashes
- Must use HTTPS
