# Line-by-Line Code Explanation

## 1. src/main.tsx - Application Entry Point

```typescript
import React from 'react'
```
**Line 1**: Imports React library. React is needed for JSX (HTML-like syntax in JavaScript).

```typescript
import ReactDOM from 'react-dom/client'
```
**Line 2**: Imports ReactDOM from 'react-dom/client'. ReactDOM handles rendering React components into the browser's DOM (Document Object Model - the webpage structure).

```typescript
import { ChakraProvider } from '@chakra-ui/react'
```
**Line 3**: Imports ChakraProvider component from Chakra UI library. This wraps the app to provide Chakra UI styling/theming functionality to all components.

```typescript
import { BrowserRouter } from 'react-router-dom'
```
**Line 4**: Imports BrowserRouter from React Router. This enables client-side routing (navigation between pages without page reloads).

```typescript
import App from './App'
```
**Line 5**: Imports the main App component from the App.tsx file in the same directory.

```typescript
import './index.css'
```
**Line 6**: Imports global CSS styles from index.css file.

```typescript
ReactDOM.createRoot(document.getElementById('root')!).render(
```
**Line 8**: 
- `document.getElementById('root')` - Finds the HTML element with id="root" (from index.html)
- `!` - TypeScript non-null assertion operator (tells TypeScript "I'm sure this element exists")
- `ReactDOM.createRoot()` - Creates a root for React 18's concurrent rendering
- `.render()` - Renders the React component tree into that root element

```typescript
  <React.StrictMode>
```
**Line 9**: React.StrictMode is a development tool that:
- Identifies potential problems
- Warns about deprecated features
- Helps catch bugs early

```typescript
    <BrowserRouter>
```
**Line 10**: Wraps the app with BrowserRouter to enable routing functionality.

```typescript
      <ChakraProvider>
```
**Line 11**: Wraps the app with ChakraProvider to enable Chakra UI components and styling.

```typescript
        <App />
```
**Line 12**: Renders the main App component (the root of your application).

```typescript
      </ChakraProvider>
    </BrowserRouter>
  </React.StrictMode>
)
```
**Lines 13-15**: Closes all the wrapper components and the render call.

---

## 2. src/App.tsx - Main Application Component

```typescript
import { useEffect, useState } from 'react';
```
**Line 1**: Imports React hooks:
- `useEffect`: Runs code after component renders (like on page load)
- `useState`: Manages component state (data that can change)

```typescript
import { Routes, Route, Navigate, useLocation } from 'react-router-dom';
```
**Line 2**: Imports routing components:
- `Routes`: Container for all route definitions
- `Route`: Defines a single route/path
- `Navigate`: Programmatically redirects to another route
- `useLocation`: Hook to get current URL location

```typescript
import { Box, Container } from '@chakra-ui/react';
```
**Line 3**: Imports Chakra UI layout components:
- `Box`: Generic container/div
- `Container`: Centered container with max-width

```typescript
import { AuthProvider, useAuth } from './contexts/AuthContext';
```
**Line 4**: Imports authentication context:
- `AuthProvider`: Wrapper component that provides auth state to all children
- `useAuth`: Hook to access authentication data/functions

```typescript
// Components
import Navbar from './components/Navbar';
```
**Line 7**: Imports the navigation bar component.

```typescript
import { PrivateRoute } from './components/PrivateRoute';
```
**Line 8**: Imports PrivateRoute component (protects routes that require login).

```typescript
// Pages
import Home from './pages/Home';
import Login from './pages/Login';
import Register from './pages/Register';
import StudentDashboard from './pages/StudentDashboard';
import TeacherDashboard from './pages/TeacherDashboard';
import Landing from './pages/Landing';
```
**Lines 11-16**: Imports all page components.

```typescript
function AppRoutes() {
```
**Line 18**: Defines a function component that handles routing logic.

```typescript
  const { currentUser, loading } = useAuth();
```
**Line 19**: Gets current user and loading state from auth context:
- `currentUser`: User object if logged in, null if not
- `loading`: Boolean indicating if auth state is still being checked

```typescript
  const location = useLocation();
```
**Line 20**: Gets current URL location (for redirects).

```typescript
  // Determine default redirect path based on user role
  const getDefaultPath = () => {
```
**Lines 22-23**: Defines a function that returns the default route based on user status.

```typescript
    if (!currentUser) return '/login';
```
**Line 24**: If no user is logged in, redirect to login page.

```typescript
    return currentUser.role === 'teacher' ? '/teacher-dashboard' : '/student-dashboard';
```
**Line 25**: If user is logged in, redirect to their role-specific dashboard.

```typescript
  };
```
**Line 26**: Closes the getDefaultPath function.

```typescript
  return (
    <Routes>
```
**Lines 28-29**: Returns JSX with Routes container for all route definitions.

```typescript
      
      <Route path="/" element={<Landing />} />
```
**Line 31**: Defines route for home page ("/") - shows Landing page component.

```typescript
      <Route path="/login" element={<Login />} />
```
**Line 32**: Defines route for "/login" - shows Login page.

```typescript
      <Route path="/register" element={<Register />} />
```
**Line 33**: Defines route for "/register" - shows Register page.

```typescript
      
      {/* Student routes */}
      <Route 
        path="/student-dashboard" 
        element={
          <PrivateRoute requiredRole="student">
            <StudentDashboard />
          </PrivateRoute>
        } 
      />
```
**Lines 35-43**: Defines student dashboard route:
- Wrapped in PrivateRoute to require authentication
- `requiredRole="student"` ensures only students can access
- Shows StudentDashboard component if authorized

```typescript
      
      {/* Teacher routes */}
      <Route 
        path="/teacher-dashboard" 
        element={
          <PrivateRoute requiredRole="teacher">
            <TeacherDashboard />
          </PrivateRoute>
        } 
      />
```
**Lines 45-53**: Same as student route but for teachers.

```typescript
      
      {/* Redirect based on role */}
      <Route 
        path="*" 
        element={<Navigate to={getDefaultPath()} state={{ from: location }} replace />} 
      />
```
**Lines 55-59**: Catch-all route ("*" matches any unmatched path):
- Redirects to default path based on user role
- `state={{ from: location }}` saves where they came from
- `replace` replaces history entry instead of adding new one

```typescript
    </Routes>
  );
}
```
**Lines 60-62**: Closes Routes and AppRoutes function.

```typescript
function App() {
```
**Line 64**: Defines main App component.

