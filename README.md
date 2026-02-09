# 💰 Personal Expense Manager

A comprehensive, feature-rich expense tracking application built for Indian users with Firebase backend and designed to be hosted on GitHub Pages.

## 🌟 Features

### Core Features
- ✅ **Email Login Only** - Secure authentication (no signup, admin-controlled users)
- 💵 **Indian Rupee (₹) Support** - All amounts formatted in INR
- 📱 **Mobile & Web Responsive** - Optimized UI for all devices
- 🔒 **Firebase Backend** - Secure cloud database

### Transaction Management
- ➕ Add/Edit/Delete transactions
- 📊 Income and Expense tracking
- 🏷️ Categorized transactions
- 💳 Multiple payment methods (UPI, Cash, Card, Net Banking, Wallet)
- 📝 Notes for each transaction
- 🔍 Advanced filters (Type, Category, Date range)
- 📤 Export to CSV

### Budget Management
- 🎯 Set monthly budgets by category
- 📈 Visual progress bars
- ⚠️ Budget alerts (warning at 80%, danger at 100%)
- 🔴 Color-coded status

### Recurring Transactions
- 🔄 Automated recurring expenses/income
- ⏰ Daily, Weekly, Monthly, Yearly frequencies
- 🤖 Auto-generated transactions
- 📅 Start date configuration

### Reports & Analytics
- 📊 **Multiple Chart Types**
  - Category breakdown (Doughnut chart)
  - Monthly trend (Line chart)
  - Income vs Expense (Bar chart)
- 📅 **Time Periods**
  - Current Month
  - Last Month
  - Last 3 Months
  - Last 6 Months
  - This Year
- 💰 **Dashboard Stats**
  - Total Balance
  - Monthly Income
  - Monthly Expenses
  - Savings Rate

### Indian-Specific Categories

**Expense Categories:**
- Groceries
- Food & Dining
- Transportation
- Utilities
- Rent/EMI
- Healthcare
- Entertainment
- Shopping
- Education
- Insurance
- Mobile Recharge
- Internet
- Clothing
- Personal Care
- Household
- Gifts
- Others

**Income Categories:**
- Salary
- Business
- Freelance
- Investment
- Rental Income
- Interest
- Gift Received
- Others

## 🚀 Setup Instructions

### 1. Firebase Setup

#### Step 1: Create Firebase Project
1. Go to [Firebase Console](https://console.firebase.google.com/)
2. Click "Add Project"
3. Enter project name (e.g., "expense-manager")
4. Disable Google Analytics (optional)
5. Click "Create Project"

#### Step 2: Enable Authentication
1. In Firebase Console, go to **Authentication**
2. Click "Get Started"
3. Go to **Sign-in method** tab
4. Click **Email/Password**
5. Enable **Email/Password** (keep Email link disabled)
6. Click "Save"

#### Step 3: Create User Accounts (Admin Only)
Since there's no signup page, you need to create users manually:

1. Go to **Authentication** → **Users** tab
2. Click "Add User"
3. Enter email and password for each user you want to create
4. Click "Add User"

**Important:** Only users you create here will be able to login!

#### Step 4: Enable Firestore Database
1. Go to **Firestore Database**
2. Click "Create Database"
3. Select **Start in production mode**
4. Choose your location (e.g., asia-south1 for India)
5. Click "Enable"

#### Step 5: Set Firestore Rules
1. Go to **Firestore Database** → **Rules** tab
2. Replace the rules with:

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // Users can only access their own data
    match /transactions/{document} {
      allow read, write: if request.auth != null && 
        request.resource.data.userId == request.auth.uid;
    }
    
    match /budgets/{document} {
      allow read, write: if request.auth != null && 
        request.resource.data.userId == request.auth.uid;
    }
    
    match /recurring/{document} {
      allow read, write: if request.auth != null && 
        request.resource.data.userId == request.auth.uid;
    }
  }
}
```

3. Click "Publish"

#### Step 6: Get Firebase Configuration
1. Go to **Project Settings** (gear icon)
2. Scroll down to "Your apps"
3. Click the **Web** icon (`</>`)
4. Register app (name it "Expense Manager Web")
5. Copy the `firebaseConfig` object

It will look like this:
```javascript
const firebaseConfig = {
  apiKey: "AIzaSyXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
  authDomain: "your-project.firebaseapp.com",
  projectId: "your-project",
  storageBucket: "your-project.appspot.com",
  messagingSenderId: "123456789012",
  appId: "1:123456789012:web:abcdef123456"
};
```

### 2. Configure the Application

1. Open `index.html`
2. Find the Firebase configuration section (around line 1020):
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
3. Replace with your actual Firebase config values
4. Save the file

### 3. Deploy to GitHub Pages

#### Option 1: Using GitHub Web Interface
1. Create a new repository on GitHub
2. Name it anything (e.g., "expense-manager")
3. Upload `index.html` to the repository
4. Go to **Settings** → **Pages**
5. Under "Source", select **main** branch
6. Click "Save"
7. Your app will be live at: `https://yourusername.github.io/expense-manager/`

