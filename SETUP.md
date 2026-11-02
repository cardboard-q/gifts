# Gift Exchange — Firebase setup

One-time setup, about 15 minutes. Everything stays on Firebase's free **Spark** plan;
don't add a billing account and there's nothing that can ever charge you.

## 1. Create the project

1. Go to <https://console.firebase.google.com> and click **Create a project**.
2. Name it (e.g. `family-gifts`). Turn **Google Analytics off**. Click **Create project**.

## 2. Turn on sign-in

1. Left sidebar → **Build → Authentication** → **Get started**.
2. **Sign-in method** tab:
   - Click **Anonymous** → toggle **Enable** → **Save**.
   - Click **Add new provider** → **Google** → toggle **Enable**, pick your email as the support email → **Save**.
3. **Settings** tab → **Authorized domains** → **Add domain** → `cardboard-q.github.io`.
   (`localhost` is already there for local testing.)

## 3. Create the database

1. Left sidebar → **Build → Firestore Database** → **Create database**.
2. Pick a location close to you (e.g. `nam5 (United States)`), then choose
   **Start in production mode** → **Create**.
3. Open the **Rules** tab. Delete what's there and paste the contents of
   [`firestore.rules`](firestore.rules).
4. Replace `you@example.com` with the Google account you'll use as organizer.
5. Click **Publish**.

## 4. Connect the page

1. Click the gear ⚙️ next to **Project Overview** → **Project settings**.
2. Under **Your apps**, click the **`</>`** (Web) icon. Nickname it `gifts`,
   leave Firebase Hosting **unchecked**, click **Register app**.
3. Copy the values from the `firebaseConfig` it shows into
   [`firebase-config.js`](firebase-config.js). These values are safe to commit;
   the security rules are what protect the data.

## 5. (Recommended) Lock the key to your site

1. Go to <https://console.cloud.google.com/apis/credentials> and select your Firebase project at the top.
2. Click the key named **Browser key (auto created by Firebase)**.
3. Under **Application restrictions** choose **Websites** and add:
   - `https://cardboard-q.github.io/*`
   - `https://YOUR_PROJECT.firebaseapp.com/*` (needed for the Google sign-in popup)
   - `http://localhost:8000/*` (for local testing)
4. **Save**. It can take a few minutes to apply.

## 6. Try it locally

```sh
cd /path/to/spinspinner
python3 -m http.server 8000
```

Open <http://localhost:8000/gifts/> (opening the file directly won't work, because
the page loads `firebase-config.js` as a module).

## 7. Publish and use it

1. Commit and push. The page will be at <https://cardboard-q.github.io/spinspinner/gifts/>.
2. Click **Organizer sign-in** at the bottom, sign in with the email from step 3.
3. Create an exchange, copy its link, and send it to the family.
4. Once everyone has joined, click **Draw names**.
