# 🔥 Firebase Setup Guide - Step by Step

This guide will walk you through setting up Firebase for your Expense Manager application.

## Prerequisites
- Google Account
- GitHub Account (for hosting)

---

## Part 1: Firebase Project Setup

### Step 1: Create Firebase Project

1. **Go to Firebase Console**
   - Visit: https://console.firebase.google.com/
   - Sign in with your Google Account

2. **Create New Project**
   - Click "Add project" (or "Create a project")
   - Enter project name: `expense-manager` (or your preferred name)
   - Click "Continue"

3. **Google Analytics**
   - Toggle OFF "Enable Google Analytics" (optional, not needed)
   - Click "Create project"
   - Wait for project to be created (~30 seconds)
   - Click "Continue"

---

## Part 2: Enable Authentication

### Step 2: Setup Email/Password Authentication

1. **Navigate to Authentication**
   - In the left sidebar, click "Authentication"
   - Click "Get started" button

2. **Enable Email/Password**
   - Go to "Sign-in method" tab
   - Click on "Email/Password"
   - Toggle ON the first option: "Email/Password"
   - Keep "Email link" toggle OFF
   - Click "Save"

### Step 3: Create User Accounts

**Important:** Since there's no signup page, you must create users manually!

1. **Go to Users Tab**
   - Click "Users" tab in Authentication
   - Click "Add user" button

2. **Add User Details**
   - Enter Email: `youremail@example.com`
   - Enter Password: (minimum 6 characters)
   - Click "Add user"

3. **Repeat for all users**
   - Add as many users as you need
   - Each user gets their own isolated data

**Example Users:**
```
User 1: john@example.com / password123
User 2: jane@example.com / password456
```

---

## Part 3: Setup Firestore Database

### Step 4: Create Firestore Database

1. **Navigate to Firestore**
   - In left sidebar, click "Firestore Database"
   - Click "Create database" button

2. **Choose Mode**
   - Select **"Start in production mode"**
   - Click "Next"

3. **Select Location**
   - Choose: `asia-south1 (Mumbai)` for Indian users
   - Or select closest location to you
   - Click "Enable"
   - Wait for database creation (~1 minute)

### Step 5: Configure Security Rules

1. **Go to Rules Tab**
   - Click "Rules" tab in Firestore Database
   - You'll see default production rules

