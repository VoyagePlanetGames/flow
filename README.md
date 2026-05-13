# Flow

**A Kanban board that gets out of your way.**

No signup. No cloud. No upsell. Open the link and start moving cards.

**Live:** https://voyageplanetgames.github.io/flow/

---

## What is Flow?

Flow is a single-page Kanban board for tracking your tasks. Three columns — **To Do**, **In Progress**, **Done** — and you drag cards between them as you make progress. That's it.

It's built for the moment you need a board *right now* and don't want to create an account, install an app, or invite a team. Open the URL, type a task, get back to work.

---

## How to use it

### Add a task

Click **+ New task** in the top right (or press `N` anywhere on the page).

Fill in:
- **Title** — what needs to be done. Required.
- **Description** — extra context. Optional.
- **Priority** — Low, Medium, or High. Shows up as a colored chip on the card.
- **Status** — which column the card lands in (defaults to To Do).

Hit **Save**. The card appears in the column you picked.

### Move a task

Just drag the card with your mouse and drop it on a different column. The card snaps into place and the count updates instantly. There's no save button — every action is auto-saved.

### Edit a task

Hover any card. The pencil icon appears in the top-right corner of the card. Click it to open the edit dialog. Change anything, hit Save.

### Delete a task

Hover the card. Click the trash icon next to the pencil. Confirm. Gone.

### Clear all Done tasks

When the Done column gets cluttered, click **Clear Done** in the top right. It nukes everything in the Done column at once (with a confirmation prompt). Use this at the end of a sprint, a day, or whenever the feeling of "I shipped a lot" needs to be reset to zero.

---

## Keyboard shortcuts

| Key | Action |
|-----|--------|
| `N` | Open the new-task dialog |
| `Esc` | Close any open dialog |
| `Enter` (in form) | Save the task |

---

## Where your data lives

Your tasks are stored in your browser's `localStorage`. This means:

- **Private by default.** Nothing is sent to a server. There is no server. I can't see your tasks, and neither can anyone else.
- **Per-device.** Your laptop and your phone each have their own board. They don't sync.
- **Persistent across refreshes** — but **not across browsers**. If you switch from Chrome to Safari, your tasks won't come along. Same browser, same device — they stay.
- **Cleared if you clear browser data.** If you wipe your browser's site data or use incognito mode, your tasks go with it.

This is intentional. Flow is for personal lists, scratch boards, and "let me just sketch out my week" — not for a 50-person engineering team.

---

## Tips for getting the most out of it

**Use it as a daily scratchboard.** Open it every morning, dump everything in your head into the To Do column, and work from it. Clear Done at end of day.

**Use priority chips to triage.** When the To Do column has 15 cards, the High-priority ones jump out. Be ruthless — most tasks are Medium or Low.

**Don't sleep on the description field.** A card titled "Fix the bug" is useless next week. A card titled "Fix the bug — when user clicks Save twice the row duplicates, see Slack thread" is a future-you gift.

**Make a card for the thing you're avoiding.** The reason it's not done isn't time — it's that it's not on a list. Putting it on the board is half the battle.

---

## What Flow is not

- A team collaboration tool (no shared boards, no comments, no @mentions)
- A project management suite (no Gantt, no dependencies, no time tracking)
- A note-taking app (Obsidian is better for that)
- Cross-device synced (see "Where your data lives" above)

If you need any of those, you probably want [Trello](https://trello.com), [Linear](https://linear.app), [Notion](https://notion.so), or [Jira](https://atlassian.com/software/jira) instead. They're great tools. Flow is the thing you reach for *before* those — when the overhead of setting them up costs more than the task is worth.

---

## Support

Flow is free and always will be. If it saves you time and you feel like buying the maker a coffee, the button in the bottom-right corner goes to [buymeacoffee.com/chenbuilds](https://buymeacoffee.com/chenbuilds). No pressure.

---

## For developers

Flow is one `index.html` file — HTML, CSS, and vanilla JavaScript, no build step, no framework, no dependencies. ~600 lines total.

To run it locally: clone this repo and open `index.html` in your browser. That's the full install.

To host your own copy: fork the repo, change the Buy Me a Coffee link in `index.html`, and enable GitHub Pages in the repo settings. Live in 60 seconds.

The code is intentionally simple — every interaction goes through a single `render()` function that rebuilds the DOM from a `tasks[]` array. Mutations call `save()` (writes to localStorage) and then `render()`. That's the whole architecture.

---

Built on Day 89 of a 100-day coding challenge.
