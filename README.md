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

1. **Get the Game Files:** [Download the original game from the creator's itch.io page](https://allhailthequeenuwu.itch.io/theamazingdigitalfangame2demo) and extract the files.
2. **Godot RE Tools:** [Download the Latest GDRETools/gdsdecomp](https://github.com/GDRETools/gdsdecomp/releases/latest) to Decompile the game.
3. **Godot**: Having [Godot](https://godotengine.org/download) installed, for this game, version [4.2.2-stable](https://github.com/godotengine/godot-builds/releases/tag/4.2.2-stable) specifically.
4. **Python (for running):** Having [Python](https://www.python.org/downloads/) installed and added to your system's PATH, to run the local web server.
5. **Git LFS (Optional, for uploading on GitHub):** [Git Large File Storage](https://git-lfs.github.com/) installed on your system. This is crucial for handling large game files in Git.


### Part A: Decompiling

We need to Decompile the game to get the Source Code from the `.exe`, to use to Build the Web Version.

1. Open Godot RE Tools -> RE Tools -> Recover project -> Select the Game Executable -> Extract it somewhere.

### Part B: Godot Build Web Version

1. Import the Decompiled Game in Godot [4.2.2-stable](https://github.com/godotengine/godot-builds/releases/tag/4.2.2-stable).
2. Editor > Manage Export Templates -> Download & Install.
3. Project -> Export -> Add -> Web.
4. In Options, in VRAM Texture Compression, Check For Mobile, and you will get a "Target platform requires 'ETC2/ASTC' texture compression. Enable 'Import ETC2 ASTC' to fix.", where you will just need to click "Fix Import" at the right of it.
5. Export Project to a dedicated, empty subfolder of your root project, name it like `godot` used in this repository for convenience. This subfolder will contain `index.html`, `.js`, `.pck`, `.wasm`, etc.

  You may also consider:
  - Export with Debug: useful for checking errors in the browser console, though it produces a slightly larger and slower build.  
  - Save the file name as `index.html`: required for hosting on certain platforms (like itch.io).

### Part C: Running the Game Locally

You need to run them using a local web server. Simply opening the `.html` file directly will not work due to browser security restrictions. You will need a slightly complex server setup than the built-in one to run it via Python, since Godot 4.0-4.2 games are multi-threaded, they require the server to have Cross Origin Isolation & SharedArrayBuffer enabled.

1. Create a file named `server.py` in the **root of your Git repository** (the same level as your `godot` folder) and add the following content:
    ```py
    import http.server
    import socketserver

    PORT = 8000

    class Handler(http.server.SimpleHTTPRequestHandler):
        def end_headers(self):
            self.send_header("Cross-Origin-Opener-Policy", "same-origin")
            self.send_header("Cross-Origin-Embedder-Policy", "require-corp")
            super().end_headers()

    with socketserver.TCPServer(("", PORT), Handler) as httpd:
        print("serving at port", PORT)
        httpd.serve_forever()
    ```
2. Open a CMD/Terminal **inside the root project folder**.
3. Run the following command to start a web server:
    ```bash
    python server.py
    ```
4. Open your web browser and go to the following address:
    **http://localhost:8000/godot/TADC%20Fangame%20sequel.html**

The game should now start and be fully playable.

### Part D: Git Repository Setup (For Vercel) (with Git LFS) (Optional)

For uploading the repository to GitHub, it's essential to use Git Large File Storage (Git LFS) for the large game files and configure Vercel-specific settings.

1. Initialize Git & Git LFS to the root of your project where your `godot` project folder folder resides via running:
    ```bash
    git init
    git lfs install
    ```
2. Tell Git LFS to track your Godot game's package and WebAssembly files. Use:
    ```bash
    git lfs track "godot/*.pck"
    git lfs track "godot/*.wasm"
    ```
    This creates/updates the `.gitattributes` file.
3. Create a file named `vercel.json` in the **root of your Git repository** (the same level as your `godot` folder) and add the following content. This ensures Vercel sends the necessary Cross-Origin Isolation headers for Godot 4.0-4.2 games.
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
4. Add and Commit All Files via:
    ```bash
    git add .
    git commit -m "Add Godot web export, Git LFS tracking, and Vercel config"
    ```
5. Create a new repository on GitHub and push your local changes. Verify on GitHub that `.pck` and `.wasm` files show "Stored with Git LFS".
    
### Part E: Deploying to Vercel
1. Go to your [Vercel Dashboard](https://vercel.com/dashboard).
2. Click "Add New..." > "Project".
3. Select "Import Git Repository" and choose the GitHub repository you set up in Part C.
4. Once the project is created, go to its "Settings" tab, go to Git -> Git LFS Support and ensure it is ENABLED. This is critical for Vercel to download your large `.pck` and `.wasm` files.
5. Save your Vercel project settings. This will automatically trigger a new deployment. If not, go to the "Deployments" tab and manually click "Redeploy".
6. Open the Vercel deployment URL in your browser. The game should load past the initial loading bar. Check your browser's Developer Tools (F12) Console for errors and Network tab for correct HTTP headers.


## Why did you make it?

For pure educational purposes, fun, and to make this game accessible on more platforms.


## Credits

- **Game:** All credit for the game's creation goes to **allhailthequeenuwu**. Please support the original creator by visiting the [official itch.io page](https://allhailthequeenuwu.itch.io/theamazingdigitalfangame2demo).
- **Web Port:** This web-based version was ported by [Nick088](https://linktr.ee/nick088).
- **Game Engine:** [Godot](https://godotengine.org/), specifically used version [4.2.2-stable](https://github.com/godotengine/godot-builds/releases/tag/4.2.2-stable) for this game.