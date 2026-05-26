# Life Hacks & Practical Guides

Real-world tips, terminal tricks, and workflows — tested on my own setup, written for developers and power users.

This repo is a **growing collection of articles**. New life hacks and guides will be added over time.

![Articles](https://img.shields.io/badge/Articles-1-blue)
![Language](https://img.shields.io/badge/Language-English%20%7C%20Bengali-lightgrey)
![License](https://img.shields.io/badge/License-MIT-green)

---

## Articles

| # | Title | Language | Topic | Read |
|---|---|---|---|---|
| 1 | How I Reclaimed 125 GB on macOS Overnight | EN · [BN](./free-macos-storage-bn.md) | macOS · Terminal · Storage | [English](./free-macos-storage.md) · [বাংলা](./free-macos-storage-bn.md) |

### Coming soon

More guides on productivity, dev environment, and everyday fixes — stay tuned.

---

## What's in this repo?

Short, actionable write-ups — not fluff. Each article covers:

- A real problem I ran into
- Step-by-step fix with commands or workflow
- What worked, what to avoid, and verified results

**Current focus areas:** macOS, developer tooling, storage & performance, terminal workflows.

---

## Featured: macOS Storage Cleanup

> **245 GB SSD · 241 GB used · 4 GB free → 129 GB free after cleanup**

Reclaimed **125 GB** overnight using Terminal only — no third-party cleaner apps.

| Target | Space back |
|---|---|
| `.angular` build cache | 82 GB |
| Aerial wallpaper videos | 9.4 GB |
| Chrome, JetBrains & dev caches | ~33.6 GB |

**[English →](./free-macos-storage.md)** · **[বাংলা →](./free-macos-storage-bn.md)**

---

## Repo structure

```
Blog-Post/
├── README.md                   # Article index (you are here)
├── free-macos-storage.md       # macOS storage cleanup (English)
├── free-macos-storage-bn.md    # macOS storage cleanup (Bengali)
└── ...                         # Future articles go here
```

### Adding a new article

1. Create a new `.md` file in the repo root (e.g. `docker-cleanup-guide.md`)
2. Add a row to the **Articles** table in this README
3. Keep each article self-contained — commands, warnings, and results in one file

---

## Topics

`life-hacks` · `macos` · `terminal` · `devtools` · `productivity` · `web-development` · `devops`

---

## Disclaimer

All guides reflect my personal setup and testing. Your environment may differ. Commands like `rm -rf` are irreversible — always verify paths before running.

---

## License

MIT — free to use, share, and modify.

---

Star this repo if you find these hacks useful — more articles coming.
