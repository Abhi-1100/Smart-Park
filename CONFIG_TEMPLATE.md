# Configuration Template for Smart Parking System

## Copy and fill in your actual values below, then update the HTML files

### 🔥 FIREBASE CONFIGURATION

```javascript
// Your Firebase Realtime Database URL
// Found in: Firebase Console → Realtime Database
const FIREBASE_DATABASE_URL = "https://parking-system-939dd-default-rtdb.firebaseio.com";

// Example: https://your-project-id-default-rtdb.firebaseio.com
```

### 👤 ADMIN CREDENTIALS

```
Admin Email: ___________________________
Admin Password: _______________________

// Create this in: Firebase Console → Authentication → Users → Add User
```

### 💳 PAYMENT CONFIGURATION (User Panel)

```javascript
// Your UPI ID for receiving payments
const UPI_ID = "yourname@paytm";  // or @okaxis, @ybl, @okicici, etc.
const UPI_NAME = "Smart Parking System";  // Business/receiver name

// Example UPI IDs:
// - PhonePe: yourname@ybl
// - Google Pay: yourname@okaxis or yourname@okicici
// - Paytm: yourname@paytm
```

### 💰 PRICING CONFIGURATION

```javascript
// Parking pricing structure
const FREE_MINUTES = 30;        // First X minutes are free
const HOURLY_RATE = 20;         // Rate per hour (in Rupees) after free period
const CURRENCY = "₹";           // Currency symbol

// Examples of pricing:
// 0-30 minutes: FREE
// 31-90 minutes: ₹20 (1 hour charged)
// 91-150 minutes: ₹40 (2 hours charged)
```

### 🎨 THEME CUSTOMIZATION (Optional)

```css
/* Main colors - Yellow & Black theme */
--yellow: #FFD700;              /* Primary accent color */
--yellow-bright: #FFF200;       /* Hover state */
--yellow-dark: #E6C200;         /* Dark variant */
--black: #0A0A0A;               /* Main background */
--black-light: #1A1A1A;         /* Card background */
--black-lighter: #2A2A2A;       /* Borders */

/* Optional: Change to different theme */
/* Example Blue theme:
--yellow: #0099FF;
--yellow-bright: #00B3FF;
--yellow-dark: #0077CC;
*/

/* Example Green theme:
--yellow: #00FF88;
--yellow-bright: #00FFAA;
--yellow-dark: #00CC77;
*/
```

---

## 📝 HOW TO USE THIS TEMPLATE

### Step 1: Fill in Your Values Above
Replace all placeholder values with your actual configuration.

### Step 2: Update admin-dashboard.html

Find and update these lines:

**Line ~450** - Firebase Configuration:
```javascript
const firebaseConfig = {
    databaseURL: "PASTE_YOUR_FIREBASE_DATABASE_URL_HERE"
};
```

**Line ~630** - Pricing Configuration (optional):
```javascript
function calculateAmount(duration) {
    if (!duration) return 0;
    const totalMinutes = duration.totalMinutes;
    
    const freeMinutes = 30;      // Change this
    const hourlyRate = 20;       // Change this
    
    if (totalMinutes <= freeMinutes) return 0;
    const chargeableMinutes = totalMinutes - freeMinutes;
    const hours = Math.ceil(chargeableMinutes / 60);
    return hours * hourlyRate;
}
```

### Step 3: Update user-panel.html

Find and update these lines:

**Line ~370** - Firebase Configuration:
```javascript
const firebaseConfig = {
    databaseURL: "PASTE_YOUR_FIREBASE_DATABASE_URL_HERE"
};
```

**Line ~520** - UPI Configuration:
```javascript
const upiId = 'abhikakadiya1043@okaxis';
const upiName = 'ParkMeee';  // Change if needed
```

**Line ~480** - Pricing Configuration (optional):
```javascript
function calculateAmount(duration) {
    if (!duration) return 0;
    const totalMinutes = duration.totalMinutes;
    
    const freeMinutes = 30;      // Change this
    const hourlyRate = 20;       // Change this
    
    if (totalMinutes <= freeMinutes) return 0;
    const chargeableMinutes = totalMinutes - freeMinutes;
    const hours = Math.ceil(chargeableMinutes / 60);
    return hours * hourlyRate;
}
```

### Step 4: Save and Test

1. Save both HTML files
2. Open in browser
3. Test admin login
4. Test user search

---

## 🔍 FINDING YOUR VALUES

### Firebase Database URL
1. Go to https://console.firebase.google.com/
2. Select your project
3. Go to "Realtime Database" in left menu
4. Look at the top - you'll see your database URL
5. Format: `https://project-name-xxxxx-default-rtdb.firebaseio.com`

### UPI ID
- This is your personal/business UPI ID
- Format: name@provider
- You can find it in your UPI app (PhonePe, GPay, Paytm)
- Or create a new one in your banking app

### Admin Credentials
- Create in Firebase Console
- Go to: Authentication → Users → Add User
- Set email and strong password
- Remember these for admin login

---

## ⚙️ ADVANCED CUSTOMIZATION

### Change Logo Text
Find in both HTML files:
```html
<h1 class="logo">Smart Parking</h1>
```
Change "Smart Parking" to your company name

### Change Page Titles
Find in `<title>` tags:
```html
<title>Smart Parking - Admin Dashboard</title>
<title>Smart Parking - User Panel</title>
```

### Modify Statistics Labels
In admin-dashboard.html, find:
```html
<div class="stat-label">Total Vehicles</div>
<div class="stat-label">Currently Parked</div>
<div class="stat-label">Total Exited</div>
<div class="stat-label">Today's Revenue</div>
```

### Change Currency Symbol
Find all instances of `₹` and replace with your currency:
- `$` for USD
- `€` for EUR
- `£` for GBP
- etc.

---

## 📋 CHECKLIST

Before going live, verify:

- [ ] Firebase Database URL updated in both files
- [ ] Admin credentials created in Firebase
- [ ] UPI ID configured in user-panel.html
- [ ] Pricing values match your requirements
- [ ] Theme colors customized (optional)
- [ ] Logo/branding updated (optional)
- [ ] Currency symbol is correct
- [ ] All files saved
- [ ] Tested in browser
- [ ] Admin login works
- [ ] User search works
- [ ] Payment QR generates correctly

---

## 💾 BACKUP THIS CONFIGURATION

Save this file with your filled values for future reference!

**Your Configuration Summary:**
```
Firebase URL: _________________________________
Admin Email: __________________________________
UPI ID: _______________________________________
Free Minutes: _____ minutes
Hourly Rate: ₹_____ per hour
Theme: Yellow/Black (or custom: _____________)
```

---

## 🆘 NEED HELP?

If you're stuck on any configuration:
1. Check QUICK_SETUP.md for step-by-step guide
2. Check README.md for detailed documentation
3. Check browser console (F12) for errors
4. Verify Firebase Console for correct setup

Remember: Test in development before production! 🚀