2. **Replace with Custom Rules**
   - Delete all existing text
   - Copy and paste this:

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    
    // Transactions collection
    match /transactions/{transactionId} {
      // Users can only read/write their own transactions
      allow read: if request.auth != null && 
                     resource.data.userId == request.auth.uid;
      allow create: if request.auth != null && 
                       request.resource.data.userId == request.auth.uid;
      allow update, delete: if request.auth != null && 
                              resource.data.userId == request.auth.uid;
    }
    
    // Budgets collection
    match /budgets/{budgetId} {
      // Users can only read/write their own budgets
      allow read: if request.auth != null && 
                     resource.data.userId == request.auth.uid;
      allow create: if request.auth != null && 
                       request.resource.data.userId == request.auth.uid;
      allow update, delete: if request.auth != null && 
                              resource.data.userId == request.auth.uid;
    }
    
    // Recurring transactions collection
    match /recurring/{recurringId} {
      // Users can only read/write their own recurring transactions
      allow read: if request.auth != null && 
                     resource.data.userId == request.auth.uid;
      allow create: if request.auth != null && 
                       request.resource.data.userId == request.auth.uid;
      allow update, delete: if request.auth != null && 
                              resource.data.userId == request.auth.uid;
    }
  }
}
```

3. **Publish Rules**
   - Click "Publish" button
   - Rules are now active!

**What these rules do:**
- ✅ Only authenticated users can access data
- ✅ Users can only see their own transactions
- ✅ Users can only modify their own data
- ✅ Complete data isolation between users

---

## Part 4: Get Firebase Configuration

### Step 6: Register Web App

1. **Go to Project Settings**
   - Click the gear icon (⚙️) next to "Project Overview"
   - Click "Project settings"

2. **Scroll to "Your apps"**
   - Scroll down to find "Your apps" section
   - Currently shows "There are no apps in your project"

3. **Add Web App**
   - Click the Web icon: `</>`
   - Enter app nickname: `Expense Manager Web`
   - **Do NOT** check "Also set up Firebase Hosting" (we're using GitHub Pages)
   - Click "Register app"

4. **Copy Configuration**
   - You'll see the Firebase SDK configuration
   - Copy ONLY the config object (between the curly braces)

**Example (yours will be different):**
```javascript
const firebaseConfig = {
  apiKey: "AIzaSyBXXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
  authDomain: "expense-manager-12345.firebaseapp.com",
  projectId: "expense-manager-12345",
  storageBucket: "expense-manager-12345.appspot.com",
  messagingSenderId: "123456789012",
  appId: "1:123456789012:web:abcdef1234567890abcdef"
};
```

5. **Save Configuration**
   - Copy this to a text file temporarily
   - Click "Continue to console"

---

## Part 5: Configure Application

### Step 7: Update index.html

1. **Open index.html**
   - Open the `index.html` file in a text editor

2. **Find Firebase Config**
   - Use Find (Ctrl+F or Cmd+F)
   - Search for: `YOUR_API_KEY`
   - You'll find this section (around line 1020):

```javascript
const firebaseConfig = {
    apiKey: "YOUR_API_KEY",
    authDomain: "YOUR_AUTH_DOMAIN",
    projectId: "YOUR_PROJECT_ID",
    storageBucket: "YOUR_STORAGE_BUCKET",
    messagingSenderId: "YOUR_MESSAGING_SENDER_ID",
    appId: "YOUR_APP_ID"
};
```

3. **Replace with Your Config**
   - Delete the placeholder values
   - Paste your actual Firebase configuration
   - Save the file

**Example (with real values):**
```javascript
const firebaseConfig = {
    apiKey: "AIzaSyBXXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
    authDomain: "expense-manager-12345.firebaseapp.com",
    projectId: "expense-manager-12345",
    storageBucket: "expense-manager-12345.appspot.com",
    messagingSenderId: "123456789012",
    appId: "1:123456789012:web:abcdef1234567890abcdef"
};
```

---

## Part 6: Deploy to GitHub Pages

### Step 8: Create GitHub Repository

1. **Go to GitHub**
   - Visit: https://github.com
   - Sign in to your account

2. **Create New Repository**
   - Click the "+" icon (top right)
   - Select "New repository"

3. **Repository Settings**
   - Repository name: `expense-manager`
   - Description: "Personal Expense Manager with Firebase"
   - Select "Public"
   - **Do NOT** initialize with README (we have our own)
   - Click "Create repository"

### Step 9: Upload Files

**Option A: Using GitHub Web Interface (Easier)**

1. **Upload Files**
   - On your new repository page, click "uploading an existing file"
   - Drag and drop these files:
     - `index.html`
     - `README.md`
     - `.gitignore`
   - Add commit message: "Initial commit"
   - Click "Commit changes"

**Option B: Using Git Command Line**

```bash
# Navigate to your project folder
cd /path/to/your/project

# Initialize git
git init

# Add files
git add .

# Commit
git commit -m "Initial commit: Expense Manager"

# Add remote (replace USERNAME and REPO)
git remote add origin https://github.com/USERNAME/expense-manager.git

# Push to GitHub
git branch -M main
git push -u origin main
```

### Step 10: Enable GitHub Pages

1. **Go to Settings**
   - In your repository, click "Settings" tab

2. **Navigate to Pages**
   - In left sidebar, click "Pages"

3. **Configure Source**
   - Under "Source", select **main** branch
   - Keep folder as **/ (root)**
   - Click "Save"

4. **Wait for Deployment**
   - GitHub will build and deploy your site
   - Takes about 1-2 minutes
   - Refresh the page to see the URL

5. **Get Your URL**
   - Your app will be live at:
   - `https://USERNAME.github.io/expense-manager/`
   - Example: `https://john-doe.github.io/expense-manager/`

