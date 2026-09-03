# Tasks — a standalone to-do app

The exact task feature from Branchline (subtasks, priority stars, focus
timer, progress bar), promoted out of the mind map into its own
page — plus Google Sign-In to sync your list to your own Google Drive.
No server, no account system of its own — everything runs in your
browser, and Google Drive is the only thing that ever sends your data
anywhere.

## How to run it

1. Unzip this folder anywhere on your computer.
2. Double-click `index.html` (or right-click → Open with → your browser).
3. That's it — works fully offline. Your tasks are saved to this
   browser's local storage automatically as you type.

## Google Sign-In setup (optional — syncs your tasks across devices)

By default, each device/browser only has its own copy of your tasks.
The Client ID already in `app.js` is Branchline's own — Google
authorizes by **origin** (scheme + host, not the full path), so if
you're hosting this on the **same GitHub Pages site** as Branchline
(e.g. `https://yourusername.github.io/...`), "Sign in with Google"
should already work with no extra setup.

If you're hosting this somewhere else (a different domain, or GitHub
Pages under a different username), you'll need your own Client ID:

1. Go to https://console.cloud.google.com/ → create a project (or pick
   an existing one).
2. **APIs & Services → Library** → search "Google Drive API" → Enable it.
3. **APIs & Services → OAuth consent screen** → set it up (choose
   "External" unless this is for a Google Workspace org; for personal
   use you can leave it in "Testing" mode and add your own Google
   account as a test user).
4. **APIs & Services → Credentials → Create Credentials → OAuth client ID**
   → Application type: **Web application**.
5. Under **Authorized JavaScript origins**, add every origin you'll open
   the app from, e.g. `https://yourusername.github.io`, plus
   `http://localhost` and `http://127.0.0.1` for local testing.
6. Save, then copy the **Client ID** (ends in `.apps.googleusercontent.com`).
7. Open `app.js`, find the line near the top that says:
   ```js
   const GOOGLE_CLIENT_ID = "270018625814-4jfdor9fci625de9b4j7hjta15urcqoe.apps.googleusercontent.com";
   ```
   and paste your own Client ID in there instead.
8. Re-upload `app.js` (bump its `?v=` number in `index.html` so browsers
   fetch the new copy) and reload the app.

The app only requests Drive's restricted `drive.file` scope, meaning it
can only see the one `tasks.json` file it creates itself — never any of
your other Drive files.

> Signing in lasts about an hour before the app quietly asks Google for
> a fresh token in the background; if that ever fails silently, just
> click "Sign in with Google" again. If two devices edit while offline
> and both later sync, the copy with the more recent edit wins — there's
> no merge.

## Features

- **Add / check off / delete tasks** — type into the box at the top and
  press `Enter` to add. Double-click a task's text to rename it.
- **Priority stars (0–3)** — click a star to set a task's priority;
  click the same star again to clear it. Stars don't reorder the list
  on their own, but weight how much a task's completion moves the
  overall progress bar (0 stars = 1×, 1 star = 2×, 2 stars = 3×, 3
  stars = 4×). "★ Sort" reorders the whole list by star count, highest
  first.
- **Subtasks** — click "▸+" on a task to add a small checklist under it.
  Checking every subtask marks the parent task done automatically, and
  checking/unchecking the parent cascades to all its subtasks. A small
  progress bar under the task shows subtask completion. Drag the ⠿
  handle to reorder subtasks — within the same task or onto a different
  one entirely. Double-click a subtask's text to rename it, ⧉ to copy
  its text, × to delete it.
- **Drag to reorder tasks** — drag the ⠿ handle on the left of any task.
- **Overall progress bar** — at the top, weighted the same way as the
  star priorities above.
- **Focus timer** — click the ⏱ on any task to start a 1-minute timer
  for it, shown in a small widget under the progress bar. "+1m" stacks
  on more time, ⏸ pauses, ■ stops. It keeps running in the background
  even while you edit other tasks, chimes when time's up, and counts
  down right in the browser tab's title (e.g. "⏱ 0:42 · Water the
  plants") so you can see it without switching back to the tab.
- **Autosave** — every edit is saved to this browser's local storage
  immediately; signed-in edits also sync to Drive a moment later.

## Notes

- Without signing in, your tasks live only in this browser's local
  storage for this page — if you clear your browser's site data, or
  open the app from a different URL/origin, they won't carry over.
  Sign in with Google if that matters to you.
- If you host this on GitHub Pages and re-upload `app.js` or
  `style.css`, bump the `?v=1` number in `index.html` to `?v=2` (etc.)
  each time, or people who already have the page open/cached may keep
  seeing the old version — browsers cache `.js`/`.css` files by their
  exact URL.