#### Option 2: Using Git Command Line
```bash
# Initialize git repository
git init

# Add files
git add index.html README.md

# Commit
git commit -m "Initial commit: Expense Manager"

# Add remote (replace with your repo URL)
git remote add origin https://github.com/yourusername/expense-manager.git

# Push to GitHub
git branch -M main
git push -u origin main

# Enable GitHub Pages
# Go to repository Settings → Pages → Select main branch → Save
```

### 4. First Time Login

1. Visit your GitHub Pages URL
2. Login with the email/password you created in Firebase Authentication
3. Start adding transactions!

## 📱 Usage Guide

### Adding a Transaction
1. Click the **+ (FAB button)** or "Add Transaction" button
2. Select **Type** (Expense or Income)
3. Enter **Amount** in ₹
4. Select **Category**
5. Choose **Date**
6. Add a **Note** (optional)
7. Select **Payment Method**
8. Click "Save Transaction"

### Setting a Budget
1. Go to **Budgets** tab
2. Click "Set Budget"
3. Select **Category**
4. Enter **Monthly Limit** in ₹
5. Set **Alert Percentage** (default 80%)
6. Click "Save Budget"

### Creating Recurring Transaction
1. Go to **Recurring** tab
2. Click "Add Recurring"
3. Fill in details (Type, Amount, Category)
4. Select **Frequency** (Daily/Weekly/Monthly/Yearly)
5. Choose **Start Date**
6. Click "Save Recurring"

The app will automatically create transactions based on frequency!

### Viewing Reports
1. Go to **Reports** tab
2. Select time period (Current Month, Last 3 Months, etc.)
3. View:
   - Monthly trend chart
   - Income vs Expense comparison
   - Category breakdown with percentages

### Exporting Data
1. Go to **Transactions** tab
2. Apply any filters you want
3. Click "Export" button
4. CSV file will download with all transactions

## 🔒 Security Features

- ✅ Authentication required for all access
- ✅ Users can only see their own data
- ✅ Firestore security rules enforce data isolation
- ✅ No client-side data storage (all in Firebase)
- ✅ Admin-controlled user creation (no public signup)

## 🎨 Customization

### Change Categories
Edit the `expenseCategories` and `incomeCategories` arrays in `index.html`:

```javascript
const expenseCategories = [
    'Your', 'Custom', 'Categories', 'Here'
];
```

### Change Colors
Modify CSS variables in the `:root` section:

```css
:root {
    --primary-color: #2563eb;
    --secondary-color: #10b981;
    --danger-color: #ef4444;
    /* etc... */
}
```

## 📊 Database Structure

### Collections

**transactions**
```javascript
{
  userId: "string",
  type: "expense" | "income",
  amount: number,
  category: "string",
  date: "YYYY-MM-DD",
  note: "string",
  paymentMethod: "cash" | "upi" | "card" | "netbanking" | "wallet",
  recurring: boolean,
  createdAt: timestamp
}
```

**budgets**
```javascript
{
  userId: "string",
  category: "string",
  amount: number,
  alertAt: number // percentage
}
```

**recurring**
```javascript
{
  userId: "string",
  type: "expense" | "income",
  amount: number,
  category: "string",
  frequency: "daily" | "weekly" | "monthly" | "yearly",
  startDate: "YYYY-MM-DD",
  note: "string",
  active: boolean,
  lastProcessed: "YYYY-MM-DD" | null
}
```

## 🐛 Troubleshooting

### Login Not Working
- Check Firebase configuration is correct
- Verify user exists in Firebase Authentication
- Check browser console for errors

### Data Not Saving
- Verify Firestore rules are set correctly
- Check Firebase quota limits
- Ensure user is authenticated

### Charts Not Showing
- Make sure Chart.js is loading (check network tab)
- Verify there's data for the selected period
- Check browser console for errors

## 🚀 Future Enhancements Ideas

- 📧 Email reports
- 🔔 Budget alert notifications
- 🌓 Dark mode
- 📱 PWA (Progressive Web App) support
- 🗂️ Attachments/receipts
- 👥 Family/household sharing
- 💱 Multi-currency support
- 🎯 Financial goals tracking
- 📈 Investment tracking
- 🧾 Bill reminders

## 📄 License

This project is open source and free to use for personal purposes.

## 💡 Tips

1. **Regular Backups**: Export your data regularly as CSV
2. **Categories**: Keep categories consistent for better reporting
3. **Notes**: Add detailed notes for better tracking
4. **Budgets**: Set realistic budgets based on past spending
5. **Recurring**: Use for all regular expenses (rent, subscriptions, etc.)

## 🆘 Support

For issues or questions:
1. Check the Troubleshooting section
2. Review Firebase Console for errors
3. Check browser console for JavaScript errors
4. Verify all setup steps were completed

## 🎉 Enjoy Managing Your Finances!

Happy tracking! 💰📊