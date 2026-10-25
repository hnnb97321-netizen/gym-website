# Forge Gym — Firebase setup

The app uses Firebase Authentication (Google) and Firestore. UTR submissions are **not automatically verified**: the owner must compare the UTR and amount against the actual UPI/bank transaction before approving. Never approve from the submitted reference alone.

## 1. Create Firebase project
1. Open https://console.firebase.google.com/ and create a project.
2. In Project settings → General → Your apps, add a Web app.
3. Copy the Firebase web config into `firebase-config.js`, replacing all `PASTE_*` placeholders. Web config is client-side; **never add a service-account JSON/private key to this repo**.
4. In Authentication → Sign-in method, enable **Google**. Set the support email.
5. In Authentication → Settings → Authorized domains, add your deployed domain (for GitHub Pages, `hnnb97321-netizen.github.io`).
6. Create a Firestore database in production mode.
7. In Firestore → Rules, publish the contents of `firestore.rules`.

## 2. Publish the website
This repo is a static website. In GitHub → Settings → Pages, choose Deploy from a branch, `main`, folder `/(root)`. The expected URL is https://hnnb97321-netizen.github.io/gym-website/ . A Pages deployment may take a few minutes.

## 3. Owner account
Sign in with **hnnb97321@gmail.com** using Google. The rules allow only that verified Google email to initialize / recover the owner role. Do not share that Google account. The app's owner email is configured in `index.html` and Firestore rules; keep both consistent if it ever changes.

## 4. Coach access
A coach must sign in once first. Open Owner → Staff & roles and assign the Coach role. Do not promote unknown accounts. The role controls are enforced by Firestore rules.

## 5. UTR payment workflow
1. Owner creates plans in Manage plans.
2. Member pays the gym through its official UPI QR/UPI ID, then enters the UTR/reference in Membership plans.
3. Owner compares the UTR, amount and date with the actual bank/UPI statement, then approves or rejects in Payment approvals.
4. Approval activates the membership and sets expiry based on plan duration.

**Important:** UTR/manual review is not a payment gateway and cannot confirm a transfer automatically. Do not publish a personal UPI QR unless the gym owner has approved it. This starter does not collect PINs/OTPs or process refunds.

## 6. Important limitations / production checklist
- Do not use sample payment records to claim money has been received.
- Test with non-sensitive accounts before real members use it.
- Add a gym-specific UPI QR/ID and contact details only after confirming them with the gym owner.
- Firestore security rules protect writes; review and test rules in Firebase Emulator before launch.
- This app is a practical MVP, not a certified payment system. Membership activation is owner-approved, and only the owner can edit roles and review payments.
