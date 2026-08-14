# Security setup

This site is a public GitHub Pages app. Its tournament data must remain public so
spectators can view it, but writes are enforced by Firebase Realtime Database
rules instead of code or URL passwords.

## Apply the database rules

In Firebase Console, open **Realtime Database** → **Rules**, replace the rules
with the contents of `database.rules.json`, then publish them. Do this before
deploying the updated site: the current database permits unauthenticated writes.

## Create staff accounts and roles

1. In Firebase Console, open **Authentication** → **Sign-in method**, enable
   **Email/Password**, and disable **Anonymous** sign-in.
2. In **Authentication** → **Users**, create an account for each staff member.
3. Copy each user UID and add its role in Realtime Database using the console:

   ```json
   {
     "roles": {
       "FIREBASE_UID_FOR_ADMIN": "admin",
       "FIREBASE_UID_FOR_SCORER": "scorer"
     }
   }
   ```

Only add these entries from the Firebase Console. The deployed app cannot write
to `/roles`.

## GitHub repository settings

In GitHub: **Settings** → **Pages**, deploy from the `main` branch and `/ (root)`.
In **Settings** → **Actions** → **General**, set the workflow token to read-only
and allow actions only from trusted sources. In **Settings** → **Branches**, add
a branch protection rule for `main`: require pull requests, at least one review,
and status checks before merging. Turn on secret scanning and push protection in
**Settings** → **Code security and analysis**.

Never place staff passwords, Firebase service-account JSON, or other secrets in
this public repository. Firebase browser configuration values are identifiers,
not credentials; database rules are the actual access control boundary.