```typescript
  return (
    <AuthProvider>
```
**Lines 65-66**: Wraps app with AuthProvider so all components can access auth state.

```typescript
      <Box minH="100vh"  display="flex" flexDirection="column">
```
**Line 67**: Creates a Box container:
- `minH="100vh"`: Minimum height is 100% of viewport height
- `display="flex"`: Uses flexbox layout
- `flexDirection="column"`: Stacks children vertically

```typescript
        <Navbar />
```
**Line 68**: Renders the navigation bar at the top.

```typescript
        <Box flex="1" py={6}>
```
**Line 69**: Creates another Box:
- `flex="1"`: Takes up remaining vertical space
- `py={6}`: Adds padding top and bottom (6 * 4px = 24px in Chakra)

```typescript
          <Container maxW="container">
```
**Line 70**: Container with max-width constraint (centers content).

```typescript
            <AppRoutes />
```
**Line 71**: Renders the AppRoutes component (all the routing logic).

```typescript
          </Container>
        </Box>
      </Box>
    </AuthProvider>
  );
}
```
**Lines 72-76**: Closes all components and the App function.

```typescript
export default App;
```
**Line 79**: Exports App component so it can be imported elsewhere (like in main.tsx).

---

## 3. src/contexts/AuthContext.tsx - Authentication Context

```typescript
import { createContext, useContext, useState, useEffect, ReactNode } from 'react';
```
**Line 1**: Imports React hooks and types:
- `createContext`: Creates a context for sharing data
- `useContext`: Hook to access context data
- `useState`: Manages component state
- `useEffect`: Runs code after render
- `ReactNode`: TypeScript type for React children

```typescript
import { auth, authService } from '../services/firebase';
```
**Line 2**: Imports Firebase auth instance and auth service functions.

```typescript
import { onAuthStateChanged } from 'firebase/auth';
```
**Line 3**: Imports Firebase function that listens for auth state changes (login/logout).

```typescript
import { User } from '../types';
```
**Line 4**: Imports User type definition.

```typescript
interface AuthContextType {
```
**Line 6**: Defines TypeScript interface (shape of data) for auth context.

```typescript
  currentUser: User | null;
```
**Line 7**: Property: current user object or null if not logged in.

```typescript
  loading: boolean;
```
**Line 8**: Property: true if checking auth state, false when done.

```typescript
  login: (email: string, password: string) => Promise<any>;
```
**Line 9**: Function: login method that takes email/password, returns Promise.

```typescript
  register: (email: string, password: string, name: string, role: 'student' | 'teacher') => Promise<any>;
```
**Line 10**: Function: register method with all required parameters.

```typescript
  logout: () => Promise<void>;
```
**Line 11**: Function: logout method that returns Promise with no value.

```typescript
}
```
**Line 12**: Closes interface.

```typescript
const AuthContext = createContext<AuthContextType | undefined>(undefined);
```
**Line 14**: Creates React context:
- `createContext<AuthContextType | undefined>`: Creates context with TypeScript type
- `undefined`: Default value (will be set by Provider)

```typescript
export function useAuth() {
```
**Line 16**: Exports custom hook to access auth context.

```typescript
  const context = useContext(AuthContext);
```
**Line 17**: Gets context value using useContext hook.

```typescript
  if (!context) {
```
**Line 18**: Checks if context exists (must be used inside AuthProvider).

```typescript
    throw new Error('useAuth must be used within an AuthProvider');
```
**Line 19**: Throws error if hook used outside Provider (prevents bugs).

```typescript
  }
```
**Line 20**: Closes if statement.

```typescript
  return context;
```
**Line 21**: Returns the context value (auth data and functions).

```typescript
}
```
**Line 22**: Closes useAuth function.

```typescript
interface AuthProviderProps {
```
**Line 24**: Defines props interface for AuthProvider component.

```typescript
  children: ReactNode;
```
**Line 25**: Property: children components that AuthProvider will wrap.

```typescript
}
```
**Line 26**: Closes interface.

```typescript
export function AuthProvider({ children }: AuthProviderProps) {
```
**Line 28**: Exports AuthProvider component:
- `{ children }`: Destructures children from props
- `: AuthProviderProps`: TypeScript type annotation

```typescript
  const [currentUser, setCurrentUser] = useState<User | null>(null);
```
**Line 29**: Creates state variable:
- `currentUser`: Current value (starts as null)
- `setCurrentUser`: Function to update the value
- `useState<User | null>(null)`: Initializes with null

```typescript
  const [loading, setLoading] = useState(true);
```
**Line 30**: Creates loading state (starts as true, will be false when auth check completes).

```typescript
  useEffect(() => {
```
**Line 32**: Runs code after component mounts (when component first appears).

```typescript
    const unsubscribe = onAuthStateChanged(auth, async (firebaseUser) => {
```
**Line 33**: Sets up Firebase auth listener:
- `onAuthStateChanged`: Listens for login/logout events
- `auth`: Firebase auth instance
- `async (firebaseUser) => {}`: Callback function that runs when auth state changes
- `unsubscribe`: Function to stop listening (cleanup)

```typescript
      try {
```
**Line 34**: Starts try-catch block (error handling).

```typescript
        if (firebaseUser) {
```
**Line 35**: Checks if user is logged in (firebaseUser exists).

```typescript
          console.log('Firebase user detected:', firebaseUser.email);
```
**Line 36**: Logs user email to console (for debugging).

```typescript
          
          // Get user profile from Firestore
          const userProfile = await authService.getUserProfile(firebaseUser.uid);
```
**Lines 38-39**: Fetches user profile from Firestore database:
- `await`: Waits for async operation to complete
- `firebaseUser.uid`: Unique user ID from Firebase Auth
- `getUserProfile`: Service function that gets user data from database

```typescript
          console.log('User profile from Firestore:', userProfile);
```
**Line 40**: Logs user profile data (debugging).

```typescript
          
          if (userProfile) {
```
**Line 42**: Checks if profile was found in database.

```typescript
            const user = {
```
**Line 43**: Creates user object with combined data.

```typescript
              id: firebaseUser.uid,
```
**Line 44**: Sets user ID from Firebase Auth.

```typescript
              name: userProfile.name || '',
```
**Line 45**: Sets name from profile, or empty string if missing (|| is OR operator).

```typescript
              email: firebaseUser.email || '',
```
**Line 46**: Sets email from Firebase Auth, or empty string if missing.

```typescript
              role: userProfile.role as 'teacher' | 'student',
```
**Line 47**: Sets role from profile, casts to TypeScript type.

