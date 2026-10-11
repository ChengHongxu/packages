# Homebrew Formulas

Homebrew formulas for packages in this tap.

## Future Support

When Homebrew support is added, formulas will be placed here:

```
homebrew/Formula/
├── setproxy.rb
└── other-package.rb
```

## Creating a Homebrew Formula

Example structure for future reference:

```ruby
class Setproxy < Formula
  desc "Cross-platform proxy environment variable manager"
  homepage "https://github.com/ChengHongxu/setproxy"
  url "https://github.com/ChengHongxu/setproxy/archive/v0.2.0.tar.gz"
  sha256 "..."
  license "MIT"

  depends_on "rust" => :build

  def install
    system "cargo", "install", *std_cargo_args
  end

  test do
    system "#{bin}/_setproxy", "--version"
  end
end
```

## Usage

```bash
brew tap chenghongxu/packages
brew install setproxy
```
