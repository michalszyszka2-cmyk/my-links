# Google sign-in setup (for Michal, one-off, ~20 minutes, £0)

The app signs people in with Google and writes one file to their own Drive. For that Google
needs an "OAuth client" that belongs to you. Only you can create it. Nothing here costs money.

## 1. New project

1. Open https://console.cloud.google.com/ and sign in with the Google account you want to own
   this (the michaltech one, not the inbox-dashboard one; the brief says a new project).
2. Top bar → project picker → **New project**. Name: `my-links`. Create, then select it.

## 2. Turn on the Drive API

1. Left menu → **APIs & Services → Library**. Search "Google Drive API". **Enable**.

## 3. Consent screen (Google Auth Platform)

Do these in THIS order. Last time (inbox dashboard, 8 Sept) the Publish button stayed grey
until the scope was added first and then three un-starred Branding fields were filled.

1. Left menu → **Google Auth Platform → Get started** (or "OAuth consent screen" in the old UI).
   App name: `My links`. User support email: your address. Audience: **External**.
   Contact email: your address. Finish.
2. **Data access** (was "Scopes") → **Add or remove scopes** → tick
   `https://www.googleapis.com/auth/drive.file`
   ("See, edit, create and delete only the specific Google Drive files that you use with
   this app"). It is a *non-sensitive* scope, so Google does not require a security review.
   Save.
3. **Branding** → fill all three, even though they carry no asterisk:
   - Application home page: `https://michalszyszka2-cmyk.github.io/my-links/`
   - Application privacy policy link: `https://michaltech.co.uk/privacy.html`
   - Authorised domains: add `michalszyszka2-cmyk.github.io` **and** `michaltech.co.uk`
     (no https://, no www).
   Save.
4. **Audience** → **Publish app** → confirm. Status should read **In production**.

### If Google refuses `michalszyszka2-cmyk.github.io` as an authorised domain

It may say the domain must be verified. Then either:
- verify it in Search Console (https://search.google.com/search-console, add property
  `https://michalszyszka2-cmyk.github.io/`, "HTML tag" method; tell me the tag and I add it to
  the app's `<head>`), or
- set the home page to `https://michaltech.co.uk` and keep only `michaltech.co.uk` as the
  authorised domain. Sign-in still works from the GitHub Pages address; the "authorised
  JavaScript origin" in step 4 is what controls that, not this field.

Tell me which one you did.

### While the app is unpublished ("Testing")

Only test users you list under Audience can sign in (up to 100), and tokens die after 7 days.
Publishing removes both limits. Because the scope is non-sensitive there is no "unverified
app" warning screen once published.

## 4. The OAuth client (the ID the app needs)

1. **Clients** (or APIs & Services → Credentials → Create credentials → OAuth client ID).
2. Type: **Web application**. Name: `my-links web`.
3. **Authorised JavaScript origins** → add exactly:
   `https://michalszyszka2-cmyk.github.io`
   (origin only, no path, no trailing slash).
4. Authorised redirect URIs: leave empty (the app uses the pop-up token flow, no redirect).
5. Create. Copy the **Client ID** (ends in `.apps.googleusercontent.com`). The client secret
   is not needed and must never go into the page.

## 5. Put the ID in the app

Paste the Client ID in the chat. It is public by design (it ships in the page), so pasting
it is fine. I replace the placeholder constant at the top of `index.html`, commit and push.

## 6. Your 5-line test

1. Open https://michalszyszka2-cmyk.github.io/my-links/ in a private window.
2. Click **Sign in with Google**, pick your account, allow "see, edit, create and delete only
   the specific Google Drive files that you use with this app".
3. Paste a link and press Enter. The message should say it was saved.
4. Open https://drive.google.com/ — a file **My links.json** is in My Drive. Open it: your
   link is inside.
5. Close the window, open the page again: the link is still there, no second sign-in prompt.

## 7. Privacy page

`https://michaltech.co.uk/privacy.html` should gain one short paragraph saying what My links
stores (one JSON file in the user's own Drive, nothing on any server, no analytics) before you
share the app widely. I draft it on request; it is not written yet.