```typescript
              createdAt: userProfile.createdAt
```
**Line 48**: Sets account creation timestamp.

```typescript
            };
```
**Line 49**: Closes user object.

```typescript
            console.log('Setting current user:', user);
```
**Line 50**: Logs user object (debugging).

```typescript
            setCurrentUser(user);
```
**Line 51**: Updates currentUser state with the user object.

```typescript
          } else {
```
**Line 52**: If profile not found in database...

```typescript
            console.log('No user profile found, using default');
```
**Line 53**: Logs message (debugging).

```typescript
            // If profile not found, use basic Firebase user info
            setCurrentUser({
```
**Lines 54-55**: Creates fallback user object with minimal data.

```typescript
              id: firebaseUser.uid,
              name: firebaseUser.displayName || 'User',
              email: firebaseUser.email || '',
              role: 'student' // Default role
```
**Lines 56-59**: Sets default values when profile missing.

```typescript
            });
```
**Line 60**: Closes setCurrentUser call.

```typescript
          }
```
**Line 61**: Closes if-else block.

```typescript
        } else {
```
**Line 62**: If no Firebase user (not logged in)...

```typescript
          console.log('No Firebase user');
```
**Line 63**: Logs message.

```typescript
          setCurrentUser(null);
```
**Line 64**: Sets currentUser to null (user logged out).

```typescript
        }
```
**Line 65**: Closes if-else block.

```typescript
      } catch (error) {
```
**Line 66**: Catches any errors that occur.

```typescript
        console.error('Error fetching user profile:', error);
```
**Line 67**: Logs error to console.

```typescript
        setCurrentUser(null);
```
**Line 68**: Sets user to null on error (safe fallback).

```typescript
      } finally {
```
**Line 69**: Always runs after try-catch (regardless of success/error).

```typescript
        setLoading(false);
```
**Line 70**: Sets loading to false (auth check complete).

```typescript
      }
```
**Line 71**: Closes finally block.

```typescript
    });
```
**Line 72**: Closes onAuthStateChanged callback.

```typescript

    return () => unsubscribe();
```
**Line 74**: Cleanup function: unsubscribes from auth listener when component unmounts (prevents memory leaks).

```typescript
  }, []);
```
**Line 75**: Empty dependency array means useEffect runs only once on mount.

```typescript

  const login = (email: string, password: string) => {
```
**Line 77**: Defines login function that takes email and password.

```typescript
    return authService.login(email, password);
```
**Line 78**: Calls Firebase login service and returns the Promise.

```typescript
  };
```
**Line 79**: Closes login function.

```typescript
  const register = (email: string, password: string, name: string, role: 'student' | 'teacher') => {
```
**Line 81**: Defines register function with all required parameters.

```typescript
    return authService.register(email, password, name, role);
```
**Line 82**: Calls Firebase register service and returns Promise.

```typescript
  };
```
**Line 83**: Closes register function.

```typescript
  const logout = async () => {
```
**Line 85**: Defines async logout function.

```typescript
    await authService.logout();
```
**Line 86**: Waits for Firebase logout to complete.

```typescript
    setCurrentUser(null);
```
**Line 87**: Clears current user from state.

```typescript
  };
```
**Line 88**: Closes logout function.

```typescript
  const value = {
```
**Line 90**: Creates object with all auth data/functions to share via context.

```typescript
    currentUser,
```
**Line 91**: Includes current user state.

```typescript
    loading,
```
**Line 92**: Includes loading state.

```typescript
    login,
```
**Line 93**: Includes login function.

```typescript
    register,
```
**Line 94**: Includes register function.

```typescript
    logout
```
**Line 95**: Includes logout function.

```typescript
  };
```
**Line 96**: Closes value object.

```typescript
  return (
    <AuthContext.Provider value={value}>
```
**Lines 98-99**: Returns Provider component that shares auth data with children.

```typescript
      {children}
```
**Line 100**: Renders all child components (wrapped by this provider).

```typescript
    </AuthContext.Provider>
  );
}
```
**Lines 101-103**: Closes Provider and AuthProvider function.

---

## 4. src/services/firebase.ts - Firebase Configuration & Services

```typescript
import { initializeApp } from 'firebase/app';
```
**Line 1**: Imports function to initialize Firebase app.

```typescript
import { getAuth, onAuthStateChanged, signInWithEmailAndPassword, createUserWithEmailAndPassword, signOut, User } from 'firebase/auth';
```
**Line 2**: Imports Firebase Auth functions:
- `getAuth`: Gets auth instance
- `onAuthStateChanged`: Listens for auth changes
- `signInWithEmailAndPassword`: Logs user in
- `createUserWithEmailAndPassword`: Creates new user
- `signOut`: Logs user out
- `User`: TypeScript type for Firebase user

```typescript
import { getFirestore, collection, doc, setDoc, getDoc, getDocs, addDoc, query, where, orderBy, deleteDoc, updateDoc, serverTimestamp, DocumentData } from 'firebase/firestore';
```
**Line 3**: Imports Firestore (database) functions:
- `getFirestore`: Gets database instance
- `collection`: References a collection (like a table)
- `doc`: References a document (like a row)
- `setDoc`: Creates/updates a document
- `getDoc`: Gets a single document
- `getDocs`: Gets multiple documents
- `addDoc`: Adds new document (auto-generates ID)
- `query`: Creates database query
- `where`: Filters documents
- `orderBy`: Sorts documents
- `deleteDoc`: Deletes a document
- `updateDoc`: Updates document fields
- `serverTimestamp`: Gets server timestamp
- `DocumentData`: TypeScript type

```typescript
// Firebase configuration from environment variables
const firebaseConfig = {
```
**Lines 5-6**: Creates configuration object for Firebase.

```typescript
  apiKey: import.meta.env.VITE_FIREBASE_API_KEY,
```
**Line 7**: Gets API key from environment variable (Vite uses `import.meta.env`).

```typescript
  authDomain: import.meta.env.VITE_FIREBASE_AUTH_DOMAIN,
```
**Line 8**: Gets auth domain from environment variable.

```typescript
  projectId: import.meta.env.VITE_FIREBASE_PROJECT_ID,
```
**Line 9**: Gets project ID from environment variable.

```typescript
  storageBucket: import.meta.env.VITE_FIREBASE_STORAGE_BUCKET,
```
**Line 10**: Gets storage bucket from environment variable.

```typescript
  messagingSenderId: import.meta.env.VITE_FIREBASE_MESSAGING_SENDER_ID,
```
**Line 11**: Gets messaging sender ID from environment variable.

