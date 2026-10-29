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

Status: accepted 29 Oct 2026 (orchestrator reran the suite live: 67 passed; fixtures replaced with made-up data of the same shape)

What changed: index.html has a `#tools` block under `#account` in the sidebar with an "Import" button (existing button style) and a hidden file input (`.json`, multiple). Pick the board's `links.json` and, optionally, `notes.json` (each file is told apart by its shape, not its name). `importGroups()` accepts `[{name, links}]`, the older `{links: [...]}` and a bare list of links; a notes.json note goes onto a link only when the link has no `note` of its own (as `noteOf()` did); a URL starting `/pages/` or `/files/` becomes `http://localhost:4420` + that path (after the note lookup, so notes keyed by the relative URL still match). Then the existing `merge()` adds folders by name and skips URLs already present, and `save()` writes it the normal way (Drive file when signed in, this browser when not). Message: "Imported N links into M folders, skipped K already here." When nothing is new it only says so and writes nothing. Bug fixed in `merge()` (from B): two links with the same URL in one folder were both added; now the second is skipped. `test/fixtures/links.json` and `notes.json` are copies of the board files as of 29 Oct 2026.

Tested: section 11 of `test/drive.test.html` feeds the real fixture files through the real file input (change event) at http://localhost:8811/test/drive.test.html; whole suite 67 passed, 0 failed. The fixture holds 20 links (not 21) in 7 groups, 6 of them with links (the unfiled group is empty). Two of them are the same page once rewritten (`/files/hq/research/yt-BQJRLZ2q_Ns/index.html#sC` and `http://localhost:4420/files/hq/research/yt-BQJRLZ2q_Ns/index.html#sC`, both in "Selling AI: research"), so the result is "Imported 19 links into 6 folders, skipped 1 already here." Checked: 19 links in the fake Drive file and on screen; the 6 folder names in board order; zero URLs starting with `/`; every board URL present at its localhost:4420 form; notes.json attached to all 18 links with no note of their own; the first yt link keeps its own note over notes.json's; titles, icons and dates kept; written by PATCH, PENDING cleared; a second import says "Imported 0 links into 0 folders, skipped 20 already here." and writes nothing; signed out it lands in browser storage; links.json alone works; the flat `{links}` shape goes into Ungrouped; notes.json alone asks for links.json.

Open questions: (1) The duplicate yt link: the kept one is titled "yt-BQJRLZ2q_Ns/index.html" with note "Which of Nate Herk's four systems to sell first"; the dropped one had "Nate Herk's 4 Claude systems, every detail". Michal may want to rename the kept one after import. (2) The brief's "21 links" is 20 in the file on 29 Oct.

## D. Bookmarklet

Status: accepted 29 Oct 2026 (orchestrator reran the suite live: 67 passed; fixtures replaced with made-up data of the same shape)

What changed: in the same `#tools` block, below Import: a "Save this page" link (button look, existing tokens) whose href is `javascript:(()=>{open('https://michalszyszka2-cmyk.github.io/my-links/?add='+encodeURIComponent(location.href)+'&title='+encodeURIComponent(document.title),'_blank')})()`, and the line "Drag this to your bookmarks bar. Click it on any page to save that page here." On load, `fromBookmarklet()` reads `?add=` and `&title=`, puts `title + ' ' + url` in `#paste` (just the URL when there is no title or the title is the URL), removes both parameters with `history.replaceState` (keeps any others and the hash) and focuses the box. Nothing is added until Enter, which runs the existing `add()`/`parse()`.

Tested: section 12 of `test/drive.test.html` (in the 67 passes): the href and the instruction line; an iframe on `../index.html?add=https%3A%2F%2Fexample.org&title=Example%20Org` has "Example Org https://example.org" in the box, the box focused, `location.search` empty, still 3 rows; Enter adds "Example Org" ("Added 1 link to Ungrouped.") and it is in browser storage. By hand in the Browser pane at http://localhost:8811/?add=https%3A%2F%2Fexample.org&title=Example%20Org: address bar became http://localhost:8811/, box pre-filled and focused, a real Enter key press added the row, no console messages. At 390 wide scrollWidth = innerWidth = 390 in light and dark; the bookmarklet link takes the panel/ink tokens in both themes (screenshot at half scale, light).

Open questions: (1) The bookmarklet always opens the github.io address, also on a local copy or a fork; a self-hoster would need to change that one URL (README stream may want to say so). (2) Dragging it to a real bookmarks bar and clicking it on another site was not tested here (needs a person's browser); the test covers what happens when that URL opens.

## E. README

Status: done

What changed: wrote README.md (screenshot, pitch, How to use, Where your links live, Run your own copy, Licence; 41 lines). Added assets/screenshot.png (headless Chromium, file:// copy, light theme, 1280x720 at 0.5 scale, three example links pasted).

Tested: screenshot viewed and shows folders plus link rows; README links point at GOOGLE-SETUP.md and LICENSE, both present.

Open questions: screenshot shows the 'Google not connected yet' sidebar note because CLIENT_ID is still the placeholder; retake after G if wanted.

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
