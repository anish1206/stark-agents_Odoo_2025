# Skill Swap Platform

A full-stack Firebase-powered application that allows users to offer and request skill swaps. Users can list their skills, request swaps, accept/reject offers, provide feedback, and more. Admins can moderate content and manage user activity. The app is responsive and mobile-friendly.

By team - Stark Agents
Anish Kshirsagar(TL) - anishkshirsagar120306@gmail.com
Prathamesh Kamble - prathamesh.kamble24@vit.edu
Khush Jain - khush.jain241@vit.edu

---

## 🚀 Features

### 👤 User Features
- Email/password authentication
- Public/private profile toggle
- Add/edit:
  - Name
  - Location (optional)
  - Profile Photo (optional)
  - Skills Offered (multi-select)
  - Skills Wanted (multi-select)
  - Availability (weekends, evenings, etc.)
- Browse/search public profiles by skills
- Request swaps by selecting offered/wanted skills
- Accept, Reject, or Delete swap requests
- View status of all swaps (pending, accepted, rejected, completed)
- Give 1–5 star feedback and message after completing swaps

### 🛠 Admin Features
- Manage all users, swaps, and feedback
- Reject inappropriate or spammy skills
- Ban/unban users
- Send platform-wide announcements via email and FCM
- Download reports (user activity, feedback, swap stats) as CSV

---

## 🔧 Tech Stack

- *Frontend:* Html + CSS + JavaScript
- *Backend:* Firebase (Firestore, Auth, Storage, Functions)
- *Other Services:* Firebase Extensions (Email Trigger, FCM), CSV Reports via Functions

---


## 🔐 Firestore Rules

js
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {

    function isAdmin() {
      return request.auth.token.role == 'admin';
    }

    match /users/{uid} {
      allow read: if resource.data.isPublic || request.auth.uid == uid || isAdmin();
      allow write: if request.auth.uid == uid;
    }

    match /swaps/{swapId} {
      allow read: if request.auth.uid in [resource.data.fromUid, resource.data.toUid] || isAdmin();
      allow create: if request.auth != null && request.resource.data.fromUid == request.auth.uid;
      allow update, delete: if request.auth.uid in [resource.data.fromUid, resource.data.toUid] || isAdmin();
    }

    match /ratings/{id} {
      allow create: if request.auth != null;
    }

    match /announcements/{id} {
      allow read: if true;
      allow write: if isAdmin();
    }
  }
}


---

## ⚙ Cloud Functions

- Auto-update average ratings on feedback submission
- Email/FCM broadcast when an announcement is posted
- Auto-cancel stale swaps (older than 30 days)
- Admin-triggered CSV export endpoints for:
  - User Activity
  - Feedback Logs
  - Swap Statistics

---

## 🧪 Testing

Use Firebase Emulator Suite:

bash
firebase emulators:start


Run frontend:

bash
cd web
npm run dev


---

## 🛠 Setup & Installation

### 1. Clone Project

bash
git clone https://github.com/your-org/skill-swap-platform.git
cd skill-swap-platform


### 2. Install Firebase CLI

bash
npm install -g firebase-tools


### 3. Firebase Setup

bash
firebase login
firebase init
firebase use --add


Enable:
- Auth (email/password)
- Firestore
- Storage
- Cloud Functions
- Hosting
- Extensions (Email, FCM, etc.)

### 4. Install Dependencies

bash
cd web
npm install
cd ../functions
npm install


---

## ▶ Run Locally

bash
firebase emulators:start


And in another terminal:

bash
cd web
npm run dev


---

## 🚀 Deployment

bash
firebase deploy --only hosting,functions,firestore,storage


---

## 📊 Admin Reports

Admin dashboard allows:
- Monitoring users and swaps
- Banning users
- Rejecting skill descriptions
- Downloading CSVs (user stats, feedback, swap history)
- Publishing announcements (FCM + email)

---

## 🖼 Screens

- Screen 1: Public Home Page (Search + Request)
- Screen 2: Login
- Screen 3: My Profile
- Screen 4: User Profile
- Screen 5: Swap Request Modal
- Screen 6: Swap Request Dashboard
- Admin Panel: Moderation + Reports + Messaging

---

## 📱 Mobile Compatibility

All screens are mobile-friendly. The UI is responsive and supports both light/dark modes using Tailwind’s dark: classes.

---

## 📣 Notifications

Announcements are stored in Firestore (/announcements) and broadcast via:
- Firebase Cloud Messaging (FCM)
- Firebase Email Extension

---


## 👨‍💻 Contributors

Developed by the Skill Swap Platform Team  
Contributions are welcome via pull requests!