```typescript
  appId: import.meta.env.VITE_FIREBASE_APP_ID,
```
**Line 12**: Gets app ID from environment variable.

```typescript
  measurementId: import.meta.env.VITE_FIREBASE_MEASUREMENT_ID,
```
**Line 13**: Gets analytics measurement ID from environment variable.

```typescript
};
```
**Line 14**: Closes configuration object.

```typescript
// Initialize Firebase
const app = initializeApp(firebaseConfig);
```
**Lines 16-17**: Initializes Firebase app with configuration.

```typescript
export const auth = getAuth(app);
```
**Line 18**: Exports auth instance (used for authentication).

```typescript
export const db = getFirestore(app);
```
**Line 19**: Exports database instance (used for Firestore operations).

```typescript
// Collections
const USERS_COLLECTION = 'users';
```
**Line 22**: Defines constant for users collection name (prevents typos).

```typescript
const ASSIGNMENTS_COLLECTION = 'assignments';
```
**Line 23**: Defines constant for assignments collection name.

```typescript
const SUBMISSIONS_COLLECTION = 'submissions';
```
**Line 24**: Defines constant for submissions collection name.

```typescript
// Authentication Services
export const authService = {
```
**Lines 26-27**: Exports object containing all auth-related functions.

```typescript
  // Sign in existing user
  login: (email: string, password: string) => {
```
**Lines 28-29**: Defines login function.

```typescript
    return signInWithEmailAndPassword(auth, email, password);
```
**Line 30**: Calls Firebase function to sign in user, returns Promise.

```typescript
  },
```
**Line 31**: Closes login function.

```typescript
  // Create new user
  register: async (email: string, password: string, name: string, role: 'student' | 'teacher') => {
```
**Lines 33-34**: Defines async register function.

```typescript
    const userCredential = await createUserWithEmailAndPassword(auth, email, password);
```
**Line 35**: Creates user account in Firebase Auth, waits for completion.

```typescript
    const user = userCredential.user;
```
**Line 36**: Extracts user object from credential.

```typescript
    
    // Create user profile in Firestore
    await setDoc(doc(db, USERS_COLLECTION, user.uid), {
```
**Lines 38-39**: Creates user document in Firestore database:
- `doc(db, USERS_COLLECTION, user.uid)`: Creates document reference
- `setDoc`: Creates/updates the document

```typescript
      name,
```
**Line 40**: Saves user's name.

```typescript
      email,
```
**Line 41**: Saves user's email.

```typescript
      role,
```
**Line 42**: Saves user's role (student or teacher).

```typescript
      createdAt: serverTimestamp(),
```
**Line 43**: Saves server timestamp (when account was created).

```typescript
    });
```
**Line 44**: Closes setDoc call.

```typescript
    
    return userCredential;
```
**Line 46**: Returns user credential (for use in calling code).

```typescript
  },
```
**Line 47**: Closes register function.

```typescript
  // Sign out user
  logout: () => {
```
**Lines 49-50**: Defines logout function.

```typescript
    return signOut(auth);
```
**Line 51**: Calls Firebase signOut function.

```typescript
  },
```
**Line 52**: Closes logout function.

```typescript
  // Get current user
  getCurrentUser: () => {
```
**Lines 54-55**: Defines function to get current user.

```typescript
    return new Promise<User | null>((resolve) => {
```
**Line 56**: Returns a Promise that resolves with User or null.

```typescript
      const unsubscribe = onAuthStateChanged(auth, (user) => {
```
**Line 57**: Sets up auth state listener.

```typescript
        unsubscribe();
```
**Line 58**: Immediately unsubscribes (only want first state).

```typescript
        resolve(user);
```
**Line 59**: Resolves Promise with user (or null if not logged in).

```typescript
      });
```
**Line 60**: Closes onAuthStateChanged callback.

```typescript
    });
```
**Line 61**: Closes Promise constructor.

```typescript
  },
```
**Line 62**: Closes getCurrentUser function.

```typescript
  // Get user profile data
  getUserProfile: async (userId: string): Promise<{
```
**Lines 64-65**: Defines async function that returns Promise with specific type.

```typescript
    id: string;
    name: string;
    email: string;
    role: string;
    createdAt: any;
  } | null> => {
```
**Lines 65-71**: Defines return type (object with these properties, or null).

```typescript
    const docRef = doc(db, USERS_COLLECTION, userId);
```
**Line 72**: Creates reference to user document in Firestore.

```typescript
    const docSnap = await getDoc(docRef);
```
**Line 73**: Fetches document from database, waits for result.

```typescript
    
    if (docSnap.exists()) {
```
**Line 75**: Checks if document exists.

```typescript
      return {
```
**Line 76**: Returns user object.

```typescript
        id: docSnap.id,
```
**Line 77**: Gets document ID.

```typescript
        ...docSnap.data()
```
**Line 78**: Spreads all document data (name, email, role, etc.).

```typescript
      } as {
        id: string;
        name: string;
        email: string;
        role: string;
        createdAt: any;
      };
```
**Lines 79-85**: TypeScript type assertion (tells TypeScript the shape).

```typescript
    }
```
**Line 86**: Closes if statement.

```typescript
    
    return null;
```
**Line 88**: Returns null if document doesn't exist.

```typescript
  }
```
**Line 89**: Closes getUserProfile function.

```typescript
};
```
**Line 90**: Closes authService object.

```typescript
// Assignment Services
export const assignmentService = {
```
**Lines 92-93**: Exports object with assignment-related functions.

```typescript
  // Create a new assignment
  createAssignment: async (data: {
```
**Lines 94-95**: Defines async function to create assignment.

```typescript
    title: string;
    description: string;
    dueDate: string;
    totalPoints: number;
    requirements?: string[];
    createdBy: string;
  }) => {
```
**Lines 95-102**: Defines parameter type (object with these properties).

```typescript
    try {
```
**Line 103**: Starts try-catch for error handling.

```typescript
      const docRef = await addDoc(collection(db, ASSIGNMENTS_COLLECTION), {
```
**Line 104**: Adds new document to assignments collection:
- `collection(db, ASSIGNMENTS_COLLECTION)`: References collection
- `addDoc`: Adds document (auto-generates ID)

```typescript
        ...data,
```
**Line 105**: Spreads all data from parameter object.

```typescript
        createdAt: serverTimestamp()
```
**Line 106**: Adds server timestamp.

