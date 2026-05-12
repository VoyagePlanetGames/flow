---
day: 89
project: Flow — Kanban Todo (web app, hostable)
date: 2026-05-10
tags: [100DaysOfCode, html, css, javascript, kanban, frontend, reflection]
---

# Day 89 — Flow Kanban Todo (Reflection)

> The right-of-passage todo list. I went Kanban (Trello-style) instead of a flat list, glassmorphism instead of plain, and wired up a Buy Me a Coffee tip jar so the same file that gets me through an interview can also accept $5 from a stranger.

## How I approached it

1. **Locked the scope first.** Three columns, drag-and-drop, localStorage, BMC button — no auth, no backend, no framework. The whole thing has to fit in one `index.html` so deploying = upload one file.
2. **Picked the visual identity before writing a line of JS.** Glassmorphism (frosted cards on a gradient with floating orbs) — instantly reads as "modern" without me having to design from scratch. Same trick the [[Linear (app)]] landing page uses.
3. **Built render-first.** Wrote `render()` to rebuild all three columns from a single `tasks[]` array. Every mutation (create / edit / delete / drag) just changes the array and calls `render()` + `save()`. Identical pattern to [[D85 Image Watermarking Desktop App]]'s `render_preview()` — one render function, no sync bugs.
4. **HTML5 drag-and-drop**, not a library. `dragstart` puts the task id in `dataTransfer`, the column's `drop` reads it back and updates `task.status`. ~15 lines, no dependencies.
5. **localStorage as the database.** `save()`/`load()` wrap `JSON.stringify`/`parse`. Seed data on first load so the empty board doesn't feel dead.
6. **BMC button as a floating action button** with a gentle bob animation — high enough contrast (#ffdd00 on dark) that it's the first thing the eye lands on.

## What was easy

- The visual layer. CSS variables + `backdrop-filter: blur()` + two radial gradients = polished look in maybe 80 lines of CSS.
- Drag-and-drop. Native HTML5 API is good enough; I don't need react-beautiful-dnd for three columns.
- Persistence. `localStorage` for a single-user todo list is two functions. No Supabase, no auth.

## What was hard

- **XSS via task title.** First draft used `innerHTML` to inject `t.title` into the card. A title of `<img src=x onerror=alert(1)>` would have executed. Switched to `textContent` for user-provided strings — same fix every web tutorial glosses over.
- **Drag ghost vs. real card state.** The dragging card needs to look "lifted" but still be in the DOM (HTML5 DnD requires it). Solved with a `.dragging` class that drops opacity to 0.4. Tried `display: none` first — broke the drop event.
- **Column drop targets.** `dragover` must call `preventDefault()` or `drop` never fires. Forgot it the first time and spent 5 min wondering why nothing worked. Classic DnD footgun.
- **Empty-state UX.** A column with no cards needs to *look* like a drop zone, not broken. Added a dashed placeholder card per empty column.
- **Mobile.** At <900px the three columns stack vertically. The FAB also has to not cover the last column's bottom card — fine because of the 120px page bottom padding.

## How I'd improve next time

- **Cloud sync.** Swap `localStorage` for Supabase so the board syncs across my phone + laptop. ~50 LOC change; same `save`/`load` interface.
- **Multi-board.** Add a sidebar with boards (Personal / Work / Side projects). Trello's core unit isn't the card — it's the board.
- **Card subtasks + checklists.** Once cards have nested checklists, this becomes a real product, not a toy.
- **Keyboard nav.** I added `N` for new task but didn't add arrow keys to move focus between cards / columns. Would matter for power users.
- **Undo.** A 5-second snackbar after delete ("Undo"). One state snapshot, one timer.
- **Analytics.** Plausible or PostHog drop-in script. If I'm hosting this and asking for tips, I want to know if the BMC click rate is 0% or 5%.

## Biggest learning today

**A single render function plus a serializable state array is the entire "framework" you need for a todo-sized app.** No React, no Vue, no virtual DOM. The pattern:

```
state = load()
render(state)
onAction → mutate state → save() → render()
```

…is literally what every JS framework is doing under the hood. For an app this size, the framework is the overhead, not the feature. Same lesson as [[D85 Image Watermarking Desktop App]]: framework when you need it, function when you don't.

The second learning is monetization-flavored: **the cost of adding a tip jar is one `<a>` tag.** The cost of building a polite, well-placed tip jar that someone actually clicks is the entire rest of the page. The button is 1% of the work; trust and aesthetics are the other 99%. Same UX truth as a [[F2P]] store: the IAP SKU isn't the conversion driver — the store presentation is.

## What I'd do differently

- **Wireframe the card before building.** I rebuilt the card layout three times (chip position, action buttons, date placement). Five minutes on paper would have saved 30 min of CSS fiddling.
- **Test on mobile earlier.** I built desktop-first and the FAB initially overlapped the "Clear Done" button at 380px wide. Should have opened DevTools mobile view from move one.
- **Plan the BMC integration first**, not last. The button affects the layout (FAB position, bottom padding) and the visual hierarchy. Treating it as an afterthought caused one redo of the footer.

## Product lens

This is the [[Trello]] cold-start problem in miniature: the empty board is the worst possible first impression. Seed data fixes it. Every onboarding I've shipped on a mobile game does the same — pre-fill the first life, the first city, the first deck. The principle: *never show a user an empty container in their first 10 seconds.* It's the difference between "I get it" and "I bounce."

Also: the **Buy Me a Coffee** model is essentially [[free-to-play]] for content — the app is free, payment is optional, and the conversion driver is delight. The same logic that makes a $5 starter pack convert at 2% in [[BitLife]] makes a $5 coffee convert at <0.5% on a tip jar. The lesson isn't "tip jars are bad," it's that **passive monetization needs scale**. A tip jar on a portfolio site is a flex, not a business. The actual revenue play, if I wanted one, would be a $3/month "Pro" tier with cloud sync — same idea as a F2P subscription.

## Pricing math (if I were really shipping this)

| Model | Conv. rate | Avg / user | Users to make $1K/mo |
|---|---|---|---|
| BMC tip jar | 0.3% | $5 one-time | ~67,000 visitors / mo |
| $3/mo Pro sub | 2% | $3/mo recurring | ~17,000 users |
| $20 lifetime | 1% | $20 one-time | ~5,000 buyers / mo |

The tip jar is the *worst* unit economics — but also the lowest friction to build. Right call for Day 89.

---

**Files:** [[index.html]] · [[README.md]]
**Related:** [[100 Days of Code MOC]] · [[D85 Image Watermarking Desktop App]] · [[D81]] · [[Trello]] · [[Glassmorphism]]
