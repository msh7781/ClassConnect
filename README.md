# 🎓 Assignment Portal

A modern, full-featured assignment management system built with React, TypeScript, Chakra UI, and Firebase. This application enables teachers to create and manage assignments while allowing students to submit their work with multimedia support.

![Assignment Portal Preview](https://img.shields.io/badge/Status-Production%20Ready-brightgreen)
![React](https://img.shields.io/badge/React-18.0+-blue)
![TypeScript](https://img.shields.io/badge/TypeScript-5.0+-blue)
![Firebase](https://img.shields.io/badge/Firebase-10.0+-orange)

## 🌟 Features

### For Teachers
- ✅ **Assignment Management** - Create, edit, and delete assignments
- ✅ **Multimedia Review** - View student submissions with text and images
- ✅ **Grading System** - Grade submissions with numerical scores and feedback
- ✅ **Student Analytics** - Track submission status and student progress
- ✅ **Real-time Dashboard** - Live updates on assignment submissions

### For Students
- ✅ **Assignment Viewing** - Browse all available assignments
- ✅ **Multimedia Submissions** - Submit text responses with up to 5 images
- ✅ **Submission Management** - Update existing submissions before due dates
- ✅ **Grade Tracking** - View grades and teacher feedback
- ✅ **Progress Monitoring** - Track submission status

### Technical Features
- 🔐 **Firebase Authentication** - Secure user registration and login
- 🗄️ **Firestore Database** - Real-time data synchronization
- 📱 **Responsive Design** - Works on desktop, tablet, and mobile
- 🎨 **Modern UI/UX** - Sleek interface with smooth animations
- 🖼️ **Image Upload** - Base64 image storage and display
- 🔒 **Role-based Access** - Separate interfaces for teachers and students

## 🚀 Quick Start

### Prerequisites
- Node.js 18.0 or higher
- npm or pnpm package manager
- Firebase project with Firestore enabled

### 1. Clone the Repository
```bash
git clone <repository-url>
cd AssignmentPortal-New
```

### 2. Install Dependencies
```bash
npm install
# or
pnpm install
```

### 3. Firebase Setup

#### A. Create Firebase Project
1. Go to [Firebase Console](https://console.firebase.google.com/)
2. Click "Create a project"
3. Follow the setup wizard
4. Enable **Authentication** and **Firestore Database**

#### B. Get Firebase Configuration
1. In your Firebase project, click the gear icon → "Project settings"
2. Scroll down to "Your apps" section
3. Click the web icon (`</>`) to add a web app
4. Register your app with a name
5. Copy the configuration object

#### C. Environment Variables
Create a `.env` file in the root directory:
```env
VITE_FIREBASE_API_KEY=your_api_key_here
VITE_FIREBASE_AUTH_DOMAIN=your_project_id.firebaseapp.com
VITE_FIREBASE_PROJECT_ID=your_project_id
VITE_FIREBASE_STORAGE_BUCKET=your_project_id.appspot.com
VITE_FIREBASE_MESSAGING_SENDER_ID=your_sender_id
VITE_FIREBASE_APP_ID=your_app_id
VITE_FIREBASE_MEASUREMENT_ID=your_measurement_id
```

### 4. Firebase Authentication Setup

#### Enable Authentication Methods
1. In Firebase Console, go to **Authentication** → **Sign-in method**
2. Enable **Email/Password** provider
3. Click "Save"

### 5. Firestore Database Setup

#### A. Create Database
1. In Firebase Console, go to **Firestore Database**
2. Click "Create database"
3. Choose "Start in test mode" (we'll configure rules later)
4. Select a location closest to your users

#### B. Security Rules
Replace the default Firestore rules with:
```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // Users can read/write their own user document
    match /users/{userId} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
      allow read: if request.auth != null; // Allow reading other users for student names
    }
    
    // Assignments - Teachers can CRUD, Students can read
    match /assignments/{assignmentId} {
      allow read: if request.auth != null;
      allow create, update, delete: if request.auth != null && 
        get(/databases/$(database)/documents/users/$(request.auth.uid)).data.role == 'teacher';
    }
    
    // Submissions - Students can CRUD their own, Teachers can read/update for grading
    match /submissions/{submissionId} {
      allow read, update: if request.auth != null && (
        resource.data.studentId == request.auth.uid ||
        get(/databases/$(database)/documents/users/$(request.auth.uid)).data.role == 'teacher'
      );
      allow create: if request.auth != null && 
        request.auth.uid == request.resource.data.studentId;
    }
  }
}
```

#### C. Create Firestore Indexes
In Firebase Console, go to **Firestore Database** → **Indexes** → **Composite** and create these indexes:

**Index 1: assignments-createdBy-createdAt**
- Collection ID: `assignments`
- Fields:
  - `createdBy` (Ascending)
  - `createdAt` (Descending)

**Index 2: submissions-assignmentId-submittedAt**
- Collection ID: `submissions`
- Fields:
  - `assignmentId` (Ascending)
  - `submittedAt` (Descending)

**Index 3: submissions-studentId-submittedAt**
- Collection ID: `submissions`
- Fields:
  - `studentId` (Ascending)
  - `submittedAt` (Descending)

**Index 4: assignments-dueDate**
- Collection ID: `assignments`
- Fields:
  - `dueDate` (Ascending)

### 6. Run the Application

#### Development Mode
```bash
npm run dev
# or
pnpm run dev
```
The app will be available at `http://localhost:5173`

#### Production Build
```bash
npm run build
npm run preview
# or
pnpm run build
pnpm run preview
```

## 👥 Demo Accounts

For testing purposes, create these accounts manually:

### Teacher Account
- **Email**: `teacher@example.com`
- **Password**: `password123`
- **Role**: `teacher`

### Student Account
- **Email**: `student@example.com`
- **Password**: `password123`
- **Role**: `student`

**Note**: When registering, make sure to select the correct role during signup.

## 📱 Usage Guide

### For Teachers

1. **Login** with teacher credentials
2. **Create Assignment**:
   - Click "Create Assignment" button
   - Fill in title, description, due date, and total points
   - Add requirements (optional)
   - Click "Create Assignment"

3. **View Submissions**:
   - Go to "Submissions" tab
   - Click "Grade" or "View" on any submission
   - Review student's text and images
   - Assign grade and provide feedback

4. **Manage Assignments**:
   - View all your assignments in the "Assignments" tab
   - See submission statistics
   - Delete assignments if needed

### For Students

1. **Login** with student credentials
2. **View Assignments**:
   - See all available assignments on the dashboard
   - Check due dates and requirements

3. **Submit Assignment**:
   - Click "Submit Assignment" on any assignment card
   - Write your response in the text area
   - Upload images (up to 5) using "Choose Images" button
   - Click "Submit Assignment"

4. **Update Submission**:
   - Click "Update Submission" on previously submitted assignments
   - Modify text or add/remove images
   - Click "Update Submission"

5. **View Grades**:
   - Graded submissions show the score and teacher feedback
   - Green badge indicates graded assignments

## 🛠️ Technology Stack

- **Frontend**: React 18, TypeScript, Vite
- **UI Library**: Chakra UI
- **Backend**: Firebase (Firestore, Authentication)
- **Styling**: Custom CSS with Chakra UI theme
- **Build Tool**: Vite
- **Package Manager**: npm/pnpm

## 🔧 Configuration

### Firestore Collection Structure

#### `users` Collection
```javascript
{
  id: string,           // Auto-generated document ID
  name: string,         // User's full name
  email: string,        // User's email address
  role: 'teacher' | 'student',  // User role
  createdAt: Timestamp  // Account creation date
}
```

#### `assignments` Collection
```javascript
{
  id: string,           // Auto-generated document ID
  title: string,        // Assignment title
  description: string,  // Assignment description
  dueDate: string,      // Due date (ISO string)
  totalPoints: number,  // Maximum points for assignment
  requirements: string[], // Array of requirements (optional)
  createdBy: string,    // Teacher's user ID
  createdAt: Timestamp  // Assignment creation date
}
```

#### `submissions` Collection
```javascript
{
  id: string,           // Auto-generated document ID
  studentId: string,    // Student's user ID
  assignmentId: string, // Reference to assignment
  content: string,      // Student's text response
  images: string[],     // Array of base64 image strings
  submittedAt: Timestamp, // Submission date
  status: 'submitted' | 'graded', // Submission status
  grade: number,        // Grade assigned by teacher (optional)
  feedback: string      // Teacher's feedback (optional)
}
```

## 🚀 Deployment

### Firebase Hosting (Recommended)

1. **Install Firebase CLI**:
```bash
npm install -g firebase-tools
```

2. **Login to Firebase**:
```bash
firebase login
```

3. **Initialize Hosting**:
```bash
firebase init hosting
```

4. **Build the Project**:
```bash
npm run build
```

5. **Deploy**:
```bash
firebase deploy
```

### Other Deployment Options
- **Vercel**: Connect your GitHub repository
- **Netlify**: Drag and drop the `dist` folder
- **GitHub Pages**: Use GitHub Actions for automatic deployment

## 🔒 Security Best Practices

1. **Environment Variables**: Never commit `.env` files to version control
2. **Firestore Rules**: Always use proper security rules in production
3. **Authentication**: Implement proper session management
4. **Image Storage**: Consider using Firebase Storage for large images
5. **Rate Limiting**: Implement rate limiting for API calls

## 🐛 Troubleshooting

### Common Issues

**Build Errors**
```bash
# Clear node_modules and reinstall
rm -rf node_modules package-lock.json
npm install
```

**Firebase Connection Issues**
- Verify all environment variables are set correctly
- Check Firebase project settings match your configuration
- Ensure Firestore database is created and accessible

**Authentication Problems**
- Verify Email/Password provider is enabled in Firebase Console
- Check that user roles are set correctly during registration
- Ensure Firestore security rules allow user document creation

**Index Errors**
- Create required Firestore indexes as listed in setup section
- Wait for indexes to build (can take several minutes)

### Performance Optimization

1. **Bundle Size**: Use dynamic imports for large components
2. **Image Optimization**: Compress images before upload
3. **Caching**: Implement proper caching strategies
4. **Lazy Loading**: Load assignments and submissions on demand

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/amazing-feature`
3. Commit changes: `git commit -m 'Add amazing feature'`
4. Push to branch: `git push origin feature/amazing-feature`
5. Open a Pull Request

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🆘 Support

If you encounter any issues or have questions:

1. Check the troubleshooting section above
2. Review Firebase documentation
3. Create an issue in the repository
4. Contact the development team

## 🔄 Version History

- **v1.0.0** - Initial release with basic assignment management
- **v1.1.0** - Added image upload functionality
- **v1.2.0** - Enhanced UI/UX with modern design
- **v1.3.0** - Improved responsive design and mobile support

---

**Built with ❤️ by the Assignment Portal Team**

For more information, visit our [documentation](docs/) or check out the [API reference](docs/api.md).