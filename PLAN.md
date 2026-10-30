# My links: plan and status

This is the one status file for the My links standalone app (brief: `D:\projects\hq\research\my-links-standalone\HERK-PROMPT.md`, 29 Oct 2026).
Every stream reads this file before starting and writes its result under its own letter when done. Status lives nowhere else.

## A. Scaffold

Status: done (accepted 29 Oct 2026)

What changed: new repo, index.html copied from the board page with the save/load, local-file and agent-panel code removed; `store` stub on localStorage key `mylinks.v1`; .gitignore, MIT LICENSE, this file. Public repo michalszyszka2-cmyk/my-links, GitHub Pages from main.

Tested: grep for api/links, /files/, agent-embed, localhost:4420, links.json, notes.json returns nothing. https://michalszyszka2-cmyk.github.io/my-links/ returns 200. In the browser: pasted `Example https://example.com` + Enter → one row, message "Added 1 link to Ungrouped."; reload → row still there ("All links · 1"); theme button switches to dark; no console errors.

Open questions: none. Departures: a pasted Windows path now says "That doesn't look like a link." (was a /files/ link); four comments reworded; board.v1.* localStorage keys kept.

## B. Google Drive storage layer

Status: REPLACED 29 Oct 2026 (evening) by stream I below. Michal's call, 29 Oct: "Desktop only." The Drive layer, the client-ID placeholder, GOOGLE-SETUP.md and test/drive.test.html are gone from the repo (history has them at dc8d9c0).

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

Open questions: none. Screenshot retaken 29 Oct (evening) after stream I; it shows the "Keep it on this computer" sidebar.

## F. Portfolio entry, DRAFT ONLY

Status: drafted, awaiting approval

What changed: PORTFOLIO-DRAFT.md written: plain-text copy (What it is / What it does / Why I built it, three steps, note) and a paste-ready `<article>` card copied from the Snip card's structure. Nothing under D:\projects\portfolio\ edited.

Tested: read portfolio/index.html section#apps (Snip, Dictate, Bill Chaser) for structure and voice; checked no banned marketing words in the copy.

Open questions: no step strip exists in the portfolio to copy (steps are written in "How to use" as Snip does); screenshot assets/apps/my-links.webp does not exist and a still image would be a new pattern; "nothing stored anywhere else" is untrue for try-it mode (localStorage) if that ships. Details at the bottom of PORTFOLIO-DRAFT.md.

## G. GATE: Google Cloud project

Status: reopened 30 Oct 2026 for stream J. Blocked on Michal: GOOGLE-SETUP.md, then paste the Client ID.

## H. GATE: retiring the board copy

Status: blocked (Michal retires it himself after his import works)

What changed:

Tested:

Open questions:

## I. File on this computer (replaces B)

Status: built and tested 29 Oct 2026 (evening); the one real-dialog step is Michal's manual check below

What changed: the `store` in index.html writes one file on the person's own computer through the File System Access API (`showSaveFilePicker` / `showOpenFilePicker`, `createWritable`). `CAN_FILE` is true when `window.showSaveFilePicker` is a function (Chrome, Edge, Brave); otherwise the sidebar says "This browser can't save a file on your computer (Chrome, Edge and Brave can), so your links stay in this browser." and the page runs in try-it mode (Firefox, Safari). Sidebar in try-it mode: "Keep it on this computer" (save picker, suggested name `My links.json`, `.json` type) and "Open my file" (open picker, for a file made before, here or on another computer). The chosen handle goes into IndexedDB (`mylinks` db, store `h`, key `file`); localStorage keeps only the flag `mylinks.v1.file = '1'`. On a later visit `queryPermission({mode:'readwrite'})` is tried without a click; if the browser wants a click the sidebar shows "The browser needs your OK to use My links.json on this visit." with an Allow button (`requestPermission`), the kept list on screen and PENDING intact. A write refused by the browser (NotAllowedError) keeps the change under PENDING with "Kept in this browser; the file updates once you allow access to the file again." and brings Allow back. A missing file (NotFoundError) says "... once you pick the file again." and the sidebar offers Open my file / New file / Forget this file; a re-pick merges the kept links into the picked file (never writes over a different file). A new file is written once as `{"groups": []}` so it is never left blank; the JSON is pretty-printed. First pick from try-it mode with links worth keeping asks "Keep these links" / "Start empty" (or "Drop them" when the picked file already has links) in `#msg`, as the Drive layer did. "Forget this file" writes anything pending first (refuses if it can't), then drops the flag and the handle and returns to try-it mode; the file stays. Every Drive constant, the Google script and the client ID are gone. `window.fakeHandles`, when present, replaces the IndexedDB store (for the test). `window.mylinks` exposes `handle`, `granted`, `handles`, `choose`, `allow`, `forget` for the test.

