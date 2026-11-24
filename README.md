# Huddle: a group chat for USC

One page (`index.html`), bright green theme. Works on any laptop or phone through a web link (GitHub Pages).

**Features:** channels you can create, direct messages, a **group voice call** ("Lounge", up to 8 people), online/offline members, typing indicators, replies, emoji reactions, edit and delete your own messages, @mentions, pictures, Markdown, unread badges, mobile layout.

## Two modes

| | Cloud mode (recommended) | Free mode (default) |
|---|---|---|
| Setup | Free Firebase project, 10 minutes, once | None |
| Who can join | Only people who open the **verification link sent to their @usc.edu email** | Anyone with the server code (optional Google sign-in badge) |
| Messages | **Stored in the cloud**; every laptop sees the full history, even after everyone leaves | Short history on a public broker, plus each browser's own copy |
| Safety | Only verified USC accounts can read or write (enforced by Firebase rules) | Not private |

## Cloud mode setup (Firebase, free Spark plan)
1. Go to <https://console.firebase.google.com/>, **Add project** (turn Google Analytics off).
2. **Build → Authentication → Get started → Sign-in method → Email/Password → enable "Email link (passwordless sign-in)"** and save.
3. **Authentication → Settings → Authorized domains → Add domain**: `kpinon-netizen.github.io`.
4. **Build → Firestore Database → Create database** (production mode, any US region).
5. Firestore **Rules** tab: paste the contents of `firestore.rules` from this repo and **Publish**.
6. **Project settings (gear) → General → Your apps → Web app (`</>`)**, register an app, and copy the `firebaseConfig` object.
7. Open the chat, expand **Advanced**, paste the config into **Firebase config**. The page reloads in cloud mode. (To make it permanent for everyone, paste it into `FIREBASE_CONFIG` near the bottom of `index.html` and push.)

Then each person: opens the site, types their `@usc.edu` address, clicks **Send link**, opens the email on that same device (check spam), and returns to the chat. They stay signed in on that laptop.

How it is enforced: sign-in uses Firebase email links (the person must be able to open mail sent to that address). The page only accepts `@usc.edu` addresses, and the Firestore rules refuse reads and writes from anyone else, so even a modified page cannot get in.

Limits: the email comes from Firebase's default sender and can land in spam or be delayed by university filters. Typing, who is online and the voice call still run on the shared public broker (no accounts there), so those parts are not private. Direct messages are ephemeral, are not stored in the cloud and are not end-to-end encrypted. The free plan allows roughly 50,000 reads and 20,000 writes per day, which is plenty for a group of friends.

## Free mode (no setup)
Pick a name and a server code and share the invite link. Optional **Sign in with Google** adds a "✌ USC" badge (see the Google client ID field under Advanced). This is not private: anyone who guesses the code can read the chat.

## Put it on GitHub Pages
Repository **Settings → Pages → Deploy from a branch → `main` / `(root)`**. Live at `https://<username>.github.io/<repo>/`. Voice calls and microphones need `https`, which GitHub Pages provides.

## Voice call
Open **Lounge** and click **Join voice**. Audio goes directly between browsers (WebRTC), signalled over the broker. Use headphones. Strict networks can block peer-to-peer audio because there is no TURN relay.
