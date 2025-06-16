---
title: Migrating from Zsh to Fish on macOS
lang: en
date: 2025-06-16 23:26:10
tags:
    - fish
categories:
    - fish
---

## Motivation

As a macOS and Arch Linux user, zsh was my default shell for more than 6 years. While zsh is easier to onboarding compared to bash, it still relies heavily on configuration.

Fish is a shell that is more intuitive, user-friendly, and helpful out-of-the-box. It provides syntax highlighting, auto-suggestions, and web-based configuration interface by default. In order to cut time spend on maintaining `.zshrc`, I decided to migrate to fish.

## Installation

For intel-based Macs, using Homebrew is the simplest way to install fish:

```bash
brew install fish fisher
fish --version
```

For Apple Silicon Macs, instead of using Homebrew (which installs to `/opt/homebrew` , it would be recommended to use the official installer from [https://fishshell.com](https://fishshell.com) will install fish to `/usr/local/bin` . This makes it easier to register as a login shell and avoid unexpected issues.

Then, add fish to the list of allowed shells:

```bash
echo /usr/local/bin/fish | sudo tee -a /etc/shells
chsh -s /usr/local/bin/fish
```

This will change login shell for the current user to fish.

## Migrating Zsh Configurations

Fish doesn’t use `.zshrc` ; it uses:

```bash
~/.config/fish/config.fish
```

So we need to manually migrate zsh settings to fish settings.

### Alias

Instead of adding alias in `config.fish`, we can set the aliases once using the `-s` option:

```bash
alias -s kubectl="kubecolor"
```

### Environment Variables

Instead of `export FAR=boo`, use universal variables:

```bash
set -Ux JAVA_HOME "/usr/local/opt/openjdk@17/libexec/openjdk.jdk/Contents/Home"
set -Ux GOROOT "/usr/local/opt/go@1.23/libexec"
set -Ux KIND_EXPERIMENTAL_PROVIDER podman
```

These variables will be automatically saved and applied across all sessions.

After that, we can use `set —show` to see variables:

```bash
set --show GOROOT
$GOROOT: set in universal scope, exported, with 1 elements
$GOROOT[1]: |/usr/local/opt/go@1.23/libexec|
```

### PATH Modifications

Instead of editing `$PATH` directly, fish provides a helper command `fish_add_path` to manage paths:

```bash
fish_add_path "/usr/local/opt/openjdk@17/bin" "/usr/local/opt/go@1.23/libexec/bin"
```

This also avoids duplication and is safer than manual appending.

## Conclusion

Fish offers easier configuration management, built-in suggestions, and syntax highlighting. Although fish is not POSIX-compliant, it is still an excellent choice for users who prefer clear, helpful tools.

Migrating from zsh to fish is fast and straightforward. With the right steps and proper configurations, we can simply enjoy a faster and friendlier command line environment.
