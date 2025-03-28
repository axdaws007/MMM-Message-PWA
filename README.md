# MagicMirror Messages PWA

A Progressive Web App (PWA) that allows authorized users to send messages to a MagicMirror display. The application uses Firebase for authentication, real-time database storage, and message management.

## Features

- Google Authentication with authorized user whitelist
- Real-time message display
- Message character limit (160 characters)
- Message deletion capability
- Mobile-friendly responsive design
- Offline capability through PWA implementation
- Automatic message cleanup (via Firebase Cloud Functions)

## Project Structure

```
MagicMirror-Messages-PWA/
├── index.html              # Main application HTML
├── firebase-config.js      # Firebase configuration (not included in repo)
├── firebase-config.example.js  # Example configuration template
├── manifest.json           # PWA manifest file
├── service-worker.js       # Service worker for offline functionality
├── placeholder-avatar.png  # Default user avatar
└── .gitignore              # Git ignore file
```

## Setup Instructions

### 1. Clone the Repository

```bash
git clone https://github.com/yourusername/MagicMirror-Messages-PWA.git
cd MagicMirror-Messages-PWA
```

### 2. Set Up Firebase Configuration

Create a `firebase-config.js` file based on the example template:

1. Copy the example configuration file:
   ```bash
   cp firebase-config.example.js firebase-config.js
   ```

2. Edit `firebase-config.js` and add your Firebase project details:
   ```javascript
   const firebaseConfig = {
       apiKey: "YOUR_ACTUAL_API_KEY",
       authDomain: "your-project.firebaseapp.com",
       databaseURL: "https://your-project-default-rtdb.firebaseio.com",
       projectId: "your-project-id",
       storageBucket: "your-project.appspot.com",
       messagingSenderId: "123456789012",
       appId: "1:123456789012:web:abcdef1234567890"
   };
   ```

> **Important:** The `firebase-config.js` file contains sensitive information and is excluded from the repository via `.gitignore`. Never commit this file to version control.

### 3. Set Up Authorized Users in Firebase

1. In your Firebase Realtime Database, create an `allowedUsers` node with authorized email addresses:
   ```json
   {
     "allowedUsers": {
       "user1@example,com": true,
       "user2@example,com": true
     }
   }
   ```
   
   Note: Firebase doesn't allow periods (`.`) in keys, so email addresses use commas instead of periods.

### 4. Deploy the Application

#### Option 1: Firebase Hosting (Recommended)

1. Install Firebase CLI:
   ```bash
   npm install -g firebase-tools
   ```

2. Initialize Firebase Hosting:
   ```bash
   firebase login
   firebase init hosting
   ```

3. Deploy:
   ```bash
   firebase deploy --only hosting
   ```

#### Option 2: Any Static Web Hosting

Upload all files except `.gitignore` to your web hosting service.

## Using the Application

1. Access the application from any device with a web browser
2. Sign in using the Google Sign-In button
3. If authorized, you'll see the message input area and previous messages
4. Type a message (up to 160 characters) and send
5. Messages appear in real-time on the MagicMirror display
6. Delete your own messages using the trash icon

## Automatic Message Cleanup

Messages older than 7 days are automatically deleted by a Cloud Function. See the `MMM-Message-Functions` repository for the Firebase Functions implementation details.

## Development Notes

### Firebase Security Rules

Recommended security rules for your Firebase Realtime Database:

```json
{
  "rules": {
    "messages": {
      ".read": "auth != null && root.child('allowedUsers').child(auth.token.email.replace('.', ',')).exists()",
      ".write": "auth != null && root.child('allowedUsers').child(auth.token.email.replace('.', ',')).exists()"
    },
    "allowedUsers": {
      ".read": "auth != null",
      ".write": false
    }
  }
}
```

### Adding New Users

To add a new authorized user:

1. Go to your Firebase Realtime Database console
2. Navigate to the `allowedUsers` node
3. Add a new entry with the user's email address (replace `.` with `,`)
4. Set the value to `true`

## Customization

- Edit styles in the `<style>` section of `index.html`
- Modify the message display format in the `loadMessages()` function
- Adjust the character limit by changing the `maxlength` attribute on the textarea

## License

MIT License - See LICENSE file for details.