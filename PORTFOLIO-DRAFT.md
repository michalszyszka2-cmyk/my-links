# My links: portfolio card

**DRAFT for Michal's approval, not applied.** Nothing under `D:\projects\portfolio\` has been edited. Written 29 Oct 2026 (stream F).

## Copy

### What it is

A plain bookmark page. Folders sit on the left and your links run down the right as rows, like an inbox. Each link has an icon, a name, a grey one-line description and the day you added it.

### What it does

Paste a link and press Enter. Drag links between folders, or tick several and move or delete them together. Filter by date, put the newest or the oldest first, and switch between light and dark. Sign in with Google once and the whole list is kept as one small file, My links.json, in your own Google Drive. Nothing is stored anywhere else, and there is no account to make.

### Why I built it

I kept my own research pages and tools in a list inside my planning board. I wanted the same thing for the people I work with, without running a server or holding anyone's data. Pocket closed in 2025, and the free alternatives either need a server or an account.

### How to use (the three steps)

1. Open the page and sign in with Google.
2. Paste a link and press Enter.
3. It is in your Drive as My links.json.

To save the page you are looking at, drag "Save this page" to your bookmarks bar and click it on any page.

### Note under the buttons

Nothing to install. It runs in your browser, and Google only lets it see the one file it makes in your Drive.

## HTML, ready to paste

Goes after the Bill Chaser `</article>` inside `<div class="apps">`. Also change `<p class="part__count">3 apps</p>` to `4 apps`.

Two things are open (see "Open questions" at the bottom): the Snip card has no step strip to copy, so the steps sit in "How to use" as Snip's do; and the screenshot `assets/apps/my-links.webp` does not exist yet, and no card so far shows a still image instead of a demo video.

```html
        <article class="card project reveal">
          <header class="project__head">
            <h3 class="cmp__name">My links</h3>
            <p class="cmp__meta"><span class="cmp__domain">Web app</span></p>
          </header>
          <div class="project__text">
            <dl class="lines">
              <div><dt>What it is</dt><dd>A plain bookmark page. Folders sit on the left and your links run down the right as rows, like an inbox. Each link has an icon, a name, a grey one-line description and the day you added it.</dd></div>
              <div><dt>What it does</dt><dd>Paste a link and press Enter. Drag links between folders, or tick several and move or delete them together. Filter by date, put the newest or the oldest first, and switch between light and dark. Sign in with Google once and the whole list is kept as one small file, My links.json, in your own Google Drive. Nothing is stored anywhere else, and there is no account to make.</dd></div>
              <div><dt>Why I built it</dt><dd>I kept my own research pages and tools in a list inside my planning board. I wanted the same thing for the people I work with, without running a server or holding anyone's data. Pocket closed in 2025, and the free alternatives either need a server or an account.</dd></div>
            </dl>
            <details class="more">
              <summary>More details</summary>
              <div class="app__body">
                <div class="app__col">
                  <h4 class="app__label">What it's about</h4>
                  <p>I kept my own research pages and tools in a list inside my planning board, and wanted the same thing for the people I work with. I did not want to run a server or hold anyone's data. So the page is only the front end: your list is one small file, My links.json, in your own Google Drive. Nothing is stored anywhere else, and there is no account to make.</p>
                </div>
                <div class="app__col">
                  <h4 class="app__label">How to use</h4>
                  <p>Open the page and sign in with Google. Paste a link and press <kbd>Enter</kbd>. It is in your Drive as My links.json.</p>
                  <p>To save the page you are looking at, drag <strong>Save this page</strong> to your bookmarks bar and click it on any page.</p>
                </div>
              </div>
            </details>
            <div class="app__foot">
              <p class="app__links">
                <a class="btn" href="https://michalszyszka2-cmyk.github.io/my-links/" target="_blank" rel="noopener">Open My links</a>
                <a class="link" href="https://github.com/michalszyszka2-cmyk/my-links" target="_blank" rel="noopener">Source ↗</a>
              </p>
              <p class="app__note">Nothing to install. It runs in your browser, and Google only lets it see the one file it makes in your Drive.</p>
            </div>
          </div>
          <div class="project__media">
            <!-- PLACEHOLDER: assets/apps/my-links.webp does not exist yet.
                 Every other card shows a demo video here; a still image is a new pattern
                 and needs Michal's call (still, video, or nothing). -->
            <img src="assets/apps/my-links.webp" alt="My links: folders on the left, links as rows on the right" width="960" height="540" loading="lazy">
          </div>
        </article>
```

## Open questions

1. **No step strip exists to copy.** The brief asked for the 3-step `<ol class="steps">` from the Snip card. There is no `class="steps"` anywhere in `portfolio/index.html` and no `.steps` rule in its CSS. Snip's steps are a sentence in "How to use", so the three steps are written the same way here. A visible strip would be a new component and is his design call.
2. **Screenshot.** `assets/apps/my-links.webp` does not exist yet. Snip, Dictate and Bill Chaser all show a demo video in `.project__media`; a plain still image is a new pattern. Still, video, or leave the media column empty: his call.
3. **Block length.** The brief says three to five sentences per block, so the three top lines are 3, 5 and 3 sentences. Snip's and Dictate's top lines are one sentence each; Bill Chaser's are two to three. If he wants it to match Snip, the longer text can move into "What it's about".
4. **"Nothing is stored anywhere else."** Plan A says try-it mode keeps links in the browser (localStorage) before sign-in. If try-it mode ships, the sentence is true only for signed-in use.
5. **The note's claim** that Google only lets the app see its own file comes from the `drive.file` scope in brief section G; it holds only if the OAuth client is set up with that scope alone.
