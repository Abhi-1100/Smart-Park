# Quick Setup Guide - Smart Parking System

## ⚡ Quick Start (5 Minutes)

### Step 1: Firebase Setup (2 minutes)

1. **Go to Firebase Console**: https://console.firebase.google.com/
2. **Create New Project**:
   - Click "Add project"
   - Name: "Smart Parking System" (or your choice)
   - Click through the setup wizard
   - Wait for project creation

3. **Enable Realtime Database**:
   - Left sidebar → "Realtime Database"
   - Click "Create Database"
   - Select location closest to you
   - Choose "Start in test mode" → Click "Enable"
   - **Copy your Database URL** (looks like: `https://parking-system-xxxxx-default-rtdb.firebaseio.com`)

4. **Enable Authentication**:
   - Left sidebar → "Authentication"
   - Click "Get started"
   - Click "Sign-in method" tab
   - Click "Email/Password" → Enable it → Save
   - Go to "Users" tab
   - Click "Add user"
   - Email: `admin@parking.com` (or your choice)
   - Password: Create a strong password
   - Click "Add user"

### Step 2: Configure Project Files (2 minutes)

1. **Update admin-dashboard.html**:
   - Open file in text editor
   - Find line ~450 (search for: `const firebaseConfig`)
   - Replace the databaseURL with your copied URL:
   ```javascript
   const firebaseConfig = {
       databaseURL: "https://your-actual-database-url.firebaseio.com"
   };
   ```

2. **Update user-panel.html**:
   - Open file in text editor
   - Find line ~370 (search for: `const firebaseConfig`)
   - Replace with the same URL:
   ```javascript
   const firebaseConfig = {
       databaseURL: "https://your-actual-database-url.firebaseio.com"
   };
   ```

3. **Update UPI ID (in user-panel.html)**:
   - Find line ~520 (search for: `const upiId`)
   - Replace with your UPI ID:
   ```javascript
   const upiId = 'yourname@paytm';  // or @okaxis, @ybl, etc.
   ```

### Step 3: Test It! (1 minute)

1. **Test Admin Dashboard**:
   - Open `admin-dashboard.html` in Chrome/Firefox
   - Login with the admin credentials you created
   - You should see the dashboard (might be empty initially)

2. **Test User Panel**:
   - Open `user-panel.html` in browser
   - Try searching for any plate number
   - It will say "not found" (normal - no data yet)

### Step 4: Add Test Data

**Option A: Manual (via Firebase Console)**
1. Go to Firebase Console → Realtime Database
2. Click the "+" icon next to your database URL
3. Add this structure:
   ```json
   {
     "numberplate": {
       "-test1": {
         "date_time": "2026-01-29 10:00:00",
         "number_plate": "TS15EL5671"
       },
       "-test2": {
         "date_time": "2026-01-29 11:30:00",
         "number_plate": "TS15EL5671"
       }
     }
   }
   ```
4. Refresh your admin dashboard - you should see the data!
5. Search for "TS15EL5671" in user panel - you should see entry/exit info!

**Option B: Via ESP32-CAM (Your Hardware)**
- Configure ESP32 with your Firebase URL
- It will automatically upload detected plates

---

## 🔧 Common Issues & Fixes

### Issue: "Permission Denied" Error

**Fix**: Update Firebase Database Rules
1. Go to Firebase Console → Realtime Database → Rules
2. Replace with:
```json
{
  "rules": {
    "numberplate": {
      ".read": true,
      ".write": true
    }
  }
}
```
3. Click "Publish"

### Issue: Admin Can't Login

**Fixes**:
- Check if email/password are correct
- Verify Firebase Authentication is enabled
- Make sure you created a user in Firebase Console
- Try creating a new admin user

### Issue: No Data Showing in Dashboard

**Checks**:
- Is Firebase URL correct in HTML files?
- Are database rules set to allow reading?
- Is there data in Firebase Console → Realtime Database?
- Are there any entries with "number_plate": "NULL"? (These are automatically filtered)

### Issue: QR Code Not Showing

**Fixes**:
- Make sure vehicle has exited (parked vehicles don't show QR)
- Check if amount is greater than ₹0
- Verify internet connection (QR library loads from CDN)
- Check browser console for errors (F12)

---

## 📱 Testing Checklist

### Admin Dashboard
- [ ] Can login with admin credentials
- [ ] See statistics (total, parked, exited, revenue)
- [ ] See parking records in table
- [ ] Search functionality works
- [ ] Can export PDF
- [ ] Can logout

### User Panel
- [ ] Can search for vehicle number
- [ ] See vehicle details (entry, exit, duration)
- [ ] Status shows correctly (Parked/Exited)
- [ ] Amount calculated correctly
- [ ] QR code generates for exited vehicles
- [ ] Shows "parked" alert for parked vehicles

### Pricing Logic Test
Test these scenarios in User Panel:

| Duration | Expected Amount |
|----------|----------------|
| 15 min   | ₹0 (Free)      |
| 30 min   | ₹0 (Free)      |
| 45 min   | ₹20            |
| 90 min   | ₹40            |
| 2h 30m   | ₹60            |

---

## 🎯 Next Steps

### For Production Use:
1. **Secure Firebase**:
   - Update database rules to require authentication
   - Enable Firebase App Check
   - Use environment variables for config

2. **Deploy Website**:
   - Upload to web hosting (Netlify, Vercel, Firebase Hosting)
   - Or use any standard web server

3. **Configure ESP32-CAM**:
   - Update WiFi credentials
   - Set Firebase URL
   - Test plate detection and upload

4. **Customize**:
   - Change colors in CSS
   - Update logo/branding
   - Modify pricing if needed
   - Add company details

### Optional Enhancements:
- Add email notifications
- Integrate SMS alerts
- Add multiple admin users
- Create reserved parking feature
- Add monthly pass system
- Integrate real payment gateway

---

## 🆘 Need Help?

### Resources:
- **Firebase Docs**: https://firebase.google.com/docs
- **Firebase Database Rules**: https://firebase.google.com/docs/database/security
- **QRCode.js**: https://davidshimjs.github.io/qrcodejs/

### Debug Mode:
Press F12 in browser to open Developer Console and check for errors.

### Test Your Firebase URL:
Open this in browser: `https://your-database-url.firebaseio.com/numberplate.json`
You should see your data in JSON format.

---

## ✅ Setup Complete!

If everything works:
- ✅ Admin can login and see dashboard
- ✅ User can search and see vehicle info
- ✅ Data syncs from Firebase in real-time
- ✅ Amounts calculate correctly
- ✅ QR codes generate for payments

**You're ready to use the Smart Parking System! 🎉**

---

**Estimated Total Setup Time**: 5-10 minutes

**Note**: This is a development setup. For production, implement proper security measures and authentication.