```typescript
      });
```
**Line 107**: Closes addDoc call.

```typescript
      return docRef.id;
```
**Line 108**: Returns the generated document ID.

```typescript
    } catch (error) {
```
**Line 109**: Catches any errors.

```typescript
      console.error("Error creating assignment:", error);
```
**Line 110**: Logs error to console.

```typescript
      throw error;
```
**Line 111**: Re-throws error (so calling code can handle it).

```typescript
    }
```
**Line 112**: Closes try-catch.

```typescript
  },
```
**Line 113**: Closes createAssignment function.

```typescript
  // Get all assignments
  getAllAssignments: async () => {
```
**Lines 115-116**: Defines function to get all assignments.

```typescript
    try {
```
**Line 117**: Starts try-catch.

```typescript
      const q = query(collection(db, ASSIGNMENTS_COLLECTION), orderBy("dueDate", "asc"));
```
**Line 118**: Creates query:
- `query`: Builds database query
- `collection(db, ASSIGNMENTS_COLLECTION)`: References collection
- `orderBy("dueDate", "asc")`: Sorts by due date, ascending (earliest first)

```typescript
      const querySnapshot = await getDocs(q);
```
**Line 119**: Executes query, waits for results.

```typescript
      
      return querySnapshot.docs.map(doc => ({
```
**Line 121**: Maps each document to an object.

```typescript
        id: doc.id,
```
**Line 122**: Adds document ID.

```typescript
        ...doc.data()
```
**Line 123**: Spreads all document data.

```typescript
      }));
```
**Line 124**: Closes map and returns array.

```typescript
    } catch (error) {
      console.error("Error getting assignments:", error);
      throw error;
    }
```
**Lines 125-128**: Error handling.

```typescript
  },
```
**Line 129**: Closes getAllAssignments function.

```typescript
  // Get assignments created by a specific teacher
  getTeacherAssignments: async (teacherId: string) => {
```
**Lines 131-132**: Defines function to get teacher's assignments.

```typescript
    try {
      const q = query(
        collection(db, ASSIGNMENTS_COLLECTION),
        where("createdBy", "==", teacherId),
        orderBy("createdAt", "desc")
      );
```
**Lines 133-137**: Creates query:
- `where("createdBy", "==", teacherId)`: Filters to assignments by this teacher
- `orderBy("createdAt", "desc")`: Sorts by creation date, descending (newest first)

```typescript
      const querySnapshot = await getDocs(q);
```
**Line 139**: Executes query.

```typescript
      
      return querySnapshot.docs.map(doc => ({
        id: doc.id,
        ...doc.data()
      }));
```
**Lines 141-144**: Maps documents to objects.

```typescript
    } catch (error) {
      console.error("Error getting teacher assignments:", error);
      throw error;
    }
```
**Lines 145-148**: Error handling.

```typescript
  },
```
**Line 149**: Closes function.

```typescript
  // Get a single assignment by ID
  getAssignmentById: async (assignmentId: string) => {
```
**Lines 151-152**: Defines function to get one assignment.

```typescript
    try {
      const docRef = doc(db, ASSIGNMENTS_COLLECTION, assignmentId);
```
**Line 153**: Creates reference to specific document.

```typescript
      const docSnap = await getDoc(docRef);
```
**Line 155**: Fetches document.

```typescript
      
      if (docSnap.exists()) {
        return {
          id: docSnap.id,
          ...docSnap.data()
        };
      }
```
**Lines 157-162**: Returns assignment if exists.

```typescript
      
      return null;
```
**Line 164**: Returns null if not found.

```typescript
    } catch (error) {
      console.error("Error getting assignment:", error);
      throw error;
    }
```
**Lines 165-168**: Error handling.

```typescript
  },
```
**Line 169**: Closes function.

```typescript
  // Update an assignment
  updateAssignment: async (assignmentId: string, updates: Partial<DocumentData>) => {
```
**Lines 171-172**: Defines function to update assignment.

```typescript
    try {
      const docRef = doc(db, ASSIGNMENTS_COLLECTION, assignmentId);
```
**Line 173**: Creates document reference.

```typescript
      await updateDoc(docRef, updates);
```
**Line 175**: Updates document with new data.

```typescript
    } catch (error) {
      console.error("Error updating assignment:", error);
      throw error;
    }
```
**Lines 176-179**: Error handling.

```typescript
  },
```
**Line 180**: Closes function.

```typescript
  // Delete an assignment
  deleteAssignment: async (assignmentId: string) => {
```
**Lines 182-183**: Defines function to delete assignment.

```typescript
    try {
      const docRef = doc(db, ASSIGNMENTS_COLLECTION, assignmentId);
```
**Line 184**: Creates document reference.

```typescript
      await deleteDoc(docRef);
```
**Line 186**: Deletes document.

```typescript
    } catch (error) {
      console.error("Error deleting assignment:", error);
      throw error;
    }
```
**Lines 187-190**: Error handling.

```typescript
  }
```
**Line 191**: Closes function and assignmentService object.

```typescript
};
```
**Line 192**: Closes assignmentService.

```typescript
// Submission Services
export const submissionService = {
```
**Lines 194-195**: Exports object with submission-related functions.

```typescript
  // Create a new submission
  createSubmission: async (data: {
    studentId: string;
    assignmentId: string;
    content: string;
    images?: string[];
  }) => {
```
**Lines 196-201**: Defines function to create submission.

```typescript
    try {
      const docRef = await addDoc(collection(db, SUBMISSIONS_COLLECTION), {
        ...data,
        images: data.images || [],
        submittedAt: serverTimestamp(),
        status: "submitted"
      });
```
**Lines 203-209**: Creates submission document:
- Spreads data
- Sets images array (or empty array if not provided)
- Adds server timestamp
- Sets status to "submitted"

```typescript
      return docRef.id;
```
**Line 210**: Returns document ID.

```typescript
    } catch (error) {
      console.error("Error creating submission:", error);
      throw error;
    }
```
**Lines 211-214**: Error handling.

```typescript
  },
```
**Line 215**: Closes function.

```typescript
  // Get submissions for a specific assignment
  getSubmissionsByAssignment: async (assignmentId: string) => {
```
**Lines 217-218**: Defines function to get all submissions for an assignment.

```typescript
    try {
      const q = query(
        collection(db, SUBMISSIONS_COLLECTION),
        where("assignmentId", "==", assignmentId),
        orderBy("submittedAt", "desc")
      );
```
**Lines 219-223**: Creates query:
- Filters by assignmentId
- Sorts by submission date, descending

