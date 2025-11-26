# SPFx Azure DevOps Setup Script

Automated SharePoint Framework project setup with Azure DevOps package feed integration.

## 🎯 What This Does

Automates the complete SPFx development environment setup:
- ✅ Creates new SPFx project using Yeoman generator
- ✅ Configures Azure DevOps Artifacts package feed
- ✅ Installs all dependencies
- ✅ Sets up spfx-fast-serve for rapid development
- ✅ Generates launch scripts and documentation
- ✅ Configures secure .npmrc with PAT authentication

## 📋 Prerequisites

Install these before running the script:

1. **Node.js v18.x** - [Download](https://nodejs.org)
2. **npm** (comes with Node.js)
3. **PowerShell** (built into Windows)

## 🚀 Quick Start

### Method 1: Double-Click (Easiest)

1. Download both files:
   - `RUN-SETUP.bat`
   - `SPFx-Azure-Setup.ps1`
2. Put them in the same folder
3. **Double-click `RUN-SETUP.bat`**
4. Answer the prompts

### Method 2: PowerShell

```powershell
# Navigate to script directory
cd C:\Path\To\Scripts

# Run the script
.\SPFx-Azure-Setup.ps1
```

## 📝 What You'll Be Asked

The script will prompt you for:

1. **Project Name** - e.g., `MyCustomWebPart`
2. **SharePoint URL** - e.g., `https://veritas.sharepoint.com` (default)
3. **Site Name** - e.g., `dev` (for testing)
4. **Azure DevOps Organization** - e.g., `mycompany`
5. **Azure DevOps Project ID** - GUID from your Azure DevOps project
6. **Package Feed Name** - e.g., `shared-packages`
7. **Package Scope** - e.g., `@mycompany`
8. **Personal Access Token (PAT)** - from Azure DevOps

Then the Yeoman SPFx generator will ask:
- Framework choice (React, etc.)
- Web part name
- Description

## 🔑 Getting Your Azure DevOps Information

### Organization Name
Your Azure DevOps URL: `https://dev.azure.com/{ORGANIZATION}/`

### Project ID (GUID)
1. Go to your Azure DevOps project
2. Click "Project Settings" (bottom left)
3. Look at the URL: `https://dev.azure.com/{org}/_settings/projects?id={PROJECT-ID}`
4. Copy the GUID after `id=`

### Package Feed Name
1. Go to "Artifacts" in Azure DevOps
2. Your feed name is shown at the top

### Personal Access Token (PAT)
1. Click your profile icon → "Personal access tokens"
2. Click "New Token"
3. Give it a name (e.g., "SPFx Development")
4. Set expiration (recommend 90 days)
5. Scopes: Check "Packaging (Read)"
6. Copy the token (you won't see it again!)

## 📂 What Gets Created

```
C:\SPFx\YourProject\
├── .npmrc                      # Azure DevOps credentials (DO NOT COMMIT)
├── .gitignore                  # Prevents .npmrc from being committed
├── README.md                   # Project documentation
├── START-DEV-SERVER.ps1        # Quick launcher script
├── src/                        # Your source code
├── config/                     # SPFx configuration
├── node_modules/               # Dependencies
└── package.json                # Project manifest
```

## 🛠️ After Setup

### Start Development

```powershell
cd C:\SPFx\YourProject
npm run serve
```

Or double-click `START-DEV-SERVER.ps1`

### Access SharePoint Workbench

1. **First Tab** - Trust SSL certificate:
   ```
   https://localhost:4321/temp/manifests.js
   ```
   Accept the warning, keep tab open

2. **Second Tab** - Open workbench:
   ```
   https://veritas.sharepoint.com/sites/{yoursite}/_layouts/15/workbench.aspx?debug=true&noredir=true&debugManifestsFile=https://localhost:4321/temp/manifests.js
   ```

### Install Custom Packages

After setup, you can install packages from your Azure DevOps feed:

```powershell
npm install @mycompany/shared-library
```

## 🔒 Security Best Practices

### ⚠️ CRITICAL: Protect Your PAT Token

The `.npmrc` file contains your Personal Access Token in plain text.

**DO NOT:**
- Commit `.npmrc` to source control
- Share `.npmrc` with others
- Email or message the file
- Store it in cloud sync folders (Dropbox, OneDrive)

**DO:**
- Keep `.npmrc` only in your local project
- Use `.gitignore` to exclude it (script creates this)
- Set PAT expiration dates
- Rotate tokens regularly

### Verify .gitignore

The script creates a `.gitignore` file that includes:
```
.npmrc
```

Always verify before committing:
```powershell
git status
# .npmrc should NOT appear in the list
```

## 📦 Production Build

```powershell
# Clean
gulp clean

# Build for production
gulp build --ship
gulp bundle --ship
gulp package-solution --ship
```

Package location: `sharepoint/solution/*.sppkg`

### Deploy to SharePoint

1. Upload `.sppkg` to App Catalog
2. Click "Deploy"
3. Trust the solution
4. Add to site collection

## 🐛 Troubleshooting

### Script Won't Run - Execution Policy

```powershell
# Open PowerShell as Administrator
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

### npm install Fails

```powershell
# Clear npm cache
npm cache clean --force

# Delete node_modules and retry
rm -r node_modules
npm install
```

### Azure Feed Authentication Fails

Check:
1. PAT token is valid (not expired)
2. Token has "Packaging (Read)" permission
3. Organization name is correct
4. Project ID is correct (GUID format)
5. Feed name is correct

Regenerate token in Azure DevOps if needed.

### Server Won't Start

```powershell
gulp clean
npm run serve
```

### SSL Certificate Warnings

This is normal for local development:
1. Visit `https://localhost:4321/temp/manifests.js`
2. Click "Advanced" → "Proceed to localhost"
3. Keep this tab open while developing

## 📚 Resources

- [SPFx Documentation](https://docs.microsoft.com/sharepoint/dev/spfx/)
- [Azure Artifacts](https://docs.microsoft.com/azure/devops/artifacts/)
- [spfx-fast-serve](https://github.com/s-KaiNet/spfx-fast-serve)
- [Yeoman SPFx Generator](https://github.com/SharePoint/generator-sharepoint)

## 🤝 Contributing

Feel free to submit issues and enhancement requests!

## 📄 License

MIT License - feel free to use and modify

---

**Example Configuration:**
- SharePoint: `https://veritas.sharepoint.com`
- Azure DevOps: `https://dev.azure.com/mycompany`
- Package Feed: `shared-packages`
- Package Scope: `@mycompany`
