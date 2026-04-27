Refund Status Companion
Refund Status Portal
Static webpage prototype styled with the supplied CSS direction, plus a companion Chrome extension for safe status extraction from the official portal.
This version is a two-page Firebase-backed web app:
Important note
`index.html` for client lookup
`office.html` for office staff login and Excel upload
This implementation deliberately does not collect or relay the Income Tax portal password through a custom webpage. Instead, it keeps login on the official portal and uses a browser extension to read the filed-return page after the user is already authenticated there.
Flow
Office staff sign in on `office.html` using Firebase Authentication.
Staff upload the latest Excel workbook.
The workbook is parsed in the browser and the rows are published to Cloud Firestore.
Clients use `index.html` to check their refund record by PAN and assessment year.
Files
`index.html` - companion webpage ready for GitHub Pages
`extension/` - Chrome extension that extracts AY, status, date, and PAN from the official portal page
`index.html` - client lookup page
`office.html` - staff login and upload page
`client.js` - client lookup logic with Firestore queries
`office.js` - Firebase auth, Excel parsing, and Firestore publishing
`firebase-config.js` - Firebase config and collection constants
`styles.css` - shared styling
`data/refund-status.json` - sample data generated from the provided workbook
`firestore.rules.example` - example rules for public reads and authenticated writes
Firebase configuration used
The app is currently wired to:
`projectId`: `client-tracker-tool-5bdf6`
`authDomain`: `client-tracker-tool-5bdf6.firebaseapp.com`
Firebase setup required
In Firebase Console, enable `Authentication -> Sign-in method -> Email/Password`.
Create the office staff users under `Authentication -> Users`.
Enable Cloud Firestore for the project.
Allow the deployed domain in Firebase Authentication authorized domains if needed.
If your Firebase console provides additional web-config fields like `appId`, add them in `firebase-config.js`.
Suggested Firestore structure
Collection: `refundRecords`
Metadata document: `datasetMeta/current`
Each uploaded row is stored with:
`assessmentYear`
`serialNo`
`codeNo`
`name`
`pan`
`refundAmount`
`refundStatus`
`uploadedBy`
`uploadedAt`
Important deployment note
This app can be deployed to GitHub Pages, but Firebase Authentication and Firestore must be enabled correctly in the Firebase project for the login and live data flow to work.
The client page also needs Firestore read access. If you want public client lookup by PAN and assessment year, your Firestore rules must allow the required reads. If you want tighter access control, add a client-authentication layer or a server-side API.
Deploy to GitHub Pages
Upload `index.html`, `README.md`, and the full `extension/` folder to your repository.
In GitHub, open the repository settings.
Go to `Pages`.
Under `Build and deployment`, choose `Deploy from a branch`.
Select the `main` branch and `/ (root)` folder.
Save the settings.
GitHub will publish the site and show the final URL on the Pages screen.
The expected Pages URL for this repository will usually be `https://tanoojreddy1801-rgb.github.io/refund-status/`.
Upload all files in this folder to your GitHub repository.
In GitHub, open `Settings -> Pages`.
Choose `Deploy from a branch`.
Select branch `main` and folder `/ (root)`.
Save the settings.
The expected Pages URL is usually:
Load the extension
`https://tanoojreddy1801-rgb.github.io/refund-status/`
Download or clone the repository locally.
Open `chrome://extensions`.
Enable `Developer mode`.
Click `Load unpacked`.
Select the `extension` folder from the repository.
References
Local preview
Firebase's web SDK docs show the modular CDN imports and app initialization flow, and Firestore's quickstart covers initializing Firestore from `initializeApp(...)` and `getFirestore(app)`.
Open `index.html` in a browser.
Sources:
Firebase web SDK overview
Cloud Firestore quickstart
Firebase Auth email/password reference
