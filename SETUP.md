# Setting up your schedule

You get two pages:
- **schedule.html** — public page, anyone visiting your site can see it
- **admin.html** — private page, only you can sign in and edit it

Both talk to a free Firebase database that stores which dates are open, limited, or closed.

## 1. Create a Firebase project
1. Go to https://console.firebase.google.com and sign in with any Google account.
2. Click **Add project**, name it something like `floriel`, and finish the setup (you can skip Google Analytics).

## 2. Turn on Firestore (the database)
1. In the left sidebar, click **Build > Firestore Database**.
2. Click **Create database**, choose a location close to you, and start in **production mode**.

## 3. Set the security rules
1. In Firestore, go to the **Rules** tab.
2. Replace the contents with:
   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /availability/{doc} {
         allow read: if true;
         allow write: if request.auth != null;
       }
     }
   }
   ```
3. Click **Publish**. This means anyone can *view* the schedule, but only a signed-in user (you) can *change* it.

## 4. Turn on sign-in and create your account
1. In the left sidebar, click **Build > Authentication**.
2. Click **Get started**, then enable the **Email/Password** provider.
3. Go to the **Users** tab and click **Add user**. Enter the email and password you'll use to log into `admin.html`. This is your admin login — keep it private.

## 5. Get your config keys
1. Click the gear icon near the top of the sidebar → **Project settings**.
2. Scroll to **Your apps**, click the **</>** (web) icon, and register an app (any nickname).
3. Firebase shows you a `firebaseConfig` object with keys like `apiKey`, `authDomain`, etc.
4. Copy those values into **both** `schedule.html` and `admin.html`, replacing the placeholder `firebaseConfig` block near the bottom of each file.

## 6. Add the files to your site
1. Copy `schedule.html`, `admin.html`, and the `css/schedule.css` file into your `Floriel` repo (same level as your `index.html`).
2. Commit and push to GitHub — GitHub Pages will publish them automatically.
3. Your pages will be live at:
   - `https://byfloriel.github.io/Floriel/schedule.html`
   - `https://byfloriel.github.io/Floriel/admin.html`
4. Add a link to `schedule.html` somewhere in your site's navigation so customers can find it. **Don't link to `admin.html` publicly** — just bookmark it for yourself, or type the URL directly when you want to edit.

## Using it day-to-day
- Go to `admin.html`, sign in with the email/password from step 4.
- Click a date to cycle it through: unset → open (green) → limited (gold) → closed (rose) → unset.
- Click **Save changes** when you're done. Changes appear on `schedule.html` right away.

## Notes
- The free Firebase tier covers this easily — a small calendar like this uses a tiny fraction of the free daily limits.
- The admin login is a real password check through Firebase, not just a hidden page — so it's safe to have `admin.html` on a public GitHub repo.
