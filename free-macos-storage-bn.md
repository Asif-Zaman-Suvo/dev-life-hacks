# 🧹 Terminal দিয়ে macOS-এ রাতে ১২৫ GB মুক্ত — এখন ১২৯ GB ফাঁকা (তৃতীয় পক্ষের অ্যাপ ছাড়াই)

> **সংক্ষেপে** — আমার ২৪৫ GB Mac SSD-এ মাত্র ৪ GB ফাঁকা ছিল (২৪১ GB ব্যবহৃত)। শুধু built-in Terminal command দিয়ে invisible build cache ও system clutter খুঁজে **রাতে ১২৫ GB মুক্ত** করেছি — এখন **১২৯ GB ফাঁকা**। কোনো সন্দেহজনক cleaner অ্যাপ লাগেনি।

![macOS Storage](https://img.shields.io/badge/macOS-Tahoe-blue?logo=apple) ![Terminal](https://img.shields.io/badge/Tool-Terminal-black?logo=gnome-terminal) ![Storage Freed](https://img.shields.io/badge/Storage%20Freed-125%20GB-brightgreen) ![Free Now](https://img.shields.io/badge/Free%20Now-129%20GB-success)

[English version](./free-macos-storage.md)

---

## 📋 সূচিপত্র

- [সমস্যা](#সমস্যা)
- [ধাপ ১: Home ডিরেক্টরি Audit](#ধাপ-১-home-ডিরেক্টরি-audit)
- [ধাপ ২: Hidden Cache খুঁজে বের করা](#ধাপ-২-hidden-cache-খুঁজে-বের-করা)
- [ধাপ ৩: Build Cache নিরাপদে মুছুন](#ধাপ-৩-build-cache-নিরাপদে-মুছুন)
- [ধাপ ৪: Application Support Clutter বিশ্লেষণ](#ধাপ-৪-application-support-clutter-বিশ্লেষণ)
- [ধাপ ৫: Aerial Wallpaper Videos মুছুন](#ধাপ-৫-aerial-wallpaper-videos-মুছুন)
- [ধাপ ৬: System ও App Cache পরিষ্কার](#ধাপ-৬-system-ও-app-cache-পরিষ্কার)
- [⚠️ যা মুছবেন না](#️-যা-মুছবেন-না)
- [চূড়ান্ত ফলাফল](#চূড়ান্ত-ফলাফল)
- [ভবিষ্যতের জন্য Best Practices](#ভবিষ্যতের-জন্য-best-practices)

---

## সমস্যা

যেকোনো ডেভেলপার **"Storage Almost Full"** নোটিফিকেশন দেখে চিন্তিত হয় — বিশেষ করে build-এর মাঝখানে বা macOS update-এর ঠিক আগে।

আমার অবস্থা:

| মেট্রিক | অবস্থা |
|---|---|
| মোট SSD | ২৪৫ GB |
| ব্যবহৃত স্থান | **২৪১ GB** 🔴 |
| ফাঁকা স্থান | **৪ GB-এর কম** 😱 |

macOS-এর native Storage Panel কোনো সাহায্য করেনি — সব কিছু vague **"System Data"** bucket-এ ঢেলে দিয়েছিল। সময় এসেছিল নিজে হাতে সমাধান করার।

---

## ধাপ ১: Home ডিরেক্টরি Audit

Home folder-এর সবচেয়ে বড় ডিরেক্টরি দেখতে এই command চালান:

```bash
du -sh ~/* 2>/dev/null | sort -rh | head -15
```

**Command ব্যাখ্যা:**

| Flag | কাজ |
|---|---|
| `du -sh` | Disk usage, summary mode, human-readable size |
| `~/*` | Home directory-র প্রতিটি item |
| `2>/dev/null` | "Permission denied" error লুকায় |
| `sort -rh` | Size অনুযায়ী বড় থেকে ছোট সাজায় |
| `head -15` | শুধু top 15 ফলাফল দেখায় |

**আমার output:**

```
 92G    /Users/username/Projects
 33G    /Users/username/Library
8.7G    /Users/username/PET
1.7G    /Users/username/PH
261M    /Users/username/Downloads
```

`Projects` ফোল্ডার একাই **৯২ GB** দখল করছিল। পরবর্তী ধাপে এর ভেতরে যাওয়া দরকার।

---

## ধাপ ২: Hidden Cache খুঁজে বের করা

Dot-folder (`.angular`, `.git`, `.cache` ইত্যাদি) **Finder-এ invisible** — default `ls`-এও দেখা যায় না। Gigabyte-এর cache এখানেই লুকিয়ে থাকে।

নির্দিষ্ট project directory-র hidden folder audit করতে:

```bash
du -sh ~/Projects/MyAngularApp/.* 2>/dev/null | sort -rh | head -10
```

**আমার output:**

```
 82G    /Users/username/Projects/MyAngularApp/.angular
206M    /Users/username/Projects/MyAngularApp/.git
```

একটি `.angular` folder-ই **৮২ GB** — Angular CLI-এর incremental build cache, মাসের পর মাস active development-এ জমেছে।

---

## ধাপ ৩: Build Cache নিরাপদে মুছুন

> [!TIP]
> **`.angular` মুছা নিরাপদ?** হ্যাঁ। এতে source code বা configuration নেই — শুধু compiled intermediate ফাইল। পরবর্তী `ng serve` বা `ng build` প্রথমবার একটু বেশি সময় নেবে, cache rebuild হলে আবার normal speed-এ ফিরবে।

```bash
rm -rf ~/Projects/MyAngularApp/.angular
```

💰 **মুক্ত স্থান: ৮২ GB**

---

## ধাপ ৪: Application Support Clutter বিশ্লেষণ

`~/Library` folder ছিল আরেকটি **৩৩ GB**। `Application Support`-এ drill down:

```bash
du -sh ~/Library/Application\ Support/* 2>/dev/null | sort -rh | head -15
```

**আমার output:**

```
9.4G    /Users/username/Library/Application Support/com.apple.wallpaper
9.1G    /Users/username/Library/Application Support/Google
1.0G    /Users/username/Library/Application Support/Cursor
850M    /Users/username/Library/Application Support/Postman
```

---

## ধাপ ৫: Aerial Wallpaper Videos মুছুন

macOS নীরবে **HD cinematic video wallpaper** download করে Apple TV-style screensaver-এর জন্য। Dynamic/aerial wallpaper ব্যবহার না করলে এই folder pure waste:

```bash
# আগে সাইজ যাচাই করুন
du -sh ~/Library/Application\ Support/com.apple.wallpaper/*

# Aerial screensaver ব্যবহার না করলে নিরাপদে মুছুন
rm -rf ~/Library/Application\ Support/com.apple.wallpaper/aerials
```

💰 **মুক্ত স্থান: ৯.৪ GB**

---

## ধাপ ৬: System ও App Cache পরিষ্কার

User cache folder check করুন:

```bash
du -sh ~/Library/Caches/* 2>/dev/null | sort -rh | head -10
```

Developer-নির্দিষ্ট safe cache — purge করতে পারেন:

```bash
# Google Chrome cache
rm -rf ~/Library/Caches/Google

# Playwright test browser binaries (প্রয়োজনে auto re-download)
rm -rf ~/Library/Caches/ms-playwright
rm -rf ~/Library/Caches/ms-playwright-go

# Node.js native module build cache
rm -rf ~/Library/Caches/node-gyp

# TypeScript compiler cache
rm -rf ~/Library/Caches/typescript

# পুরানো JetBrains IDE config & cache (VS Code/Cursor-এ switch করলে)
rm -rf ~/Library/Caches/JetBrains
rm -rf ~/Library/Application\ Support/JetBrains
```

---

## ⚠️ যা মুছবেন না

> [!CAUTION]
> `rm -rf` irreversible। একটা typo সপ্তাহ নষ্ট করতে পারে। Execute করার আগে path দুবার যাচাই করুন।

| Path | কেন স্পর্শ করবেন না |
|---|---|
| `.git` | Local git history — মুছলে **repository tracking নষ্ট** |
| `node_modules` (সক্রিয় project) | Project ভেঙে যাবে; `npm install` / `pnpm install` লাগবে |
| `~/Library/.../Google` (core folder) | Chrome profile, password, extension মুছে যেতে পারে |
| `/System` বা `/System Data` | Core macOS OS file — মুছলে **machine brick** হতে পারে |

---

## চূড়ান্ত ফলাফল

### মুক্ত স্থানের Breakdown

| Cleanup Target | মুক্ত স্থান |
|---|---|
| `.angular` Build Cache | 82.0 GB |
| Aerial Wallpaper Videos | 9.4 GB |
| Chrome, JetBrains & Dev Caches | ~33.6 GB |
| **মোট মুক্ত** | **125 GB** |

> **গণনা যাচাই:** 82 + 9.4 + 33.6 = 125 GB মুক্ত · 241 GB used − 125 GB = **116 GB used** · 245 GB − 116 GB = **129 GB free**

### আগে vs পরে

| মেট্রিক | আগে | পরে |
|---|---|---|
| মোট SSD | 245 GB | 245 GB |
| ব্যবহৃত স্থান | 241 GB 🔴 | 116 GB ✅ |
| ফাঁকা স্থান | 4 GB 😱 | **129 GB** 🎉 |

---

## ভবিষ্যতের জন্য Best Practices

**১. Rogue `node_modules` খুঁজুন**

Inactive side-project নীরবে gigabyte dependency জমিয়ে রাখে। এক command-এ সব খুঁজুন:

```bash
find ~ -name "node_modules" -type d -maxdepth 5 2>/dev/null | xargs du -sh 2>/dev/null | sort -rh
```

**২. Native Storage GUI-তে পুরোপুরি বিশ্বাস করবেন না**

Graphical disk utility developer asset (Docker image, build bundle, local database) misclassify করে generic "System Data"-তে। Accurate reading-এর জন্য `du -sh` ব্যবহার করুন।

**৩. মাসিক audit চালান**

Routine-এ যোগ করুন — এক মিনিটের কম সময় লাগে:

```bash
du -sh ~/* 2>/dev/null | sort -rh | head -15
```

Mac halt হওয়ার আগেই anomaly ধরুন।

---

## উপসংহার

Healthy Mac রাখতে automated cleaning software দরকার নেই। কয়েকটি Terminal command দিয়ে exact folder চিহ্নিত, context বোঝা, এবং production code হারানো ছাড়াই নিরাপদে সরানো সম্ভব।

> **macOS Tahoe**-তে পরীক্ষিত। আপনার local environment ও dev stack অনুযায়ী ফলাফল ভিন্ন হতে পারে। `rm -rf` চালানোর আগে path দুবার যাচাই করুন।

---

*এই গাইড কাজে লাগলে repo-তে ⭐ Star দিন বা অন্য developer-দের সাথে share করুন!*
