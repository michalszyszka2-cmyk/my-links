# My links

![My links: folders on the left, saved links on the right](assets/screenshot.png)

A plain bookmark page that keeps your links in your own Google Drive. Live at https://michalszyszka2-cmyk.github.io/my-links/

Folders sit on the left and links sit in rows on the right, like an inbox. Each link shows an icon, a name, a grey one-line description and the day it was added.

## How to use

1. Open the page. Before you sign in it runs in try-it mode and keeps links in this browser only.
2. Sign in with Google once to keep your list in your Drive.
3. Paste a link into the box at the top and press Enter.
4. Drag a link onto a folder to move it, or tick several to move or delete them together.
5. Filter by date, sort newest or oldest first, and switch between light and dark with the button top right.

At the bottom of the sidebar there is a "Save this page" link. Drag it to your bookmarks bar and click it on any page to save that page here. The Import button next to it reads a links.json from the board version.

## Where your links live

Your list is one file, `My links.json`, in your own Google Drive. The page finds it again by a hidden marker on the file. Nothing is stored on any server.

The page asks for Google's `drive.file` permission, which only lets it see files it created itself. It cannot read the rest of your Drive.

"Sign out" makes the browser forget you. The file in your Drive stays where it is.

## Run your own copy

There is no build step and there are no dependencies. The whole thing is one HTML file.

1. Fork this repository.
2. In the fork's settings, turn on GitHub Pages from the `main` branch.
3. In Google Cloud, create your own OAuth web client and add your Pages address (for example `https://yourname.github.io`) as an authorised JavaScript origin.
4. Paste its client ID into the `CLIENT_ID` constant near the top of `index.html`.

[GOOGLE-SETUP.md](GOOGLE-SETUP.md) walks through the Google side step by step.

## Licence

MIT. See [LICENSE](LICENSE).

Built by Michal Szyszka, [michaltech.co.uk](https://michaltech.co.uk).
