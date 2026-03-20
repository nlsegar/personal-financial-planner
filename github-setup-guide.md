# GitHub Setup Guide

How to push the financial dashboard to a private GitHub repository.

**Important**: This dashboard contains real financial data. Always keep the repository PRIVATE.

---

## Option 1: GitHub Desktop (Recommended for Non-Developers)

### Windows / Mac

1. **Install GitHub Desktop**: https://desktop.github.com/
2. **Sign in** with your GitHub account
3. **Create a new repository**:
   - File → New Repository
   - Name: `financial-dashboard`
   - Local path: wherever you want the folder (e.g., `C:\Users\YourName\Documents`)
   - Check "Initialize with a README" (optional — you can add one later)
   - Click **Create Repository**
4. **Add your files**:
   - Click "Show in Explorer" (Windows) or "Show in Finder" (Mac)
   - Copy your `.html` dashboard file and `README.md` into this folder
5. **Commit**:
   - Go back to GitHub Desktop — it shows the new files as changes
   - Type a commit message (e.g., "initial dashboard")
   - Click **Commit to main**
6. **Publish**:
   - Click **Publish repository** in the top bar
   - **CHECK "Keep this code private"**
   - Click **Publish Repository**
7. **Done** — your repo is at `https://github.com/YOUR_USERNAME/financial-dashboard` (private)

### Updating Later

1. Edit the HTML file in the folder
2. Open GitHub Desktop — it shows the changes
3. Type a commit message, click Commit, then click **Push origin**

---

## Option 2: Command Line (Git)

### Prerequisites
- Git installed (`git --version` to check)
- GitHub account

### Windows (PowerShell or Command Prompt)

```bash
# Navigate to your file
cd "C:\Users\YourName\Documents\financial-dashboard"

# Initialize repo
git init
git add .
git commit -m "initial dashboard"
git branch -M main

# Connect to GitHub (create the empty repo on github.com/new first — select PRIVATE, no README)
git remote add origin https://github.com/YOUR_USERNAME/financial-dashboard.git
git push -u origin main
```

### Mac / Linux (Terminal)

```bash
cd ~/Documents/financial-dashboard
git init
git add .
git commit -m "initial dashboard"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/financial-dashboard.git
git push -u origin main
```

### Authentication

GitHub no longer accepts passwords for git push. You need either:

**Option A: Personal Access Token (easiest)**
1. Go to https://github.com/settings/tokens
2. Generate new token (classic)
3. Check the `repo` scope
4. Copy the token
5. When git asks for your password, paste the token instead

**Option B: SSH Key**
1. Generate: `ssh-keygen -t ed25519 -C "your_email@example.com"`
2. Add to GitHub: Settings → SSH and GPG keys → New SSH key
3. Use SSH remote: `git remote set-url origin git@github.com:YOUR_USERNAME/financial-dashboard.git`

### Updating Later

```bash
cd "C:\Users\YourName\Documents\financial-dashboard"
git add .
git commit -m "updated balances March 2026"
git push
```

---

## Option 3: Self-Hosting on Your Home Network

No GitHub needed. Just serve the file from any computer on your network.

### Quick (Python)
```bash
cd /path/to/dashboard/folder
python3 -m http.server 8080 --bind 0.0.0.0
```
Access from any device: `http://YOUR_LOCAL_IP:8080/dashboard.html`

Find your IP: `ipconfig` (Windows) or `ifconfig` / `ip addr` (Mac/Linux)

### Always-On (Linux/Raspberry Pi — systemd)
```bash
sudo tee /etc/systemd/system/finance-dash.service << 'EOF'
[Unit]
Description=Financial Dashboard
After=network.target

[Service]
WorkingDirectory=/home/youruser/dashboard
ExecStart=/usr/bin/python3 -m http.server 8080 --bind 0.0.0.0
Restart=always

[Install]
WantedBy=multi-user.target
EOF

sudo systemctl enable finance-dash
sudo systemctl start finance-dash
```

### Security Notes

- **Never expose port 8080 to the internet** — this has your real financial data
- Keep it on your local network only
- If you need remote access, use a VPN (Tailscale, WireGuard) instead of port forwarding
