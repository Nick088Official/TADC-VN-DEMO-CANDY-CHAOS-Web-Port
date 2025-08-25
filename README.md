# TADC VN DEMO CANDY CHAOS - Web Port

This is an **Unofficial Web Port** of the free RPG Maker MV horror game [**TADC VN DEMO CANDY CHAOS (aka The Amazing Digital Circus Visual Novel Fangame 2 Demo) by allhailthequeenuwu**](https://allhailthequeenuwu.itch.io/theamazingdigitalfangame2demo). This port allows the game to be played directly in a web browser without needing any downloads/installations.

## How to play

There are 2 methods:

### Vercel
Go to https://tadc-vn-demo-candy-chaos-web-port.vercel.app/godot/TADC%20Fangame%20sequel.html

### Local Server
1. Click Code -> Download ZIP -> Extract the Zip.
2. [Get Python](https://www.python.org/downloads/) installed and added to your system's PATH.
3. Open a CMD/Terminal **inside the root project folder** (I manually renamed the folder for a shorter url for github pages and to not confuse with the non-game files).
4. Run the following command to start a simple web server:
    ```bash
    python server.py
    ```
5. Open your web browser and go to the following address:
    **http://localhost:8000/godot/TADC%20Fangame%20sequel.html**


## How did you make it?

This guide provides step-by-step instructions for building a web version of the game.

### Prerequisites

1. **Get the Game Files:** [Download the original game from the creator's itch.io page](https://allhailthequeenuwu.itch.io/theamazingdigitalfangame) and extract the files.
2. **GDRETools/gdsdecomp:** [Download the Latest GDRETools/gdsdecomp](https://github.com/GDRETools/gdsdecomp/releases/latest) to Decompile the game.
3. **Godot**: Having [Godot](https://godotengine.org/download) installed, for this game, version 4.2.2 specifically.
4. **Python (for running):** Having [Python](https://www.python.org/downloads/) installed and added to your system's PATH, to run the local web server.
5. **Git LFS (Optional, for uploading on GitHub):** [Git Large File Storage](https://git-lfs.github.com/) installed on your system. This is crucial for handling large game files in Git.


### Part A: Decompiling

We need to Decompile the game to get the Source Code from the `.exe`, to use to Build the Web Version.

1. Open GDRETools/gdsdecomp -> RE Tools -> Recover project -> Select the Game Executable -> Extract it somewhere.

### Part B: Godot Build Web Version

1. Open the Decompiled Game in Godot 4.2.2.
2. Editor > Manage Export Templates -> Download & Install.
3. Project -> Export -> Add -> Web.
4. In Options, in VRAM Texture Compression, Check For Mobile, and you will get a "Target platform requires 'ETC2/ASTC' texture compression. Enable 'Import ETC2 ASTC' to fix.", where you will just need to click "Fix Import" at the right of it.
5. Export Project to a dedicated, empty subfolder of your root project, name it like `godot` used in this repository for convenience. This subfolder will contain `index.html`, `.js`, `.pck`, `.wasm`, etc.

### Part C: Running the Game Locally

You need to run them using a local web server. Simply opening the `.html` file directly will not work due to browser security restrictions. You will need a slightly complex server setup than the built-in one to run it via Python, since Godot 4.x games require the server to have Cross Origin Isolation & SharedArrayBuffer enabled.

1. [Download this repository's server.py](https://github.com/Nick088Official/TADC-VN-DEMO-CANDY-CHAOS-Web-Port/blob/main/server.py), and place it inside the root project folder.
2. Open a CMD/Terminal **inside the root project folder**.
3. Run the following command to start a web server:
    ```bash
    python server.py
    ```
4. Open your web browser and go to the following address:
    **http://localhost:8000/godot/TADC%20Fangame%20sequel.html**

The game should now start and be fully playable.

### Part D: Git Repository Setup for Vercel (with Git LFS) (Optional)

For deploying to Vercel like I did, it's essential to use Git Large File Storage (Git LFS) for the large game files and configure Vercel-specific settings.

1. **Initialize Git & Git LFS:**
    Navigate to the root of your project where your `godot/` project folder folder resides.
    ```bash
    git init
    git lfs install
    ```
2. **Track Large Files:**
    Tell Git LFS to track your Godot game's package and WebAssembly files. Use:
    ```bash
    git lfs track "web_export/*.pck"
    git lfs track "web_export/*.wasm"
    ```
    This creates/updates the `.gitattributes` file.
3. **Add Vercel Configuration (`vercel.json`):**
    Create a file named `vercel.json` in the **root of your Git repository** (the same level as your `godot/` folder) and add the following content. This ensures Vercel sends the necessary Cross-Origin Isolation headers for Godot 4.x games.
    ```json
    {
      "headers": [
        {
          "source": "/(.*)",
          "headers": [
            {
              "key": "Cross-Origin-Opener-Policy",
              "value": "same-origin"
            },
            {
              "key": "Cross-Origin-Embedder-Policy",
              "value": "require-corp"
            }
          ]
        }
      ]
    }
    ```
4.  **Add and Commit All Files:**
    ```bash
    git add .
    git commit -m "Add Godot web export, Git LFS tracking, and Vercel config"
    ```
5.  **Push to GitHub:**
    Create a new repository on GitHub and push your local changes. Verify on GitHub that `.pck` and `.wasm` files show "Stored with Git LFS".
    
### Part E: Deploying to Vercel
1.  **Create Vercel Project:**
    - Go to your Vercel Dashboard ([https://vercel.com/dashboard](https://vercel.com/dashboard)).
    - Click "Add New..." > "Project".
    - Select "Import Git Repository" and choose the GitHub repository you set up in Part C.
2.  **Configure Vercel Project Settings:**
    - Once the project is created, go to its "Settings" tab.
    - Under "Build & Development Settings":
        - **Root Directory:** If your Godot export files are in a subdirectory like `web_export/` and `vercel.json` is at the root, set the "Root Directory" to `web_export/`. If you flattened your structure and all files (including `vercel.json`) are at the top level, leave this as `.` (default).
    - Under the **"Git"** section (sometimes under "Advanced Settings"):
        - Find **"Git LFS Support"** and **ensure it is ENABLED**. This is critical for Vercel to download your large `.pck` and `.wasm` files.
3.  **Trigger Deployment:**
    - Save your Vercel project settings. This will automatically trigger a new deployment. If not, go to the "Deployments" tab and manually click "Redeploy".
4.  **Verify Deployment:**
    - Check the Vercel deployment's "Resources" tab. Your `.pck` and `.wasm` files should now show their correct, large sizes (not tiny placeholders).
    - Open the Vercel deployment URL in your browser. The game should load past the initial loading bar. Check your browser's Developer Tools (F12) Console for errors and Network tab for correct HTTP headers.


## Why did you make it?

For pure educational purposes, fun, and to make this game accessible on more platforms.


## Credits

- **Game:** All credit for the game's creation goes to **allhailthequeenuwu**. Please support the original creator by visiting the [official itch.io page](https://allhailthequeenuwu.itch.io/theamazingdigitalfangame2demo).
- **Web Port:** This web-based version was ported by [Nick088](https://linktr.ee/nick088).

- **Game Engine:** [Godot](https://godotengine.org/), specifically used version 4.2.2 for this game.