Tested: `test/file.test.html` (replaces drive.test.html) loads index.html in an iframe with the two pickers, the file handles and the handle store stubbed: 80 checks, all pass at http://127.0.0.1:8812/test/file.test.html on 29 Oct 2026. Covers try-it samples + add + reload; new file via the save picker (name, type, `{"groups": []}` written, handle remembered, no question, sidebar "Kept on this computer as My links.json"); write round-trips, pretty-printed, PENDING cleared; reload reads with query only, no prompt; permission "prompt" on load shows Allow with the kept list, PENDING intact, Allow writes it; Allow refused says so; denied mid-session keeps the change, Allow writes it; an early add is merged, not overwriting; file gone shows the pick-again buttons, Forget refuses while unsaved, New file takes the kept links; Keep/Start empty both ways; Open my file reads a file with links without writing it; Open my file on non-JSON rolls back to try-it mode; cancelled picker does nothing; forget; no-picker browser note and try-it still works; no Google left in the page; import (board fixtures) into the file and into the browser; bookmarklet; the real IndexedDB store round-trips. By hand at 127.0.0.1:8812: sidebar text and both buttons present, no console messages, scrollWidth = innerWidth at 1024 and 375. README screenshot retaken (headless Chrome 1280x720, saved at 640x360).

Manual check for Michal (the native file dialog can't be driven from the Browser pane):
1. Open https://michalszyszka2-cmyk.github.io/my-links/ in Chrome or Edge: the three Examples show and the sidebar has "Keep it on this computer".
2. Click it, save `My links.json` somewhere (Desktop is fine): the sidebar says "Kept on this computer as My links.json" and the list is empty.
3. Paste a link and press Enter: "Added 1 link to Ungrouped." Open the file in Notepad: the link is inside.
4. Reload the page: either the link is there at once, or the sidebar asks for one click on Allow and then it is.
5. Optional: Import `D:\projects\board\pages\my-links\links.json` + `notes.json` (both in one pick) and check the file.

Open questions: (1) Chrome may remember the permission between visits ("Allow on every visit") or ask every time; only his real test shows which. (2) The bookmarklet opens the github.io page in a new tab; that tab is the same origin, so the same file and permission apply.

## J. Google Drive sync (added beside I)

Status: built and tested against a fake Drive 30 Oct 2026, not committed; dormant until a real Client ID is in `index.html`. Michal's ask, 30 Oct: "connect my links app to Google Drive so it can synchronize across my PC and phone." This reverses the Google part of "Desktop only"; the file route stays as it was. He chose no server over staying signed in with a small server.

What changed: third storage mode `drive` in index.html beside `local` and `file`. One file, `My links.json`, in the person's Drive, scope `drive.file`, found by name so a second device joins the same file. Sign-in is the full-page OAuth redirect (`response_type=token`), not the Google script and its pop-up: `prompt=none` with the remembered email as `login_hint` on a new visit needs no tap when Google already knows the person; if Google refuses, a Reconnect button appears, at most one silent trip per 5 minutes so it can't loop. The token is kept in localStorage until it expires (~1 hour); the `state` value is checked on the way back. BASE keeps the last agreed list, shown before a reconnect. `mergeLists(base, mine, theirs)` is a three-way merge by URL, run when the file's Drive `version` differs from the one last seen. On returning to the page (focus, visibilitychange) it pulls changes, unless something is selected, dragged or being edited. Writes go through one queue. "Move to Google Drive" carries a file-mode list across. A trashed or deleted file is remade from BASE. A file broken by hand is never overwritten.

Tested: `test/drive.test.html`, 56 passed, 0 failed, fake Drive and fake Google, two devices simulated by swapping storage. `test/file.test.html` 55 passed, 0 failed (the "no Google in the page" check was replaced). Found and fixed by the suite: opening offline then adding a link wrongly dropped the sign-in. No sideways scroll at 375.

Not tested: real Google. Nothing has touched a real client ID, real Drive or a real phone. Open questions: (1) whether the silent trip works on his phone, and from a home-screen icon; (2) Drive has no write lock in this design, so two writes within the same half-second can still lose one; (3) a link edited on both devices keeps the version of whichever wrote last; (4) the live portfolio card and PORTFOLIO-DRAFT.md still describe the computer file only; (5) the privacy paragraph in GOOGLE-SETUP.md step 7 is not written.

