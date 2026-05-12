# Flow — Kanban Todo (Day 89)

A glassmorphism-style Kanban board built in a single `index.html`. No backend, no build step — data persists in `localStorage`.

## Features

- Three columns: **To Do / In Progress / Done**
- Drag-and-drop between columns
- Add / edit / delete tasks with title, description, priority
- Priority chips (Low / Medium / High) with color-coded left border
- Counter chips + stats strip
- "Clear Done" bulk action
- Keyboard shortcut: **N** to open new-task modal, **Esc** to close
- Persists to `localStorage` — refresh-proof
- Mobile responsive (stacks to single column under 900px)
- Floating **Buy Me a Coffee** button

## Run locally

Just open `index.html` in your browser. That's it.

```bash
open index.html        # macOS
```

---

## Part 1 — Set up real Buy Me a Coffee payments

The button currently points to `https://buymeacoffee.com/yourname`. To make it actually pay you:

### Step 1: Create your BMC account
1. Go to **https://www.buymeacoffee.com/** → "Start my page".
2. Sign up with email or Google.
3. Pick a username — this becomes your URL: `buymeacoffee.com/<username>`. Choose carefully; it's hard to change.
4. Fill out your page: profile photo, short bio, cover image. (BMC pages with a photo + bio convert ~3× better — same UX rule as App Store screenshots.)

### Step 2: Connect a payout method
- BMC supports **Stripe** (cards, Apple/Google Pay) and **PayPal**.
- In dashboard → **Payments** → connect Stripe (recommended — lower fees, faster payout).
- For Stripe you need: bank account, government ID, tax info. Takes ~10 min.
- BMC takes **5%** + Stripe fees (~2.9% + 30¢). So a $5 coffee → you net ~$4.45.

### Step 3: Wire the button in `index.html`
Find this line near the bottom:
```html
<a class="bmc-fab" href="https://buymeacoffee.com/yourname" ...>
```
Replace `yourname` with your actual BMC username. Save. Done.

### Step 4 (optional): Add the BMC widget instead of just a link
BMC offers an embeddable widget that opens a modal on your site (no redirect). In your dashboard → **Widgets** → "Button" → copy the script tag and paste it inside `<head>`. It's ~5 lines.

### Step 5: Test the payment flow
- Use BMC's **test mode** (Stripe test cards: `4242 4242 4242 4242`) to verify a payment lands in your dashboard before going live.

---

## Part 2 — Host it on the web (free)

Pick one. GitHub Pages is the path of least resistance.

### Option A — GitHub Pages (Recommended)

1. Create a free GitHub account if you don't have one.
2. Create a new repo, e.g. `flow-kanban`. Make it **public**.
3. Upload `index.html` (drag-drop in the GitHub web UI works).
4. Repo → **Settings** → **Pages** → Source: `Deploy from branch` → Branch: `main` → `/ (root)` → **Save**.
5. Wait ~30 sec. Your site is live at `https://<your-github-username>.github.io/flow-kanban/`.

### Option B — Netlify (drag-and-drop deploy)

1. Sign up at **https://www.netlify.com/** (free tier).
2. Dashboard → **Add new site** → **Deploy manually** → drag the folder containing `index.html` onto the page.
3. Live in ~10 sec at `https://<random-name>.netlify.app`. You can rename the subdomain in site settings.

### Option C — Vercel
Similar to Netlify. `npm i -g vercel` → run `vercel` in the folder → follow prompts.

### Bonus: Custom domain
- Buy a domain ($10/yr — Namecheap, Cloudflare, Porkbun).
- In GitHub Pages / Netlify, point the custom domain at your site (they walk you through the DNS records).
- HTTPS is auto-issued via Let's Encrypt.

---

## Files

- `index.html` — the entire app (HTML + CSS + JS in one file)
- `D89_Reflection.md` — Obsidian reflection note
- `README.md` — this file

## What changes if you outgrow `localStorage`

`localStorage` is per-browser, per-device. If you want tasks to sync across your phone + laptop, you need a backend. Cheapest path:
- **Supabase** (free tier, Postgres + auth out of the box) — swap the `save()` and `load()` functions to hit the Supabase JS SDK. ~50 lines of code total.
- **Firebase Firestore** — same idea, Google ecosystem.

But for a personal portfolio piece + a tip jar, `localStorage` is plenty.