```typescript
      const querySnapshot = await getDocs(q);
      
      return querySnapshot.docs.map(doc => ({
        id: doc.id,
        ...doc.data()
      }));
```
**Lines 224-230**: Executes query and maps results.

```typescript
    } catch (error) {
      console.error("Error getting submissions:", error);
      throw error;
    }
```
**Lines 231-234**: Error handling.

```typescript
  },
```
**Line 235**: Closes function.

```typescript
  // Get submissions by a specific student
  getStudentSubmissions: async (studentId: string) => {
```
**Lines 237-238**: Defines function to get student's submissions.

```typescript
    try {
      const q = query(
        collection(db, SUBMISSIONS_COLLECTION),
        where("studentId", "==", studentId),
        orderBy("submittedAt", "desc")
      );
```
**Lines 239-243**: Creates query filtering by studentId.

```typescript
      const querySnapshot = await getDocs(q);
      
      return querySnapshot.docs.map(doc => ({
        id: doc.id,
        ...doc.data()
      }));
```
**Lines 244-250**: Executes and maps results.

```typescript
    } catch (error) {
      console.error("Error getting student submissions:", error);
      throw error;
    }
```
**Lines 251-254**: Error handling.

```typescript
  },
```
**Line 255**: Closes function.

```typescript
  // Get submission by student and assignment
  getSubmissionByStudentAndAssignment: async (studentId: string, assignmentId: string) => {
```
**Lines 257-258**: Defines function to get specific student's submission for specific assignment.

```typescript
    try {
      const q = query(
        collection(db, SUBMISSIONS_COLLECTION),
        where("studentId", "==", studentId),
        where("assignmentId", "==", assignmentId)
      );
```
**Lines 259-264**: Creates query with two filters (student AND assignment).

```typescript
      const querySnapshot = await getDocs(q);
      
      if (!querySnapshot.empty) {
        const doc = querySnapshot.docs[0];
        return {
          id: doc.id,
          ...doc.data()
        };
      }
      
      return null;
```
**Lines 265-275**: Returns first document if found, null otherwise.

```typescript
    } catch (error) {
      console.error("Error getting submission:", error);
      throw error;
    }
```
**Lines 276-279**: Error handling.

```typescript
  },
```
**Line 280**: Closes function.

```typescript
  // Update a submission
  updateSubmission: async (submissionId: string, updates: Partial<DocumentData>) => {
```
**Lines 282-283**: Defines function to update submission.

```typescript
    try {
      const docRef = doc(db, SUBMISSIONS_COLLECTION, submissionId);
      await updateDoc(docRef, updates);
```
**Lines 284-286**: Updates document with new data.

```typescript
    } catch (error) {
      console.error("Error updating submission:", error);
      throw error;
    }
```
**Lines 287-290**: Error handling.

```typescript
  },
```
**Line 291**: Closes function.

```typescript
  // Grade a submission
  gradeSubmission: async (submissionId: string, grade: number, feedback?: string) => {
```
**Lines 293-294**: Defines function to grade submission.

```typescript
    try {
      const docRef = doc(db, SUBMISSIONS_COLLECTION, submissionId);
      await updateDoc(docRef, {
        grade,
        feedback,
        status: "graded"
      });
```
**Lines 295-301**: Updates submission with grade, feedback, and sets status to "graded".

```typescript
    } catch (error) {
      console.error("Error grading submission:", error);
      throw error;
    }
```
**Lines 302-305**: Error handling.

```typescript
  }
```
**Line 306**: Closes function and submissionService object.

```typescript
};
```
**Line 307**: Closes submissionService.

```typescript
// User Services
export const userService = {
```
**Lines 309-310**: Exports object with user-related functions.

```typescript
  // Get all students
  getAllStudents: async () => {
```
**Lines 311-312**: Defines function to get all students.

```typescript
    try {
      const q = query(
        collection(db, USERS_COLLECTION),
        where("role", "==", "student")
      );
```
**Lines 313-317**: Creates query filtering users by role="student".

```typescript
      const querySnapshot = await getDocs(q);
      
      return querySnapshot.docs.map(doc => ({
        id: doc.id,
        ...doc.data()
      }));
```
**Lines 318-323**: Executes query and maps results.

```typescript
    } catch (error) {
      console.error("Error getting students:", error);
      throw error;
    }
```
**Lines 324-327**: Error handling.

```typescript
  }
```
**Line 328**: Closes function and userService object.

```typescript
};
```
**Line 329**: Closes userService.

---

---

## 5. src/components/PrivateRoute.tsx - Route Protection Component

```typescript
import { Navigate } from 'react-router-dom';
```
**Line 1**: Imports Navigate component for programmatic redirects.

```typescript
import { useAuth } from '../contexts/AuthContext';
```
**Line 2**: Imports useAuth hook to access authentication state.

```typescript
import { ReactNode } from 'react';
```
**Line 3**: Imports ReactNode type for TypeScript (represents any valid React child).

```typescript
import { Center, Spinner } from '@chakra-ui/react';
```
**Line 4**: Imports Chakra UI components:
- `Center`: Centers content
- `Spinner`: Loading spinner component

```typescript
interface PrivateRouteProps {
```
**Line 6**: Defines TypeScript interface for component props.

```typescript
  children: ReactNode;
```
**Line 7**: Property: the child components to render if authorized.

```typescript
  requiredRole?: 'teacher' | 'student';
```
**Line 8**: Optional property: role required to access this route (undefined means any logged-in user).

```typescript
}
```
**Line 9**: Closes interface.

```typescript
export function PrivateRoute({ children, requiredRole }: PrivateRouteProps) {
```
**Line 11**: Exports PrivateRoute component:
- `{ children, requiredRole }`: Destructures props
- `: PrivateRouteProps`: TypeScript type annotation

```typescript
  const { currentUser, loading } = useAuth();
```
**Line 12**: Gets current user and loading state from auth context.

```typescript
  console.log('PrivateRoute - currentUser:', currentUser);
```
**Line 14**: Logs current user for debugging.

```typescript
  console.log('PrivateRoute - requiredRole:', requiredRole);
```
**Line 15**: Logs required role for debugging.

```typescript
  console.log('PrivateRoute - loading:', loading);
```
**Line 16**: Logs loading state for debugging.

```typescript
  if (loading) {
```
**Line 18**: Checks if auth state is still being determined.

```typescript
    return (
      <Center h="100vh">
```
**Lines 19-20**: Returns centered loading spinner:
- `h="100vh"`: Full viewport height

