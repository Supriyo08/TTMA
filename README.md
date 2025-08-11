```markdown
# Tabla Tuition Management App

A comprehensive Flutter application designed for managing tabla music tuition classes, student payments, and administrative tasks. This app provides separate interfaces for students and administrators with real-time payment tracking and automated notification systems.

## 📱 Features

### Student Panel
- **Secure Authentication** - Firebase-based login system
- **Payment Gateway Integration** - Razorpay payment processing
- **Payment History** - Complete transaction records with timestamps
- **Real-time Payment Status** - Active/inactive subscription tracking
- **Push Notifications** - Automated payment reminders

### Admin Panel
- **Student Management** - Add, edit, and search students by name or ID
- **Payment Tracking** - Monitor all student payments and payment history
- **Dynamic Fee Management** - Adjust tuition amounts and discount structures
- **Notification Control** - Send custom notifications and automated reminders
- **Analytics Dashboard** - Revenue tracking and payment insights
- **Overdue Payment Alerts** - Automatic notifications for students with 1+ month overdue payments

## 🛠️ Technology Stack

- **Frontend:** Flutter (Dart)
- **Backend:** Firebase (Firestore, Authentication, Cloud Functions)
- **Payment Gateway:** Razorpay
- **Push Notifications:** Firebase Cloud Messaging (FCM)
- **State Management:** Provider Pattern
- **Database:** Cloud Firestore
- **Authentication:** Firebase Auth

## 📋 Prerequisites

Before running this application, ensure you have the following installed:

- [Flutter SDK](https://flutter.dev/docs/get-started/install) (>=3.0.0)
- [Dart SDK](https://dart.dev/get-dart) (>=3.0.0)
- [Android Studio](https://developer.android.com/studio) or [VS Code](https://code.visualstudio.com/)
- [Firebase CLI](https://firebase.google.com/docs/cli)
- Android/iOS development environment setup

## 🚀 Installation

### 1. Clone the Repository
```

git clone https://github.com/yourusername/tabla-tuition-app.git
cd tabla-tuition-app

```

### 2. Install Dependencies
```

flutter pub get

```

### 3. Firebase Setup
1. Create a new Firebase project at [Firebase Console](https://console.firebase.google.com/)
2. Add Android/iOS apps to your Firebase project
3. Download `google-services.json` (Android) and `GoogleService-Info.plist` (iOS)
4. Place the configuration files in the appropriate directories:
   - Android: `android/app/google-services.json`
   - iOS: `ios/Runner/GoogleService-Info.plist`

### 4. Configure Firebase Services
Enable the following Firebase services:
- **Authentication** (Email/Password)
- **Cloud Firestore**
- **Cloud Messaging**
- **Cloud Functions** (optional for automated notifications)

### 5. Razorpay Setup
1. Create a Razorpay account at [Razorpay Dashboard](https://dashboard.razorpay.com/)
2. Get your API keys (Key ID and Key Secret)
3. Update the Razorpay configuration in the payment screen

### 6. Run the Application
```

flutter run

```

## 📁 Project Structure

```

lib/
├── main.dart                          \# App entry point
├── screens/
│   ├── splash_screen.dart             \# App splash screen
│   ├── login_screen.dart              \# Authentication screen
│   ├── student/
│   │   ├── student_dashboard.dart     \# Student main dashboard
│   │   ├── payment_screen.dart        \# Payment processing
│   │   ├── payment_history_screen.dart\# Payment history view
│   │   └── profile_screen.dart        \# Student profile
│   └── admin/
│       ├── admin_dashboard.dart       \# Admin main dashboard
│       ├── students_list_screen.dart  \# Student management
│       ├── student_details_screen.dart\# Individual student details
│       ├── payment_management_screen.dart \# Fee management
│       ├── notifications_screen.dart  \# Notification center
│       └── analytics_screen.dart      \# Revenue analytics
├── providers/
│   ├── auth_provider.dart             \# Authentication state management
│   ├── student_provider.dart          \# Student data management
│   └── payment_provider.dart          \# Payment state management
├── models/
│   ├── student.dart                   \# Student data model
│   └── payment.dart                   \# Payment data model
└── services/
├── firebase_service.dart          \# Firebase operations
├── payment_service.dart           \# Payment processing
└── notification_service.dart      \# Push notification handling

```

## 🗄️ Database Schema

### Students Collection
```

