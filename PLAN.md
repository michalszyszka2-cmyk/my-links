# My links: plan and status

This is the one status file for the My links standalone app (brief: `D:\projects\hq\research\my-links-standalone\HERK-PROMPT.md`, 29 Oct 2026).
Every stream reads this file before starting and writes its result under its own letter when done. Status lives nowhere else.

## A. Scaffold

Status: done (accepted 29 Oct 2026)

What changed: new repo, index.html copied from the board page with the save/load, local-file and agent-panel code removed; `store` stub on localStorage key `mylinks.v1`; .gitignore, MIT LICENSE, this file. Public repo michalszyszka2-cmyk/my-links, GitHub Pages from main.

Tested: grep for api/links, /files/, agent-embed, localhost:4420, links.json, notes.json returns nothing. https://michalszyszka2-cmyk.github.io/my-links/ returns 200. In the browser: pasted `Example https://example.com` + Enter → one row, message "Added 1 link to Ungrouped."; reload → row still there ("All links · 1"); theme button switches to dark; no console errors.

Open questions: none. Departures: a pasted Windows path now says "That doesn't look like a link." (was a /files/ link); four comments reworded; board.v1.* localStorage keys kept.

## B. Google Drive storage layer

Status: built, awaiting Michal's sign-in test (29 Oct 2026)

What changed: the `store` stub in index.html is now a Google Drive layer, browser only, no server. `CLIENT_ID` is one constant at the top of the script (placeholder `PASTE-YOUR-CLIENT-ID.apps.googleusercontent.com`); while it is the placeholder the sidebar button reads "Google not connected yet" (disabled) and the page runs in try-it mode. Try-it mode: the list lives in localStorage `mylinks.v1` and starts with three sample links in one folder "Examples" (example.com, Wikipedia's Bookmark page, MDN); the samples are not written to storage until something changes. Sign-in: Google Identity Services token client, scope `drive.file` only, loaded only when a client ID is in. On sign-in the page looks for the file by `appProperties` marker (`app=my-links`, `files.list` with `q`), creates `My links.json` in the Drive root (multipart upload, visible to them) when there is none, reads it, and if the browser held try-it links that are not the untouched samples asks "Keep these links" / "Start empty" in `#msg` (no window.confirm). Every change PATCHes the whole JSON (`upload/drive/v3/files/{id}?uploadType=media`) through the existing `push()`/`save()`/`sync()` and PENDING retry. localStorage remembers only `mylinks.v1.signedin` and `mylinks.v1.file`, never the token; on load with a remembered sign-in it asks for a token with `prompt: ''` and, if that fails (popup blocked, closed, timed out after 8 s), shows the sign-in button with whatever is kept in this browser on screen and PENDING intact. A 401 or a timed-out token drops the token, shows the sign-in button, and the change is kept under PENDING until they sign in again. A link added while signed out, before the file was read, is kept as a partial list and merged into the file on sign-in (never overwrites it). "Sign out" first writes anything pending (refuses to sign out if that fails), then forgets the two keys and returns to try-it mode; the Drive file stays and the grant is not revoked. If the remembered file is gone from Drive (404) the id is forgotten and the file is found by marker or created again. The sidebar block `#account` sits under the folders in the existing button/muted-text styles, no new fonts. The stub's error wording is now "this browser has no room to store it".

Tested: `test/drive.test.html` loads index.html in an iframe with `fetch` and the Google token client stubbed (fake Drive in memory; the test saves and restores this browser's own keys). 42 checks, all pass on 29 Oct 2026 at http://127.0.0.1:8798/test/drive.test.html: try-it samples + add + reload; first sign-in creates one file named `My links.json` with `appProperties.app=my-links`, no parents, after a `files.list` by marker; signed-in + file id remembered, token never in localStorage; write PATCHes the whole JSON and clears PENDING; reload reads the file back with a silent (`prompt: ''`) token; with the file id forgotten the file is found by marker and not created again; 401 shows "Kept in this browser; the file updates once you sign in again.", keeps the list and PENDING, shows the sign-in button, then sign-in writes the kept change; silent refresh failure on load shows the sign-in button with the kept list on screen; a link added while signed out is merged, not overwriting; try-it data brings the Keep / Start empty question and both answers do what they say; sign out forgets the keys, brings the samples back, leaves the file, no revoke; placeholder ID shows "Google not connected yet" and loads no Google script; wording fix present. Live page at 127.0.0.1:8798: samples show, `Quietside quietside.co.uk` adds a row, reload keeps it, no console messages, scrollWidth = innerWidth at 1280 and 390, light and dark (screenshots at half scale). The grep for api/links, /files/, agent-embed, localhost:4420, notes.json returns nothing; `links.json` matches only the Drive file's own name `My links.json`.

Manual check for Michal (after gate G gives a client ID):
1. Paste the client ID into `const CLIENT_ID = '...'` at the top of the script in index.html, commit, push, wait a minute for Pages.
2. Open https://michalszyszka2-cmyk.github.io/my-links/ in a private window: the three Examples show and the sidebar button reads "Sign in with Google".
3. Click it, pick the account, allow "See, edit, create and delete only the specific Google Drive files that you use with this app": the sidebar says "Signed in" and the list is empty.
4. Paste a link and press Enter: the message reads "Added 1 link to Ungrouped." and no error.
5. Open drive.google.com: `My links.json` is in My Drive; open it and the link is inside. Reload the page: the link is still there (if the sign-in window is blocked on reload, click "Sign in with Google" once).

Open questions: (1) whether Google lets the silent `prompt: ''` token request open on page load without a click; browsers block popups with no gesture, so Michal may see the sign-in button on every fresh load (one click, nothing lost). Only his real test can settle it. (2) The two-button question says "Drop them" instead of "Start empty" when the Drive file already has links, because "Start empty" would read as wiping the Drive list.

## C. Import from the board copy

Status: pending

What changed:

Tested:

Open questions:

## D. Bookmarklet

Status: pending

What changed:

Tested:

Open questions:

## E. README

Status: pending

What changed:

Tested:

Open questions:

## F. Portfolio entry, DRAFT ONLY

Status: drafted, awaiting approval

What changed: PORTFOLIO-DRAFT.md written: plain-text copy (What it is / What it does / Why I built it, three steps, note) and a paste-ready `<article>` card copied from the Snip card's structure. Nothing under D:\projects\portfolio\ edited.

Tested: read portfolio/index.html section#apps (Snip, Dictate, Bill Chaser) for structure and voice; checked no banned marketing words in the copy.

Open questions: no step strip exists in the portfolio to copy (steps are written in "How to use" as Snip does); screenshot assets/apps/my-links.webp does not exist and a still image would be a new pattern; "nothing stored anywhere else" is untrue for try-it mode (localStorage) if that ships. Details at the bottom of PORTFOLIO-DRAFT.md.

## G. GATE: Google Cloud project

Status: blocked (only Michal can do this in the Google console)

What changed:

Tested:

Open questions:

## H. GATE: retiring the board copy

Status: blocked (Michal retires it himself after his import works)

What changed:

Tested:

Open questions:
