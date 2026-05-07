# PureTrack

Sales & Inventory Tracker 📦 💵
A mobile-first, serverless web application designed to help small retail suppliers and resellers
track sales, manage specific product inventory, reconcile cash against supplier debts, and
monitor team performance.
This app is built with HTML, Tailwind CSS, and vanilla JavaScript, and uses Google Firebase
for real-time cloud syncing and data persistence.
✨ Key Features
Dynamic Sales Wizard: 3-step animated checkout flow mapping items, weights, and base
supplier costs to custom retail prices.
Itemized Inventory Management: Track stock levels granularly (e.g., Soft 1g, Hard 3.5g)
and instantly see your estimated potential profit based on current stock.
Supplier Ledger & Cash Reconciliation: Automatically track your rolling debt to your
supplier, log cash payments, add custom debts, and visually reconcile your on-hand cash
and bank balances to see if you are short or in surplus.
Team Performance: Switch between active sellers and view leaderboards showing gross
sales and net profit per team member.
Cloud Sync & Backup: Automated syncing to Google Firestore with local fallback options
(Import/Export JSON).
🚀Setup & Deployment Guide
To host this application yourself for free, you will need to set up a Google Firebase project for
the database/authentication, and a GitHub repository for web hosting.
Phase 1: Firebase Setup (Backend)
1. Create a Firebase Project:
Go to the Firebase Console.
Click Add Project, give it a name (e.g., sales-tracker ), and proceed through the
creation steps (Google Analytics is optional).
2. Enable Authentication:
In the left sidebar, click Authentication -> Get Started.
Go to the Sign-in method tab.
5/7/26, 4:50 PM App Update: Bundle Deals and Profit Maximization - Google Gemini
https://gemini.google.com/app/f2183a1c2417f8b6?utm_source=app_launcher&utm_medium=owned&utm_campaign=base_all 1/4
Enable Anonymous sign-in. (This allows the app to silently create an account for you
to tie your data to without needing passwords).
3. Enable Firestore Database:
In the left sidebar, click Firestore Database -> Create Database.
Choose a location close to you.
Start in Test Mode (or update the rules later).
Recommended Security Rules: Go to the Rules tab in Firestore and paste the following
to ensure only logged-in users can access their own data:
rules_version = '2';
service cloud.firestore {
match /databases/{database}/documents {
match /artifacts/{appId}/users/{userId}/{document=**} {
allow read, write: if request.auth != null && request.auth.uid == us
}
}
}
4. Get Your Firebase Config:
Go to Project Overview (gear icon) -> Project settings.
Scroll down to the Your apps section and click the </> (Web) icon to add a web app.
Register the app (name it anything).
Firebase will present you with a firebaseConfig object. Keep this page open; you will
need to paste this into your HTML file.
Phase 2: Preparing the index.html File
Currently, the HTML file is built to inject variables dynamically ( __firebase_config ). To deploy
it yourself on GitHub Pages, you need to hardcode your specific Firebase credentials.
Open your index.html file and locate the initFirebase() function inside the <script> tag
near the bottom.
Find this block of code:
const firebaseConfig = typeof __firebase_config !== 'undefined' ? JSON.parse(__fi
if (!firebaseConfig) {
console.warn("No Firebase config found. Running disconnected.");
return;
}
5/7/26, 4:50 PM App Update: Bundle Deals and Profit Maximization - Google Gemini
https://gemini.google.com/app/f2183a1c2417f8b6?utm_source=app_launcher&utm_medium=owned&utm_campaign=base_all 2/4
appId = typeof __app_id !== 'undefined' ? __app_id : 'default-app-id';
Replace it with your actual Firebase config:
// Paste the config object you got from Phase 1, Step 4 here:
const firebaseConfig = {
apiKey: "YOUR_API_KEY",
authDomain: "YOUR_PROJECT_ID.firebaseapp.com",
projectId: "YOUR_PROJECT_ID",
storageBucket: "YOUR_PROJECT_ID.firebasestorage.app",
messagingSenderId: "YOUR_SENDER_ID",
appId: "YOUR_APP_ID"
};
// Set your own custom App ID name
appId = 'my-custom-sales-tracker';
Remove the custom token logic (Optional but recommended): If you are running this
standalone, you only need Anonymous Auth. Change the sign-in block inside initFirebase()
from:
if (typeof __initial_auth_token !== 'undefined' && __initial_auth_token) {
await signInWithCustomToken(auth, __initial_auth_token);
} else {
await signInAnonymously(auth);
}
To:
await signInAnonymously(auth);
Phase 3: Deployment via GitHub Pages
1. Create a GitHub Repository:
Log into GitHub.
Click the + in the top right and select New repository.
Name it (e.g., sales-tracker ), make it Public or Private, and click Create repository.
2. Upload Your File:
5/7/26, 4:50 PM App Update: Bundle Deals and Profit Maximization - Google Gemini
https://gemini.google.com/app/f2183a1c2417f8b6?utm_source=app_launcher&utm_medium=owned&utm_campaign=base_all 3/4
Click uploading an existing file on the repository setup page.
Drag and drop your modified index.html file into the repository.
Commit the changes.
3. Enable GitHub Pages:
In your GitHub repository, click on the Settings tab.
On the left sidebar, click on Pages.
Under "Build and deployment", set the Source to Deploy from a branch .
Under "Branch", select main (or master ), leave the folder as / (root) , and click
Save.
Wait 1-2 minutes. Refresh the page, and GitHub will show you the live URL where your
app is hosted!
⚙️App Usage Guide
1. First Launch: When you first load the app, it will sync anonymously to the cloud. You will
see a green cloud icon ☁️ at the top and a User ID string on the Sales tab.
2. Setup Catalog: Go to Settings -> Catalog & Pricing. Add your Item Types (e.g., Sativa)
and Weights (e.g., 1g).
3. Set Pricing: Use the Pricing Matrix dropdown in Settings to define the Supplier Cost and
Suggested Retail Price for every combo. This is required for the profit calculator to work.
4. Add Stock: Go to the Sales tab, click "Manage" on the Inventory Overview, and type in
how many of each specific item you currently have.
5. Sell: Use the wizard on the Sales page to ring up customers.
Overpayments/underpayments will automatically calculate against your set supplier costs.
6 End of Day: Hit Save Day & Reset Shift to lock in your daily history update your active
5/7/26, 4:50 PM App Update: Bundle Deals and Profit Maximization - Google Gemini
https://gemini.google.com/app/f2183a1c2417f8b6?utm_source=app_launcher&utm_medium=owned&utm_campaign=base_all 4/4