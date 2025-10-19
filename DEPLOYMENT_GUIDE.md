# My Hexo Blog Quick Reference

## 1. Initial Setup (First Time Only)
```bash
# Install Hexo CLI globally
npm install -g hexo-cli

# Install project dependencies
npm install
```

## 2. Local Development
```bash
# Start local dev server
hexo server
# or
npm run server

# Build site (test before deploy)
hexo generate
# or
npm run build

# Clean build files
hexo clean
# or
npm run clean

# Create new post
hexo new "Post Title"
```

## 3. Deploy Workflow

### Step 1: Fix Security & Update
```bash
npm audit fix
npm update
npm run build  # Test it works
```

### Step 2: Disable Branch Protection
- Go to repo → Settings → Branches → Branch protection rules
- Click "Delete" to temporarily disable
- **Remember to re-enable after push!**

### Step 3: Deploy as always
```bash
git add .
git commit -m "Your message"
git push origin main
```

### Step 4: Re-enable Branch Protection
- Go back to Settings → Branches
- Click "Add rule" → "main" → Enable "Restrict pushes that create files"

## 4. GitHub Actions (Automatic)
- **Triggers**: Push to main branch
- **What it does**: Installs deps → Builds site → Deploys to GitHub Pages
- **You don't need to do anything** - it's automatic after push

## 5. Content Management
- **New post**: Add `.md` file to `source/_posts/`
- **Delete post**: Just delete the `.md` file
- **Edit post**: Modify existing `.md` file

## Quick Commands
```bash
hexo server       # Local dev
hexo generate     # Build site
hexo clean        # Clean build
hexo new "title"  # New post
npm audit fix     # Fix security
npm update        # Update deps
```