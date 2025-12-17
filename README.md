# 🎮 Nobody's Perfect - Web Game

A browser-based web app for a "Nobody's Perfect" style trivia game with real-time database synchronization and automatic scoring.

## Features

✅ **Team Management** - Teams can register with names  
✅ **Questions & Answers** - Host asks questions, teams submit invented answers  
✅ **Voting System** - Teams vote on all answers (except their own)  
✅ **Automatic Scoring** - Points awarded based on the classic game rules:
- Voting for the Correct answer: +3 Points
- Per vote received on own invented answer: +1 Point

✅ **Real-Time Updates** - Firebase WebSockets, no reload required  
✅ **Responsive Design** - Works on Desktop, Tablet, and Mobile  
✅ **Persistent Storage** - All data stored in Firebase Realtime Database
✅ **Custom Background** - Host can upload a custom background image

## Tech Stack

- **Frontend**: HTML5, CSS3, JavaScript (ES6+)
- **Backend**: Firebase Realtime Database
- **Authentication**: Firebase Anonymous Authentication
- **Hosting**: GitHub Pages / any static server

## 🚀 Step-by-Step Installation Guide

Here is the easiest way to get your own version of the game running.

### 1. Setup Firebase (Database & Backend)

1.  Go to [console.firebase.google.com](https://console.firebase.google.com/) and sign in with your Google account.
2.  Click on **"Add project"**.
3.  Give the project a name (e.g., `my-game-project`) and click "Continue".
4.  Disable "Google Analytics" (not needed for this game) and click **"Create project"**.
5.  Wait a moment for the project to be ready, then click **"Continue"**.
6.  You are now in the dashboard. Click on the **Web icon** `</>` (a circle with `</>` inside, below the project name).
7.  Give the app a name (e.g., `game-app`) and click **"Register app"**.
8.  **IMPORTANT:** You will see a code block (`const firebaseConfig = ...`). Copy the content between the curly braces `{ ... }` (including the braces). You will need this in Part 2.
9.  Click "Continue to console".

**Activate Database:**
10. In the left menu, click **"Build"** -> **"Realtime Database"**.
11. Click **"Create Database"**.
12. Choose a location (e.g., **"Belgium (europe-west1)"**) and click "Next".
13. Choose **"Start in test mode"** and click **"Enable"**.
    *   *Note: Test mode allows read/write access for 30 days. For permanent use, adjust the rules later.*

**Update Database Rules (CRITICAL for Multi-Session):**
14. Go to the **"Rules"** tab in the Realtime Database.
15. Change the rules to allow access to the `games` node:
```json
{
  "rules": {
    "games": {
      ".read": true,
      ".write": true
    }
  }
}
```
16. Click **"Publish"**.

**Activate Authentication:**
17. In the left menu, click **"Build"** -> **"Authentication"**.
18. Click **"Get started"**.
19. Select **"Anonymous"** under "Additional providers".
20. Toggle the switch to "Enable" and click **"Save"**.

### Part 2: Connect Code

1.  Open the file `firebase-config.js` in your project folder.
2.  Replace the entire content of the `firebaseConfig` object with the data you copied in Step 8.
    The file should look like this:
    ```javascript
    export const firebaseConfig = {
        apiKey: "Your-API-Key...",
        authDomain: "your-project.firebaseapp.com",
        databaseURL: "https://your-project...firebasedatabase.app",
        projectId: "your-project-id",
        storageBucket: "your-project.firebasestorage.app",
        messagingSenderId: "123456...",
        appId: "1:123456...",
        measurementId: "G-..."
    };
    ```
3.  Save the file.

### Part 3: Run Locally

Since this project uses JavaScript ES6 Modules, you cannot simply double-click `index.html`. You need a local web server.

**Option A: Using Python (Pre-installed on macOS/Linux/most Windows)**
1. Open your terminal/command prompt in the project folder.
2. Run:
   ```bash
   python -m http.server
   # OR for Python 2
   python -m SimpleHTTPServer
   ```
3. Open `http://localhost:8000` in your browser.

**Option B: Using Node.js**
1. Install `http-server` globally (once):
   ```bash
   npm install -g http-server
   ```
2. Run in project folder:
   ```bash
   http-server
   ```
3. Open the link shown (usually `http://127.0.0.1:8080`).

### Part 4: Publish on GitHub (Frontend)

1.  Go to [github.com/new](https://github.com/new) and create a new repository.
2.  Give it a name (e.g., `trivia-game`) and select **"Public"**.
3.  Click **"Create repository"**.
4.  Upload your files:
    *   **Option A (Easy - via Browser):**
        1. Click the link "uploading an existing file" in the "Quick setup" area.
        2. Drag and drop all files from your local folder (`index.html`, `host.html`, `firebase-config.js`, `styles.css`, etc.) into the browser window.
        3. Click the green button **"Commit changes"** at the bottom.
    *   **Option B (Advanced - via Git):**
        Open your terminal in the project folder and run:
        ```bash
        git init
        git add .
        git commit -m "Initial release"
        git branch -M main
        git remote add origin https://github.com/YOUR_USERNAME/REPO_NAME.git
        git push -u origin main
        ```

**Activate GitHub Pages (to go live):**
5.  Go to the **"Settings"** tab in your repository.
6.  Click **"Pages"** in the left menu.
7.  Under "Build and deployment" -> "Branch":
    *   Select **"main"** (or "master") from the dropdown.
    *   Click **"Save"**.
8.  Wait about 1-2 minutes. Refresh the page (`F5`).
9.  A box will appear at the top with the link: `Your site is live at...`
10. Click it – Your game is now online! 🥳

**Tip:** Share the link to `index.html` with your players. You (the host) should click "Host New Game" on that page to start.

## Usage

### 👥 For Players

1. Open `index.html` (or the GitHub Pages URL)
2. Click "Join Game"
3. Enter the Session Code (displayed on Host's screen) and your Team Name
4. Wait for the game to start
5. Enter your answer when prompted
6. Vote on other answers
7. View the scoreboard

### 🎮 For Host

1. Open `index.html`
2. Click **"Host New Game"** (this generates a unique Session Code)
3. **Setup Phase:**
   - Enter a question and the correct answer
   - Click "Add Question" (repeat for all questions)
   - (Optional) Customize colors or background
   - Click **"Create Lobby"** when ready
4. **Game Phase:**
   - Share the Session Code (or QR Code) with players
   - Wait for teams to join
   - Click "Start Game" to begin answering phase
   - Control the flow: "To Voting Phase" -> "Start Voting" -> "Show Results" -> "Next Question"

## Database Structure

```
games/
└── SESSION_CODE/             # e.g., "A1B2C"
    ├── currentQuestion/      # Active question data
    │   ├── id: "q1"
    │   ├── text: "Question..."
    │   ├── correct: "Answer..."
    │   └── timerStartTime: 1234567890
    │
    ├── questions/            # All questions for this session
    │   └── q1/ ...
    │
    ├── answers/              # Answers for current question
    │   └── q1/
    │       └── TeamName/
    │           ├── text: "Answer..."
    │           └── votes: 2
    │
    ├── scores/               # Team scores
    │   ├── Team1: 10
    │   └── Team2: 8
    │
    ├── teams/                # Registered teams
    │   └── TeamName/ ...
    │
    └── settings/             # Session settings
        ├── background: "base64..."
        ├── bgScale: 2
        └── theme: { ... }
```

## Customization

### Change Design
Colors/Styles are defined in `styles.css`. Modify the CSS variables:

```css
:root {
    --primary-color: #6366f1;    /* Main color */
    --secondary-color: #ec4899;   /* Secondary color */
    --bg-color: #0f172a;          /* Background color */
}
```

## Mobile Optimization

The app is fully responsive:
- Mobile: 1 Column
- Tablet: 2 Columns
- Desktop: Optimized layout

## License

Free to use. Have fun playing!