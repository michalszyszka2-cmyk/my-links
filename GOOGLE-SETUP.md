# Google Drive setup (for Michal, one-off, about 20 minutes, £0)

Google needs to know the app exists before it lets anyone connect a Drive. That record is an
"OAuth client". Only you can create it, because it lives in your Google account.
Nothing here costs money. The Drive button stays hidden until the ID is in the app.

## 1. New project

1. Open https://console.cloud.google.com/ and sign in with the Google account you want to own
   this. Your links go in whichever Drive you connect later, so this account only owns the setup.
2. Top bar → project picker → **New project**. Name: `my-links`. Create, then select it.

## 2. Turn on the Drive API

1. Left menu → **APIs & Services → Library**. Search "Google Drive API". **Enable**.

## 3. Consent screen (Google Auth Platform)

Do these in THIS order. On the inbox dashboard the Publish button stayed grey until the scope
was added first and then three un-starred Branding fields were filled.

1. Left menu → **Google Auth Platform → Get started**.
   App name: `My links`. User support email: your address. Audience: **External**.
   Contact email: your address. Finish.
2. **Data access** → **Add or remove scopes** → tick
   `https://www.googleapis.com/auth/drive.file`
   ("See, edit, create and delete only the specific Google Drive files that you use with
   this app"). Save.
3. **Branding** → fill all three, even though they carry no asterisk:
   - Application home page: `https://michalszyszka2-cmyk.github.io/my-links/`
   - Application privacy policy link: `https://michaltech.co.uk/privacy.html`
   - Authorised domains: add `michalszyszka2-cmyk.github.io` **and** `michaltech.co.uk`
     (no https://, no www).
   Save.
4. **Audience** → **Publish app** → confirm. Status should read **In production**.

### If Google refuses `michalszyszka2-cmyk.github.io` as an authorised domain

It may say the domain must be verified. Verify it in Search Console
(https://search.google.com/search-console, add property
`https://michalszyszka2-cmyk.github.io/`, "HTML tag" method). Tell me the tag and I add it to
the app's `<head>`.

### While the app is unpublished ("Testing")

Only test users you list under Audience can connect, and Google asks you to approve again
every 7 days. Publishing removes both limits.

## 4. The OAuth client (the ID the app needs)

1. **Clients** → **Create client**. Type: **Web application**. Name: `my-links web`.
2. **Authorised JavaScript origins** → add exactly:
   `https://michalszyszka2-cmyk.github.io`
3. **Authorised redirect URIs** → add exactly, with the slash at the end:
   `https://michalszyszka2-cmyk.github.io/my-links/`
4. Create. Copy the **Client ID** (ends in `.apps.googleusercontent.com`). The client secret is
   not needed and must never go in the page.

## 5. Put the ID in the app

Paste the Client ID in the chat. It is public by design, because it ships in the page. I put it
in `index.html`, commit and push.

## 6. Your test

On the computer, at https://michalszyszka2-cmyk.github.io/my-links/:

1. Click **Move to Google Drive** (or **Connect Google Drive**), pick your account, allow access.
2. If it asks about links already on this device, click **Keep these links**.
3. Open https://drive.google.com/ . A file **My links.json** is in My Drive with your links in it.

On the phone, in its normal browser (Chrome or Safari, not a home-screen icon yet):

4. Open the same address, scroll to the bottom, click **Connect Google Drive**, pick the same
   account. Your links appear.
5. Add a link on the phone. Go to the computer's tab: it shows up within a moment of you
   clicking back on the page.
6. An hour or more later, open the page on the phone. It should jump to Google and back by
   itself. Tell me whether it did, or whether it asked you to tap Reconnect.

## 7. Privacy page

`https://michaltech.co.uk/privacy.html` should gain one short paragraph saying what My links
stores (one JSON file in the person's own Drive, nothing on any server, no analytics) before the
Drive button is shared widely. I draft it on request; it is not written yet.