```typescript
        <Spinner size="xl" />
```
**Line 21**: Renders large loading spinner.

```typescript
      </Center>
    );
```
**Lines 22-23**: Closes Center and return statement.

```typescript
  }
```
**Line 24**: Closes if statement.

```typescript
  // Check if user is authenticated
  if (!currentUser) {
```
**Lines 26-27**: Checks if user is NOT logged in.

```typescript
    console.log('No current user, redirecting to login');
```
**Line 28**: Logs redirect action.

```typescript
    return <Navigate to="/login" />;
```
**Line 29**: Redirects to login page if not authenticated.

```typescript
  }
```
**Line 30**: Closes if statement.

```typescript
  // Check if user has the required role
  if (requiredRole && currentUser.role !== requiredRole) {
```
**Lines 32-33**: Checks if route requires specific role AND user doesn't have that role.

```typescript
    console.log(`Role mismatch. User role: ${currentUser.role}, Required: ${requiredRole}, Redirecting to ${currentUser.role}-dashboard`);
```
**Line 34**: Logs role mismatch with details (template literal string).

```typescript
    return <Navigate to={`/${currentUser.role}-dashboard`} />;
```
**Line 35**: Redirects to user's appropriate dashboard (teacher or student).

```typescript
  }
```
**Line 36**: Closes if statement.

```typescript
  console.log('PrivateRoute allowing access');
```
**Line 38**: Logs successful authorization.

