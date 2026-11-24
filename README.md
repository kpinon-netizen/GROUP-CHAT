# Huddle: a free group chat

One file (`index.html`), no server, no build. Works on GitHub Pages. Bright green theme.

**Features:** server codes, channels you can create, direct messages, a **group voice call** ("Lounge", up to 8 people), online/offline members, away / do-not-disturb, typing indicators, replies, emoji reactions, edit and delete your own messages, @mentions, pictures (paste or ＋), Markdown, unread badges, recent history for late joiners, **USC email verification badge**, mobile layout.

## Put it on GitHub Pages
1. Create a public repository on GitHub and upload `index.html` and `README.md`.
2. Settings → Pages → Deploy from a branch → `main` / `(root)` → Save.
3. After a minute it is live at `https://<username>.github.io/<repo>/`. Voice calls and microphones need `https`, which GitHub Pages provides.
4. Open it, pick a name and server code, click **Invite** and send the link.

## USC email verification (optional, free)
A static page cannot check an email by itself, so this uses **Sign in with Google**. Everyone's Google ID token is checked in the browser (signature against Google's public keys, issuer, audience, expiry, `email_verified`, `hd = usc.edu`, and a nonce tied to that person's session). Verified people get a cardinal **✌ USC** badge. In **USC-only mode** you only see messages, members and calls from verified `@usc.edu` people.

One-time setup (about 5 minutes, you do this once for the site):
1. Go to <https://console.cloud.google.com/>, create a project, then **APIs & Services → OAuth consent screen** (External, add your email) and publish it.
2. **Credentials → Create credentials → OAuth client ID → Web application**.
3. Under **Authorized JavaScript origins** add `https://<username>.github.io` (and `http://localhost` if you test locally).
4. Copy the client ID (ends in `.apps.googleusercontent.com`).
5. Either paste it under **Advanced** on the join screen, or put it in `GOOGLE_CLIENT_ID` near the bottom of `index.html` so everyone gets it automatically.

Honest limits:
- Messages travel over a public broker without accounts. The badge proves a *person signed in with a USC Google account*; individual messages are not cryptographically signed, so a determined person could still spoof a name. USC-only mode hides unverified people in the app, but it does not stop an outsider from reading the broker traffic. Do not share sensitive information.
- Tokens last about an hour. After that the badge disappears until you sign in again.
- Some @usc.edu addresses may not be Google-hosted; those cannot sign in this way.
- For real access control (invites, banning, private history) you would need a backend such as Firebase or Supabase with `@usc.edu` rules.

## Group voice call
Click **Lounge** under Voice channels, then **Join voice**. Audio goes directly between browsers (WebRTC, peer to peer) and is signalled over the same broker. Mute, deafen and a speaking ring are included. It is a mesh, so it is best for 8 people or fewer. Use headphones. Strict school or corporate networks can block peer-to-peer audio, because there is no TURN relay.

## How the chat works
Messages travel through a public MQTT broker (`wss://broker.emqx.io:8084/mqtt`) from the browser. Each channel keeps a short retained snapshot of the last 40 messages so someone joining later sees recent history (it can take a few seconds). Your own history is also saved in your browser. You can change the broker under **Advanced**.

## Other notes
- DMs only reach people who are online at that moment and are not end-to-end encrypted.
- Pictures are shrunk to about 640 px and are not kept in the shared history.
- Needs internet for the MQTT library (unpkg), the font (Google Fonts) and Google sign-in.
