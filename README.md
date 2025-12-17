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

### Part 1: Setup Firebase (Database & Backend)

**A. Create Project**
1.  Go to [console.firebase.google.com](https://console.firebase.google.com/) and sign in with your Google account.
2.  Click on **"Add project"**.
3.  Give the project a name (e.g., `my-game-project`) and click "Continue".
4.  Disable "Google Analytics" (not needed for this game) and click **"Create project"**.
5.  Wait a moment for the project to be ready, then click **"Continue"**.

**B. Register Web App**
1.  Click on **"Add app"** (below the project name).
2.  Click on the **Web icon** `</>` (a circle with `</>` inside, below the project name).
3.  Give the app a name (e.g., `game-app`) and click **"Register app"** (Keep Firebase Hosting disabled).
4.  **IMPORTANT:** You will see a code block (`const firebaseConfig = ...`). Copy the content between the curly braces `{ ... }` (including the braces). You will need this in Part 2.
5.  Click **"Continue to console"**.

**C. Create Database**
1.  In the left menu, click **"Build"** -> **"Realtime Database"**.
2.  Click **"Create Database"**.
3.  Choose a location (e.g., **"Belgium (europe-west1)"**) and click **"Next"**.
4.  Choose **"Start in test mode"** and click **"Enable"**.

**D. Update Database Rules**
1.  Go to the **"Rules"** tab in the Realtime Database.
2.  Change the rules to allow access to the `games` node. Delete the existing code and paste this:
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
3.  Click **"Publish"**.

**E. Activate Authentication**
1.  In the left menu, click **"Build"** -> **"Authentication"**.
2.  Click **"Get started"**.
3.  Select **"Anonymous"** under "Additional providers".
4.  Toggle the switch to "Enable" and click **"Save"**.

### Part 2: Get the Code & Host on GitHub

To make the game accessible to your friends, you will host it for free on GitHub Pages. Instead of downloading files, you can copy them directly from my repository.

**A. Create a GitHub Account & Import Repository**
1.  Go to [github.com](https://github.com/) and sign up or log in.
2.  In the top-right corner, click the **+** icon and select **"Import repository"**.
3.  **Your old repository's clone URL:** Paste this link:  
    `https://github.com/JTBeuter/Nobody-s-Perfect.git`
4.  **Repository Name:** Enter a name for your game (e.g., `my-trivia-game`).
5.  **Privacy:** Select **Public**.
6.  Click **"Begin import"**.
7.  Wait for the process to finish (it might take a minute). When done, click the link to go to your new repository.

**B. Connect Your Firebase**
Now you need to put your own Firebase keys into the code.
1.  In your new repository, find the file list and click on `firebase-config.js`.
2.  Click the **pencil icon** (top right of the file view) to edit the file.
3.  **⚠️ IMPORTANT:** When replacing the code, **ensure the line starts with `export const`**.
    *   Firebase gives you: `const firebaseConfig = ...`
    *   **YOU MUST CHANGE IT TO:** `export const firebaseConfig = ...`
    
    Replace the file content with your config so it looks exactly like this:
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
4.  Scroll down and click the green button **"Commit changes"**.

**C. Activate GitHub Pages**
1.  In your repository, click the **"Settings"** tab (top menu bar).
2.  In the left sidebar, click on **"Pages"** (under the "Code and automation" section).
3.  Under **"Build and deployment"**, find the **"Branch"** section.
4.  Click the dropdown menu that says "None" and select **"main"** (or "master").
5.  Click **"Save"**.
6.  Wait about 1-2 minutes for GitHub to build your site. Refresh the page occasionally.
7.  A bar will appear at the top of the page saying: **"Your site is live at..."** followed by a link (e.g., `https://your-username.github.io/my-trivia-game/`).
8.  Click that link to open your game!

**D. Share with Players**
1.  Copy the URL of your live site.
2.  Send this link to your players.
3.  Open the link yourself and click **"Host New Game"** to start.

## Usage

### For Players
1.  Open the game link provided by the host.
2.  Click **"Join Game"**.
3.  Enter the **Session Code** (displayed on the Host's screen) and your **Team Name**.
4.  Wait for the game to start.
5.  Enter your invented answer when prompted.
6.  Vote on other answers (guessing the correct one).
7.  View the scoreboard after each round.

### For Host
1.  Open the game link.
2.  Click **"Host New Game"** (this generates a unique Session Code).
3.  **Setup Phase:**
    *   Enter a question and the correct answer.
    *   Click **"Add Question"** (repeat for all questions).
    *   (Optional) Customize colors or background.
    *   Click **"Create Lobby"** when ready.
4.  **Game Phase:**
    *   Share the **Session Code** (or QR Code) with players.
    *   Wait for teams to join.
    *   Click **"Start Game"** to begin the answering phase.
    *   Control the flow: "To Voting Phase" -> "Start Voting" -> "Show Results" -> "Next Question".

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
