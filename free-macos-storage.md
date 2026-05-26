# 🧹 How I Reclaimed 125 GB on macOS Overnight — 129 GB Free Now (Terminal, No Third-Party Apps)

> **TL;DR** — My 245 GB Mac SSD had less than 4 GB free (241 GB used). Using only built-in Terminal commands, I reclaimed **125 GB overnight** and now have **129 GB free** — by hunting down invisible build caches and system clutter. No sketchy cleaner apps required.

![macOS Storage](https://img.shields.io/badge/macOS-Tahoe-blue?logo=apple) ![Terminal](https://img.shields.io/badge/Tool-Terminal-black?logo=gnome-terminal) ![Storage Freed](https://img.shields.io/badge/Storage%20Freed-125%20GB-brightgreen) ![Free Now](https://img.shields.io/badge/Free%20Now-129%20GB-success)

[বাংলা সংস্করণ](./free-macos-storage-bn.md)

---

## 📋 Table of Contents

- [The Problem](#the-problem)
- [Step 1: Audit Your Home Directory](#step-1-audit-your-home-directory)
- [Step 2: Uncover Hidden Cache Monsters](#step-2-uncover-hidden-cache-monsters)
- [Step 3: Safely Purge Build Caches](#step-3-safely-purge-build-caches)
- [Step 4: Investigate Application Support Clutter](#step-4-investigate-application-support-clutter)
- [Step 5: Delete Heavy Aerial Wallpapers](#step-5-delete-heavy-aerial-wallpapers)
- [Step 6: Clean System & App Caches](#step-6-clean-system--app-caches)
- [⚠️ What NOT to Delete](#️-what-not-to-delete)
- [Final Results](#final-results)
- [Best Practices Going Forward](#best-practices-going-forward)

---

## The Problem

Every developer knows the dread of the **"Storage Almost Full"** notification — especially mid-build or right before a macOS update.

My situation:

| Metric | Status |
|---|---|
| Total SSD Capacity | 245 GB |
| Used Storage | **241 GB** 🔴 |
| Free Storage | **< 4 GB** 😱 |

The native macOS Storage Panel was no help — it dumped everything into a vague **"System Data"** bucket. Time to take matters into my own hands.

---

## Step 1: Audit Your Home Directory

Run this command to see the largest directories in your home folder:

```bash
du -sh ~/* 2>/dev/null | sort -rh | head -15
```

**Command breakdown:**

| Flag | What it does |
|---|---|
| `du -sh` | Disk usage, summary mode, human-readable sizes |
| `~/*` | Every item in your home directory |
| `2>/dev/null` | Suppresses "permission denied" errors |
| `sort -rh` | Sorts by size, largest first |
| `head -15` | Shows only the top 15 results |

**My output:**

```
 92G    /Users/username/Projects
 33G    /Users/username/Library
8.7G    /Users/username/PET
1.7G    /Users/username/PH
261M    /Users/username/Downloads
```

The `Projects` folder alone was eating **92 GB**. Time to dig deeper.

---

## Step 2: Uncover Hidden Cache Monsters

Dot-folders (`.angular`, `.git`, `.cache`, etc.) are **invisible in Finder** and skipped by `ls` by default. This is where gigabytes go to hide.

Audit hidden folders inside a project directory:

```bash
du -sh ~/Projects/MyAngularApp/.* 2>/dev/null | sort -rh | head -10
```

**My shocking output:**

```
 82G    /Users/username/Projects/MyAngularApp/.angular
206M    /Users/username/Projects/MyAngularApp/.git
```

A single `.angular` folder had ballooned to **82 GB** — the Angular CLI's incremental build cache, accumulated over months of active development.

---

## Step 3: Safely Purge Build Caches

> [!TIP]
> **Is it safe to delete `.angular`?** Yes. It contains no source code or configuration — only compiled intermediaries. Your next `ng serve` or `ng build` will be slower once as it rebuilds the cache from scratch, then return to normal speed.

```bash
rm -rf ~/Projects/MyAngularApp/.angular
```

💰 **Space reclaimed: 82 GB**

---

## Step 4: Investigate Application Support Clutter

The `~/Library` folder was another 33 GB. Drill into `Application Support`:

```bash
du -sh ~/Library/Application\ Support/* 2>/dev/null | sort -rh | head -15
```

**My output:**

```
9.4G    /Users/username/Library/Application Support/com.apple.wallpaper
9.1G    /Users/username/Library/Application Support/Google
1.0G    /Users/username/Library/Application Support/Cursor
850M    /Users/username/Library/Application Support/Postman
```

---

## Step 5: Delete Heavy Aerial Wallpapers

macOS silently downloads **HD cinematic video wallpapers** for Apple TV-style screensavers. If you don't use dynamic/aerial wallpapers, this folder is pure waste.

```bash
# Verify the size first
du -sh ~/Library/Application\ Support/com.apple.wallpaper/*

# Safe to delete if you don't use aerial screensavers
rm -rf ~/Library/Application\ Support/com.apple.wallpaper/aerials
```

💰 **Space reclaimed: 9.4 GB**

---

## Step 6: Clean System & App Caches

Check your user cache folder:

```bash
du -sh ~/Library/Caches/* 2>/dev/null | sort -rh | head -10
```

Safe, developer-specific caches you can purge:

```bash
# Google Chrome cache
rm -rf ~/Library/Caches/Google

# Playwright test browser binaries (re-downloaded automatically when needed)
rm -rf ~/Library/Caches/ms-playwright
rm -rf ~/Library/Caches/ms-playwright-go

# Node.js native module build cache
rm -rf ~/Library/Caches/node-gyp

# TypeScript compiler cache
rm -rf ~/Library/Caches/typescript

# Old JetBrains IDE configs & caches (if you've moved to VS Code/Cursor)
rm -rf ~/Library/Caches/JetBrains
rm -rf ~/Library/Application\ Support/JetBrains
```

---

## ⚠️ What NOT to Delete

> [!CAUTION]
> `rm -rf` is irreversible. One typo can ruin your week. Always double-check paths before executing.

| Path | Why You Must Avoid It |
|---|---|
| `.git` | Contains your entire local git history. Deleting this **destroys your repository tracking**. |
| `node_modules` (active projects) | Breaks the project until you re-run `npm install` / `pnpm install`. |
| `~/Library/.../Google` (core folder) | Can wipe Chrome profiles, saved passwords, and extensions. |
| `/System` or `/System Data` | Core macOS OS files. Deleting these **can brick your machine**. |

---

## Final Results

### Space Reclaimed Breakdown

| Cleanup Target | Reclaimed |
|---|---|
| `.angular` Build Cache | 82.0 GB |
| Aerial Wallpaper Videos | 9.4 GB |
| Chrome, JetBrains & Dev Caches | ~33.6 GB |
| **Total Reclaimed** | **125 GB** |

> **Math check:** 82 + 9.4 + 33.6 = 125 GB reclaimed · 241 GB used − 125 GB = **116 GB used** · 245 GB − 116 GB = **129 GB free**

### Before vs. After

| Metric | Before | After |
|---|---|---|
| Total SSD Capacity | 245 GB | 245 GB |
| Used Storage | 241 GB 🔴 | 116 GB ✅ |
| Free Storage | 4 GB 😱 | **129 GB** 🎉 |

---

## Best Practices Going Forward

**1. Hunt down rogue `node_modules`**

Inactive side-projects silently hoard gigabytes of old dependencies. Find them all at once:

```bash
find ~ -name "node_modules" -type d -maxdepth 5 2>/dev/null | xargs du -sh 2>/dev/null | sort -rh
```

**2. Don't trust the native Storage GUI**

The graphical disk utility misclassifies developer assets (Docker images, build bundles, local databases) as generic "System Data". Always use `du -sh` for accurate readings.

**3. Run monthly audits**

Add this to your routine — takes less than a minute:

```bash
du -sh ~/* 2>/dev/null | sort -rh | head -15
```

Spot anomalies before your Mac grinds to a halt.

---

## Conclusion

You don't need automated cleaning software to maintain a healthy Mac. With a handful of Terminal commands, you can pinpoint the exact folders draining your storage, understand their context, and safely remove them — without losing a single line of production code.

> Validated on **macOS Tahoe**. Results will vary based on your local environment and active dev stack. Always double-check paths before running `rm -rf`.

---

*If this helped you, consider starring ⭐ this repo or sharing it with a fellow developer!*