---

## Part 7: Test Your Application

### Step 11: First Login

1. **Visit Your App**
   - Go to your GitHub Pages URL
   - You should see the login page

2. **Login**
   - Enter email: (the one you created in Firebase)
   - Enter password: (the one you set)
   - Click "Sign In"

3. **Success!**
   - You should now see the dashboard
   - All stats will show ₹0 (no data yet)

### Step 12: Add First Transaction

1. **Click Add Transaction**
   - Click the "+" floating button (bottom right)
   - Or click "Add Transaction" button

2. **Fill Details**
   - Type: Expense
   - Amount: 500
   - Category: Groceries
   - Date: Today
   - Note: "Weekly groceries"
   - Payment: UPI
   - Click "Save Transaction"

3. **Verify**
   - Transaction appears in "Recent Transactions"
   - Dashboard stats update
   - Go to "Transactions" tab to see all

---

## 🎉 You're All Set!

Your Personal Expense Manager is now:
- ✅ Configured with Firebase
- ✅ Deployed on GitHub Pages
- ✅ Accessible from any device
- ✅ Secure with user authentication
- ✅ Ready to track expenses!

---

## 📱 Next Steps

1. **Bookmark the URL** - Add it to your home screen on mobile
2. **Set a Budget** - Go to Budgets tab and set your monthly limits
3. **Add Recurring** - Set up recurring expenses like rent, subscriptions
4. **Track Daily** - Add transactions as you spend
5. **Review Monthly** - Check reports to understand spending patterns

---

## 🔧 Managing Users

### Adding New Users
1. Go to Firebase Console → Authentication → Users
2. Click "Add user"
3. Enter email and password
4. Click "Add user"
5. Share credentials with the user

### Removing Users
1. Go to Firebase Console → Authentication → Users
2. Click on user you want to remove
3. Click "Delete user"
4. Confirm deletion

### Resetting Passwords
1. Go to Firebase Console → Authentication → Users
2. Click on the user
3. Click "Reset password"
4. Copy the link and send to user

---

## 🛟 Troubleshooting

### "Permission denied" errors
- Check Firestore rules are set correctly
- Verify user is logged in
- Check browser console for details

### Login not working
- Verify user exists in Firebase Authentication
- Check email/password are correct
- Ensure Firebase config is correct in index.html

### Data not showing
- Check browser console for errors
- Verify Firebase config is correct
- Try logging out and back in

### Site not loading
- Wait 2-3 minutes after enabling GitHub Pages
- Check repository is public
- Verify index.html is in root folder

---

## 🔒 Security Best Practices

1. **Never share your Firebase config publicly** (it's safe in client code, but don't commit to public repos with sensitive data)
2. **Use strong passwords** for user accounts
3. **Regularly backup data** by exporting to CSV
4. **Monitor Firebase Console** for unusual activity
5. **Keep Firestore rules strict** - only allow user access to their own data

---

## 💰 Firebase Free Tier Limits

Your app runs on Firebase's free "Spark" plan:

- **Authentication**: Unlimited users
- **Firestore**: 
  - 1 GB storage
  - 50,000 reads/day
  - 20,000 writes/day
  - 20,000 deletes/day

**For personal use, this is more than enough!**

To monitor usage:
1. Go to Firebase Console
2. Click "Usage and billing"
3. View current usage

---

## 📧 Need Help?

If you encounter issues:
1. Check this guide again
2. Review Firebase Console for errors
3. Check browser console (F12) for JavaScript errors
4. Verify all steps were completed

---

## 🎊 Enjoy Your Expense Manager!

You now have a fully functional, secure, and feature-rich expense tracking application! 

Start tracking your finances and gain insights into your spending habits! 💰📊✨