```typescript
  return <>{children}</>;
```
**Line 39**: Returns children wrapped in React Fragment (empty tags):
- `<>...</>`: Fragment syntax (doesn't add extra DOM element)
- Renders the protected component if all checks pass

```typescript
}
```
**Line 40**: Closes PrivateRoute function.

---

## 6. src/pages/Login.tsx - Login Page (Key Lines Explained)

```typescript
import { useState } from 'react';
```
**Line 1**: Imports useState hook for component state management.

```typescript
import { useNavigate } from 'react-router-dom';
```
**Line 2**: Imports useNavigate hook for programmatic navigation.

```typescript
import { useAuth } from '../contexts/AuthContext';
```
**Line 3**: Imports useAuth to access login function.

```typescript
import {
  Box, Button, Card, CardBody, CardHeader, Center, Container,
  FormControl, FormLabel, Heading, Input, InputGroup,
  InputRightElement, VStack, Text, Link, Alert, AlertIcon,
  IconButton, Flex, HStack, Divider,
} from '@chakra-ui/react';
```
**Lines 4-26**: Imports Chakra UI components for building the login form UI.

```typescript
import { ViewIcon, ViewOffIcon } from '@chakra-ui/icons';
```
**Line 28**: Imports icons for show/hide password toggle.

```typescript
export default function Login() {
```
**Line 30**: Exports Login component as default.

```typescript
  const [email, setEmail] = useState('');
```
**Line 31**: Creates state for email input (starts empty).

```typescript
  const [password, setPassword] = useState('');
```
**Line 32**: Creates state for password input (starts empty).

```typescript
  const [showPassword, setShowPassword] = useState(false);
```
**Line 33**: Creates state for password visibility toggle (starts hidden).

```typescript
  const [loading, setLoading] = useState(false);
```
**Line 34**: Creates state for loading indicator (starts false).

```typescript
  const [error, setError] = useState('');
```
**Line 35**: Creates state for error message (starts empty).

```typescript
  const { login } = useAuth();
```
**Line 37**: Gets login function from auth context.

```typescript
  const navigate = useNavigate();
```
**Line 38**: Gets navigate function for routing.

```typescript
  const handleLogin = async (e: React.FormEvent) => {
```
**Line 40**: Defines async function to handle form submission:
- `e: React.FormEvent`: TypeScript type for form event

```typescript
    e.preventDefault();
```
**Line 41**: Prevents default form submission (stops page reload).

```typescript
    setError('');
```
**Line 42**: Clears any previous error messages.

```typescript
    if (!email || !password) {
```
**Line 44**: Validates that both fields are filled.

```typescript
      setError('Please fill in all fields');
```
**Line 45**: Sets error message if validation fails.

```typescript
      return;
```
**Line 46**: Stops function execution (doesn't attempt login).

```typescript
    }
```
**Line 47**: Closes if statement.

```typescript
    try {
```
**Line 49**: Starts try-catch for error handling.

```typescript
      setLoading(true);
```
**Line 50**: Sets loading to true (shows loading spinner).

```typescript
      await login(email, password);
```
**Line 51**: Calls login function and waits for completion.

```typescript
      navigate('/dashboard');
```
**Line 52**: Navigates to dashboard after successful login.

```typescript
    } catch (err: any) {
```
**Line 53**: Catches any errors during login:
- `err: any`: TypeScript allows any error type

```typescript
      console.error('Login error:', err);
```
**Line 54**: Logs error to console.

```typescript
      setError(err.message || 'Failed to login. Please check your credentials.');
```
**Line 55**: Sets user-friendly error message:
- Uses error message if available
- Falls back to default message if not

```typescript
    } finally {
```
**Line 56**: Always runs after try-catch.

```typescript
      setLoading(false);
```
**Line 57**: Sets loading to false (hides spinner).

```typescript
    }
```
**Line 58**: Closes finally block.

```typescript
  };
```
**Line 59**: Closes handleLogin function.

```typescript
  return (
    <Center minH="100vh" bg="linear-gradient(135deg, #f8fafc 0%, #e2e8f0 100%)" py={12}>
```
**Lines 61-62**: Returns JSX:
- `Center`: Centers content
- `minH="100vh"`: Full viewport height
- `bg="linear-gradient(...)"`: Purple gradient background
- `py={12}`: Vertical padding

```typescript
        <Container maxW="md">
```
**Line 63**: Creates container with max-width "md" (medium size).

```typescript
          <Card bg="white" borderRadius="20px" boxShadow="..." border="1px solid" borderColor="gray.100" overflow="hidden">
```
**Lines 64-70**: Creates card component:
- `bg="white"`: White background
- `borderRadius="20px"`: Rounded corners
- `boxShadow`: Shadow effect
- `border`: Border styling
- `overflow="hidden"`: Hides overflow (for rounded corners)

```typescript
            <CardHeader bg="linear-gradient(135deg, #7d02c4 0%, #9333ea 100%)" color="white" textAlign="center" py={8}>
```
**Line 72**: Creates header with purple gradient:
- `bg="linear-gradient(...)"`: Purple gradient
- `color="white"`: White text
- `textAlign="center"`: Centers text
- `py={8}`: Padding top/bottom

```typescript
              <VStack spacing={2}>
```
**Line 73**: Creates vertical stack with spacing.

```typescript
                <Heading size="lg" fontWeight="700">Welcome Back!</Heading>
```
**Line 74**: Renders heading:
- `size="lg"`: Large size
- `fontWeight="700"`: Bold

```typescript
                <Text opacity={0.9}>Sign in to your account</Text>
```
**Line 75**: Renders subtitle with slight transparency.

```typescript
              </VStack>
            </CardHeader>
```
**Lines 76-77**: Closes VStack and CardHeader.

```typescript
            <CardBody p={8}>
```
**Line 79**: Creates card body with padding.

```typescript
              <form onSubmit={handleLogin}>
```
**Line 80**: Creates form that calls handleLogin on submit.

```typescript
                <VStack spacing={6}>
```
**Line 81**: Creates vertical stack for form fields.

```typescript
                  {error && (
```
**Line 82**: Conditionally renders error if error state exists.

```typescript
                    <Alert status="error" borderRadius="12px" bg="red.50" border="1px solid" borderColor="red.200">
```
**Line 83**: Creates error alert with styling.

```typescript
                      <AlertIcon />
```
**Line 84**: Renders error icon.

```typescript
                      {error}
```
**Line 85**: Displays error message.

```typescript
                    </Alert>
                  )}
```
**Lines 86-87**: Closes Alert and conditional.

```typescript
                  <FormControl isRequired>
```
**Line 89**: Creates form control with required validation.

```typescript
                    <FormLabel color="gray.700" fontWeight="600">Email Address</FormLabel>
```
**Line 90**: Renders label for email field.

```typescript
                    <Input
                      type="email"
                      value={email}
                      onChange={(e) => setEmail(e.target.value)}
                      placeholder="Enter your email"
                      borderRadius="12px"
                      borderColor="gray.200"
                      _focus={{ borderColor: "#7d02c4", boxShadow: "0 0 0 1px #7d02c4" }}
                      py={6}
                    />
```
**Lines 91-100**: Creates email input:
- `type="email"`: Email input type
- `value={email}`: Controlled input (value from state)
- `onChange`: Updates state on change
- `placeholder`: Hint text
- `borderRadius`: Rounded corners
- `_focus`: Purple border on focus (Chakra pseudo-props)

```typescript
                  </FormControl>
```
**Line 101**: Closes form control.

```typescript
                  <FormControl isRequired>
                    <FormLabel color="gray.700" fontWeight="600">Password</FormLabel>
                    <InputGroup>
```
**Lines 103-105**: Creates password field with input group (for show/hide button).

```typescript
                      <Input
                        type={showPassword ? 'text' : 'password'}
                        value={password}
                        onChange={(e) => setPassword(e.target.value)}
                        placeholder="Enter your password"
                        borderRadius="12px"
                        borderColor="gray.200"
                        _focus={{ borderColor: "#7d02c4", boxShadow: "0 0 0 1px #7d02c4" }}
                        py={6}
                      />
```
**Lines 106-115**: Creates password input:
- `type={showPassword ? 'text' : 'password'}`: Toggles between text/password based on state

```typescript
                      <InputRightElement h="full">
```
**Line 116**: Creates right element container (for icon button).

```typescript
                        <IconButton
                          aria-label={showPassword ? 'Hide password' : 'Show password'}
                          icon={showPassword ? <ViewOffIcon /> : <ViewIcon />}
                          onClick={() => setShowPassword(!showPassword)}
                          variant="ghost"
                          color="gray.500"
                          _hover={{ color: "#7d02c4" }}
                        />
```
**Lines 117-124**: Creates toggle button:
- `aria-label`: Accessibility label
- `icon`: Shows different icon based on state
- `onClick`: Toggles showPassword state
- `variant="ghost"`: Transparent button style

```typescript
                      </InputRightElement>
                    </InputGroup>
                  </FormControl>
```
**Lines 125-127**: Closes input group and form control.

```typescript
                  <Button
                    type="submit"
                    bg="#7d02c4"
                    color="white"
                    _hover={{ bg: "#6b019f", transform: "translateY(-2px)" }}
                    _active={{ bg: "#5a0186", transform: "translateY(0)" }}
                    borderRadius="12px"
                    width="full"
                    py={6}
                    fontSize="md"
                    fontWeight="600"
                    transition="all 0.3s"
                    isLoading={loading}
                    loadingText="Signing in..."
                  >
                    Sign In
                  </Button>
```
**Lines 129-145**: Creates submit button:
- `type="submit"`: Submits form
- `bg="#7d02c4"`: Purple background
- `_hover`: Darker purple on hover
- `_active`: Even darker on click
- `isLoading={loading}`: Shows spinner when loading
- `loadingText`: Text shown during loading

```typescript
                  <Divider />
```
**Line 147**: Adds horizontal divider.

```typescript
                  <Text textAlign="center" color="gray.600">
                    Don't have an account?{' '}
                    <Link color="#7d02c4" fontWeight="600" _hover={{ textDecoration: "underline" }} onClick={() => navigate('/register')}>
                      Sign up here
                    </Link>
                  </Text>
```
**Lines 149-155**: Renders registration link:
- `{' '}`: Adds space between text and link
- `onClick`: Navigates to register page

```typescript
                  <VStack spacing={2} pt={4}>
                    <Text fontSize="sm" color="gray.500" textAlign="center">Demo Accounts:</Text>
                    <HStack spacing={4} fontSize="xs" color="gray.600">
                      <Text>Teacher: teacher@example.com</Text>
                      <Text>Student: student@example.com</Text>
                    </HStack>
                    <Text fontSize="xs" color="gray.500">Password: password123</Text>
                  </VStack>
```
**Lines 161-167**: Displays demo account information for testing.

```typescript
                </VStack>
              </form>
            </CardBody>
          </Card>
        </Container>
      </Center>
```
**Lines 169-173**: Closes all components and the return statement.

```typescript
  );
}
```
**Lines 174-175**: Closes Login component.

---

This covers the main architecture and key components. The StudentDashboard and TeacherDashboard follow similar patterns but are more complex with:
- Multiple state variables
- useEffect hooks for data loading
- Modal components for forms
- Image handling
- Complex conditional rendering

Would you like me to explain specific sections of the dashboard files, or do you have questions about any particular part?

