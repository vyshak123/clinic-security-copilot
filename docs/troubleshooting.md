# Troubleshooting log

Every problem hit during the build: what broke, why, and how it was fixed.

| Date | Phase | Problem | Cause | Fix |
|---|---|---|---|---|
| 2026-10-21 | 0 | `brew install terraform` found no formula | Terraform is no longer in Homebrew's core list | Installed from HashiCorp's tap: `brew tap hashicorp/tap` then `brew install hashicorp/tap/terraform` |
| 2026-10-22 | 0 | `brew install azure-cli` failed: "Command Line Tools does not support macOS 14" | Apple Command Line Tools were outdated for the installed macOS | Removed and reinstalled them (`sudo rm -rf /Library/Developer/CommandLineTools`, `sudo xcode-select --install`), then reinstalled Azure CLI 2.91.0 |
