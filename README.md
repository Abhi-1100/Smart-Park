# Smart Parking System

A modern, responsive web-based parking management system with Firebase integration, featuring separate admin and user interfaces with real-time data synchronization.

## 🚀 Features

### Admin Dashboard
- **Secure Authentication**: Firebase-based admin login system
- **Real-time Monitoring**: Live parking data synchronized from Firebase
- **Smart Filtering**: Automatically filters out NULL/invalid plate entries
- **Comprehensive Statistics**:
  - Total vehicles
  - Currently parked vehicles
  - Total exited vehicles
  - Today's revenue
- **Advanced Search**: Filter records by plate number
- **PDF Export**: Generate detailed parking reports
- **Entry/Exit Tracking**: Automatic calculation of parking duration
- **Dynamic Pricing**: ₹20/hour after first 30 minutes free

### User Panel
- **Simple Search**: Users can only view their own vehicle data
- **Vehicle Status**: Real-time entry/exit information
- **Duration Calculation**: Automatic time tracking
- **Payment Integration**: Dynamic UPI QR code generation
- **Mobile-Friendly**: Fully responsive design
- **Clear Pricing Display**: Transparent billing information

## 🎨 Design Features

- **Modern UI**: Yellow and black professional theme
- **Distinctive Typography**: Syne (display) + Space Mono (body)
- **Smooth Animations**: Fade-in effects and hover states
- **Grid Background**: Subtle animated background pattern
- **Card-Based Layout**: Clean, organized information display
- **Responsive Design**: Works seamlessly on all devices

## 📋 Prerequisites

Before you begin, ensure you have:

1. **Firebase Account**: Create a project at [Firebase Console](https://console.firebase.google.com/)
2. **Firebase Realtime Database**: Set up with the following structure:
   ```json
   {
     "numberplate": {
       "-Ok9XPAD8IUA0S_54L-Q": {
         "date_time": "2026-01-29 21:45:16",
         "number_plate": "TS15EL5671"
       }
     }
   }
   ```

3. **Firebase Authentication**: Enable Email/Password authentication

## 🛠️ Setup Instructions

### Step 1: Firebase Configuration

1. **Create Firebase Project**:
   - Go to [Firebase Console](https://console.firebase.google.com/)
   - Click "Add Project"
   - Follow the setup wizard

2. **Set Up Realtime Database**:
   - In your Firebase project, go to "Realtime Database"
   - Click "Create Database"
   - Choose "Start in test mode" (update rules later for production)
   - Note your database URL: `https://YOUR-PROJECT-ID-default-rtdb.firebaseio.com`

3. **Configure Database Rules**:
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
   > ⚠️ **Important**: These are test rules. Update for production!

4. **Enable Authentication**:
   - Go to "Authentication" → "Sign-in method"
   - Enable "Email/Password"
   - Add an admin user:
     - Go to "Users" tab
     - Click "Add user"
     - Enter email and password for admin access

### Step 2: Update Firebase URLs

1. **Update Admin Dashboard** (`admin-dashboard.html`):
   ```javascript
   // Line ~450
   const firebaseConfig = {
       databaseURL: "https://YOUR-PROJECT-ID-default-rtdb.firebaseio.com"
   };
   ```

2. **Update User Panel** (`user-panel.html`):
   ```javascript
   // Line ~370
   const firebaseConfig = {
       databaseURL: "https://YOUR-PROJECT-ID-default-rtdb.firebaseio.com"
   };
   ```

### Step 3: Configure UPI Payment (User Panel Only)

Update the UPI ID in `user-panel.html`:

```javascript
// Line ~520
const upiId = 'YOUR-UPI-ID@okaxis';  // Replace with your UPI ID
const upiName = 'Smart Parking System';
```

### Step 4: Deploy Files

**Option A: Local Testing**
1. Open files directly in a web browser
2. Both pages will work with Firebase online

**Option B: Web Hosting**
1. Upload both HTML files to your web server
2. Access via your domain

**Option C: Firebase Hosting**
```bash
# Install Firebase CLI
npm install -g firebase-tools

# Login to Firebase
firebase login

# Initialize Firebase in your project folder
firebase init hosting

# Deploy
firebase deploy
```

## 📱 Usage Guide

### For Administrators

1. **Access Admin Dashboard**:
   - Open `admin-dashboard.html`
   - Login with Firebase admin credentials

2. **View Dashboard**:
   - See real-time statistics
   - Monitor currently parked vehicles
   - Track revenue

3. **Search Records**:
   - Use search box to filter by plate number
   - View complete parking history

4. **Export Reports**:
   - Click "Export PDF" to generate reports
   - PDF includes all parking records

### For Vehicle Owners

1. **Access User Panel**:
   - Open `user-panel.html`
   - No login required

2. **Check Vehicle Status**:
   - Enter vehicle number plate
   - Click "Search"

3. **View Information**:
   - Entry time
   - Exit time (if exited)
   - Duration
   - Amount to pay

4. **Make Payment** (if applicable):
   - Scan QR code with UPI app
   - Payment link includes amount and reference

## 💰 Pricing Logic

The system implements automatic pricing:

- **First 30 minutes**: FREE
- **After 30 minutes**: ₹20 per hour (or part thereof)

**Examples**:
- 15 minutes → ₹0
- 30 minutes → ₹0
- 45 minutes → ₹20 (1 hour charged)
- 90 minutes → ₹40 (2 hours charged)
- 150 minutes → ₹60 (3 hours charged)

## 🔄 Entry/Exit Logic

The system automatically determines entry and exit:

1. **First Detection**: Marked as ENTRY
2. **Second Detection**: Marked as EXIT
3. **Duration**: Calculated between entry and exit
4. **Amount**: Calculated based on duration

**Example Flow**:
```
Vehicle TS15EL5671 detected at 10:00 AM → ENTRY
Vehicle TS15EL5671 detected at 11:30 AM → EXIT
Duration: 1h 30m
Amount: ₹40 (first 30 min free, then 2 hours @ ₹20/hr)
```

## 🔒 Security Considerations

### Current Setup (Development)
- Database rules allow public read/write
- Good for testing with ESP32-CAM

### Production Recommendations
1. **Update Firebase Rules**:
   ```json
   {
     "rules": {
       "numberplate": {
         ".read": "auth != null",
         ".write": "auth != null"
       }
     }
   }
   ```

2. **Secure ESP32-CAM**:
   - Use Firebase Admin SDK
   - Store credentials securely
   - Implement authentication token

3. **Admin Authentication**:
   - Use strong passwords
   - Enable 2FA if available
   - Limit admin users

4. **HTTPS**:
   - Deploy on HTTPS domain
   - Ensure all Firebase connections are secure

## 🐛 Troubleshooting

### Admin Can't Login
- Verify Firebase Authentication is enabled
- Check user exists in Firebase Console
- Confirm email/password are correct
- Check browser console for errors

### No Data Showing
- Verify Firebase URL is correct
- Check Firebase rules allow read access
- Ensure data exists in database
- Check for NULL entries (filtered automatically)

### QR Code Not Generating
- Verify QRCode.js library loaded
- Check UPI ID is configured
- Ensure amount is greater than 0
- Check browser console for errors

### ESP32-CAM Integration Issues
- Verify Firebase URL in ESP32 code
- Check WiFi connectivity
- Ensure JSON format is correct
- Verify date_time format: "YYYY-MM-DD HH:MM:SS"

## 📊 Data Structure

### Firebase Database Structure
```json
{
  "numberplate": {
    "-UniqueID1": {
      "date_time": "2026-01-29 10:00:00",
      "number_plate": "TS15EL5671"
    },
    "-UniqueID2": {
      "date_time": "2026-01-29 10:15:30",
      "number_plate": "NULL"  // Automatically filtered
    },
    "-UniqueID3": {
      "date_time": "2026-01-29 11:30:00",
      "number_plate": "TS15EL5671"
    }
  }
}
```

## 🔧 Customization

### Change Color Theme
Edit CSS variables in both HTML files:

```css
:root {
    --yellow: #FFD700;        /* Main accent color */
    --black: #0A0A0A;         /* Background */
    --black-light: #1A1A1A;   /* Card background */
    /* ... more colors ... */
}
```

### Modify Pricing
Update `calculateAmount` function in both files:

```javascript
function calculateAmount(duration) {
    if (!duration) return 0;
    
    const totalMinutes = duration.totalMinutes;
    
    // Customize these values:
    const freeMinutes = 30;      // Free time
    const hourlyRate = 20;       // Rate per hour
    
    if (totalMinutes <= freeMinutes) return 0;
    
    const chargeableMinutes = totalMinutes - freeMinutes;
    const hours = Math.ceil(chargeableMinutes / 60);
    
    return hours * hourlyRate;
}
```

### Add More Statistics
In `admin-dashboard.html`, add new stat cards:

```html
<div class="stat-card">
    <div class="stat-label">Your Label</div>
    <div class="stat-value" id="yourValue">0</div>
</div>
```

## 📝 File Structure

```
smart-parking-system/
│
├── admin-dashboard.html      # Admin interface with full access
├── user-panel.html           # User interface for vehicle owners
└── README.md                 # This file
```

## 🌐 Browser Compatibility

- ✅ Chrome (Recommended)
- ✅ Firefox
- ✅ Safari
- ✅ Edge
- ✅ Mobile browsers (iOS/Android)

## 📞 Support

### Common Questions

**Q: Can multiple admins access the dashboard?**
A: Yes, create multiple users in Firebase Authentication.

**Q: How to handle vehicle re-entry?**
A: The system automatically treats it as a new parking session after exit.

**Q: Can I customize the free parking duration?**
A: Yes, modify the `calculateAmount` function (see Customization section).

**Q: How to integrate with payment gateway?**
A: Replace QR code generation with payment gateway API integration.

## 🚀 Future Enhancements

Potential features to add:
- SMS/Email notifications
- Monthly parking passes
- Reserved parking slots
- Integration with payment gateways
- Mobile app (React Native/Flutter)
- License plate recognition improvements
- Multi-location support
- Advanced analytics and reporting

## 📄 License

This project is provided as-is for educational and commercial use.

## 🙏 Credits

- **Firebase**: Backend and authentication
- **QRCode.js**: QR code generation
- **jsPDF**: PDF export functionality
- **Google Fonts**: Syne and Space Mono typefaces

---

**Made with ⚡ for Smart City Parking Solutions**

For questions or support, please refer to the troubleshooting section or check Firebase documentation.
