# Student Project & Task Hub

A static HTML app for teacher/student project tracking, task checklists, progress indicators, project chat, and JSON/Excel exports. It is packaged for GitHub + Vercel and uses Firebase Authentication (anonymous sign-in) and Cloud Firestore for cloud persistence.

## Important security notice

**This is a prototype/demo package, not safe for real student records yet.** The original UI implements usernames/passwords inside client-side code and stores teacher credentials in Firestore as plain text. The included Firestore rules allow any signed-in anonymous visitor to read and write shared demo data. This means a public deployment must only contain dummy/test data.

Before using it with real students, replace the prototype credential checks with Firebase Authentication (email/password or another supported provider), implement teacher/student role authorization using trusted server-side claims or carefully designed rules, hash/remove all stored passwords, and restrict Firestore access. Do not store passwords in Firestore or expose them in the teacher profile screen.

## Folder layout

- `public/index.html` — app interface and client logic
- `public/firebase-config.js` — Firebase web app configuration placeholder
- `firestore.rules` — demo-only Firestore rules
- `vercel.json` — static hosting/security headers
- `.gitignore` — excludes common secrets and local files

## 1. Create Firebase storage/database

1. Open the Firebase Console: https://console.firebase.google.com/
2. Create a Firebase project.
3. In **Project settings → General → Your apps**, add a Web App and copy its Firebase configuration.
4. Replace the placeholder values in `public/firebase-config.js` with that web app config.
5. In **Build → Authentication → Sign-in method**, enable **Anonymous** sign-in (required by this demo prototype).
6. In **Build → Firestore Database**, create a database.
7. Open Firestore **Rules**, paste the contents of `firestore.rules`, and publish them. These rules are for dummy/demo data only; they are not production-secure.
8. The app writes shared demo records under `artifacts/student-project-tracker-prod/public/data/projects` and the teacher registry under `artifacts/student-project-tracker-prod/public/data/teachers_registry/accounts`.

Firebase config in a browser app is not a server secret. Never put Firebase Admin SDK service-account JSON or private keys in this folder or frontend code.

## 2. Upload to GitHub

1. Download and unzip this project folder.
2. Edit `public/firebase-config.js` first.
3. Create a new repository at https://github.com/new
4. Upload the **contents** of this folder (so `public`, `firestore.rules`, `vercel.json`, and `README.md` appear at repository root), or use Git:
   ```bash
   git init
   git add .
   git commit -m "Initial Student Project Task Hub"
   git branch -M main
   git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPOSITORY.git
   git push -u origin main
   ```

## 3. Deploy to Vercel

1. Open https://vercel.com/new and import the GitHub repository.
2. Framework preset: **Other**.
3. Root directory: repository root.
4. Build command: leave blank.
5. Output directory: `public`.
6. Click **Deploy**.
7. Open the generated Vercel URL and test using dummy records.

## Notes

- Data persists in Cloud Firestore once Firebase is configured; it is not stored in the GitHub repository.
- Without a valid Firebase config, the app falls back to browser local storage, which is device/browser-specific and not shared across users.
- GitHub stores source code; Vercel hosts the site; Firebase Firestore stores the app's cloud data.
- Do not commit private keys, student data exports, or credentials to GitHub.
