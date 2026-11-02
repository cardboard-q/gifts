# Gift Exchange — Firebase setup

One-time setup, about 10 minutes. Everything stays on Firebase's free **Spark** plan;
don't add a billing account and there's nothing that can ever charge you.

Nobody needs an account or password. Access works through links:

- **Family link** (`?g=…`): see all wishlists and add yourself.
- **Edit link** (`?g=…&edit=…`): everyone gets one when they add themselves. Opening it
  on any device lets that device edit their wishlist.
- **Organizer link** (`?g=…&admin=…`): you get it when you create the exchange. Opening it
  on any device lets that device draw names, edit anyone's wishlist, and re-send edit links.

## 1. Create the project

1. Go to <https://console.firebase.google.com> and click **Create a project**.
2. Name it (e.g. `family-gifts`). Turn **Google Analytics off**. Click **Create project**.

## 2. Turn on anonymous sign-in

This is invisible to people: it just gives each browser an ID so the rules can tell
devices apart.

1. Left sidebar → **Build → Authentication** → **Get started**.
2. **Sign-in method** tab → **Anonymous** → toggle **Enable** → **Save**.

## 3. Create the database

1. Left sidebar → **Build → Firestore Database** → **Create database**.
2. Pick a location close to you (e.g. `nam5 (United States)`), then choose
   **Start in production mode** → **Create**.
3. Open the **Rules** tab. Delete what's there, paste the contents of
   [`firestore.rules`](firestore.rules), and click **Publish**.

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
2. Create an exchange. Click **🔑 Copy my organizer link** and save it somewhere safe.
3. Click **📋 Copy link** and send the family link to everyone.
4. Once everyone has joined, click **Draw names**.
