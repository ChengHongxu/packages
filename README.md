# ChengHongxu's Package Repository

Personal package repository supporting multiple package managers.

## 📦 Available Package Managers

### Conda/Pixi

```bash
# Add channel
pixi config append channels "https://chenghongxu.github.io/packages/conda" --global

# Install
pixi global install setproxy
```

### Homebrew (Coming Soon)

```bash
# Add tap
brew tap chenghongxu/packages

# Install
brew install setproxy
```

## 📋 Available Packages

| Package | Version | Conda | Homebrew | Description |
|---------|---------|-------|----------|-------------|
| [setproxy](https://github.com/ChengHongxu/setproxy) | 0.2.0 | ✅ | 🚧 | Cross-platform proxy environment variable manager |

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
│       └── setproxy.rb (future)
├── .github/
│   └── workflows/
│       ├── update-conda.yml
│       └── update-homebrew.yml (future)
└── README.md
```

## 🔗 URLs

- **Conda Channel**: https://chenghongxu.github.io/packages/conda
- **Homebrew Tap**: https://github.com/ChengHongxu/packages (brew tap)
- **Repository**: https://github.com/ChengHongxu/packages

---

Powered by [rattler-build](https://github.com/prefix-dev/rattler-build), [pixi](https://pixi.sh), and [Homebrew](https://brew.sh)
