# Setup Instructions for packages Repository

Multi-package-manager repository supporting Conda/Pixi and Homebrew.

## 🗂️ Repository Structure

```
packages/
├── conda/                    # Conda channel
│   ├── linux-64/
│   ├── osx-64/
│   ├── osx-arm64/
│   ├── win-64/
│   └── noarch/
├── homebrew/                 # Homebrew tap
│   └── Formula/
├── .github/workflows/
│   ├── update-conda.yml
│   └── update-homebrew.yml (future)
└── README.md
```

## 1. Create Repository on GitHub

1. Go to https://github.com/new
2. Repository name: `packages`
3. Description: "Personal package repository for multiple package managers"
4. Public repository
5. **Do NOT** initialize with README
6. Create repository

## 2. Initialize and Push

```bash
cd /tmp/packages-repo-template

# Initialize git
git init
git add .
git commit -m "Initial commit: multi-package-manager repository structure"

# Add remote and push
git remote add origin https://github.com/ChengHongxu/packages.git
git branch -M main
git push -u origin main
```

## 3. Enable GitHub Pages

1. Go to repository Settings → Pages
2. Source: Deploy from a branch
3. Branch: `gh-pages` / `(root)`
4. Save

**Note**: The `gh-pages` branch will be created when first package is added.

## 4. Create GitHub Personal Access Token (PAT)

1. Go to https://github.com/settings/tokens/new
2. Note: "Package repository dispatch"
3. Scopes: `repo` + `workflow`
4. Generate token
5. **Copy the token**

## 5. Add Secret to Package Repositories

For each package repo (e.g., setproxy):

1. Go to package repo Settings → Secrets → Actions
2. New repository secret
3. Name: `CHANNEL_DISPATCH_TOKEN`
4. Value: Paste the PAT
5. Add secret

## 6. Update Package Repository Workflows

In each package repo's `.github/workflows/release.yml`, update the repository_dispatch event type:

```yaml
- name: Trigger packages repo update
  uses: peter-evans/repository-dispatch@v3
  with:
    token: ${{ secrets.CHANNEL_DISPATCH_TOKEN }}
    repository: ChengHongxu/packages
    event-type: add-conda-package  # Changed from add-package
    client-payload: |
      {
        "package": "setproxy",
        "version": "${{ steps.version.outputs.VERSION }}",
        "run_id": "${{ github.run_id }}"
      }
```

## 7. Test the Setup

### Test Conda Channel

```bash
cd /path/to/setproxy
git tag v0.2.0
git push origin v0.2.0
```

Wait for workflows, then verify:

```bash
# Check channel
curl https://chenghongxu.github.io/packages/conda/linux-64/repodata.json

# Add and install
pixi config append channels "https://chenghongxu.github.io/packages/conda" --global
pixi global install setproxy
```

## 8. Future: Adding Homebrew Support

When ready to add Homebrew:

1. Create formula in `homebrew/Formula/setproxy.rb`
2. Add `.github/workflows/update-homebrew.yml`
3. Update package repo to trigger `add-homebrew-package` event

Users will then be able to:

```bash
brew tap chenghongxu/packages
brew install setproxy
```

## URLs by Package Manager

| Package Manager | Channel URL |
|----------------|-------------|
| Conda/Pixi | `https://chenghongxu.github.io/packages/conda` |
| Homebrew | `https://github.com/ChengHongxu/packages` (tap) |

## Troubleshooting

### Conda packages not found
```bash
# Verify channel structure
curl https://chenghongxu.github.io/packages/conda/linux-64/repodata.json

# Clear cache
pixi clean cache
```

### Workflow fails
- Check PAT has `repo` + `workflow` scopes
- Verify event-type is `add-conda-package`
- Check gh-pages branch exists

## Directory Benefits

✅ **Organized**: Clear separation between package managers  
✅ **Extensible**: Easy to add new package managers (apt, yum, etc.)  
✅ **Standard**: Homebrew taps expect formulas at root or in `Formula/`  
✅ **Clean URLs**: Each manager has its own subdirectory  

**Conda**: `https://chenghongxu.github.io/packages/conda`  
**Homebrew**: `brew tap chenghongxu/packages` (Git repo root)