{
"studentId": "unique_id",
"name": "Student Name",
"email": "student@email.com",
"phone": "+91xxxxxxxxxx",
"registrationDate": "timestamp",
"lastPaymentDate": "timestamp",
"isActive": true
}

```

### Payments Collection
```

{
"paymentId": "unique_payment_id",
"studentId": "student_reference",
"amount": 2000,
"paymentDate": "timestamp",
"paymentMode": "razorpay/cash/upi",
"status": "completed",
"monthsCovered": 1
}

```

### Settings Collection
```

{
"monthlyFee": 2000,
"twoMonthDiscount": 5,
"autoReminders": true,
"reminderDays": 7
}

```

## 🔧 Configuration

### Environment Variables
Create a `.env` file in the root directory:
```

RAZORPAY_KEY_ID=your_razorpay_key_id
RAZORPAY_KEY_SECRET=your_razorpay_key_secret
FIREBASE_PROJECT_ID=your_firebase_project_id

```

### Firebase Security Rules
Update Firestore security rules:
```

rules_version = '2';
service cloud.firestore {
match /databases/{database}/documents {
// Students can read/write their own data
match /students/{studentId} {
allow read, write: if request.auth != null \&\&
(request.auth.uid == studentId ||
get(/databases/$(database)/documents/users/$(request.auth.uid)).data.role == 'admin');
}

    // Payments - students can read their own, admin can read/write all
    match /payments/{paymentId} {
      allow read, write: if request.auth != null && 
        (resource.data.studentId == request.auth.uid || 
         get(/databases/$(database)/documents/users/$(request.auth.uid)).data.role == 'admin');
    }
    
    // Settings - admin only
    match /settings/{document=**} {
      allow read, write: if request.auth != null && 
        get(/databases/$(database)/documents/users/$(request.auth.uid)).data.role == 'admin';
    }
    }
}

```

## 📱 App Store Deployment

### Android (Google Play Store)
1. Generate a signed APK:
```

flutter build apk --release

```

2. Create app bundle for Play Store:
```

flutter build appbundle --release

```

3. Upload to [Google Play Console](https://play.google.com/console/)

### iOS (Apple App Store)
1. Build for iOS:
```

flutter build ios --release

```

2. Archive and upload via Xcode to [App Store Connect](https://appstoreconnect.apple.com/)

## 🧪 Testing

### Run Unit Tests
```

flutter test

```

### Run Integration Tests
```

flutter drive --target=test_driver/app.dart

```

## 📊 Analytics & Monitoring

### Firebase Analytics
Track user engagement and app performance:
- Student login frequency
- Payment completion rates
- Feature usage statistics
- Crash reporting

### Performance Monitoring
Monitor app performance with Firebase Performance:
- App startup time
- Screen loading times
- API response times

## 🔐 Security Features

- **Role-based Access Control** - Separate student and admin permissions
- **Data Encryption** - Secure payment information storage
- **Input Validation** - Prevent SQL injection and data tampering
- **Secure Authentication** - Firebase Auth with email verification
- **Payment Security** - PCI-compliant payment processing via Razorpay

## 🚀 Future Enhancements

- [ ] **Multi-teacher Support** - Extend for multiple instructors
- [ ] **Class Scheduling** - Lesson booking system
- [ ] **Progress Tracking** - Student learning progress
- [ ] **Video Lessons** - Recorded lesson integration
- [ ] **Referral System** - Student referral rewards
- [ ] **Offline Mode** - Basic functionality without internet
- [ ] **Dark Theme** - UI theme customization
- [ ] **Multi-language Support** - Localization

## 🐛 Known Issues

- Payment gateway sandbox mode for testing
- Push notifications require device registration
- iOS build requires Apple Developer account

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 📞 Support

For support and queries:
- Email: support@tablaturion.com
- GitHub Issues: [Create an issue](https://github.com/yourusername/tabla-tuition-app/issues)
- Documentation: [Wiki](https://github.com/yourusername/tabla-tuition-app/wiki)

## 👥 Authors

- **Your Name** - *Initial work* - [YourGitHub](https://github.com/yourusername)

## 🙏 Acknowledgments

- Flutter team for the amazing framework
- Firebase for backend services
- Razorpay for payment gateway integration
- Material Design for UI/UX guidelines

---

**Version:** 1.0.0  
**Last Updated:** December 2024  
**Maintained by:** [Your Name/Organization]
```
