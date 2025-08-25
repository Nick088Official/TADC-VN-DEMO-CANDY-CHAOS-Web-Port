# TADC VN 1ST DAY - Web Port

This is an **Unofficial Web Port** of the free RPG Maker MV horror game [**TADC VN DEMO CANDY CHAOS (aka The Amazing Digital Circus Visual Novel Fangame 2 Demo) by allhailthequeenuwu**](https://allhailthequeenuwu.itch.io/theamazingdigitalfangame2demo). This port allows the game to be played directly in a web browser without needing any downloads/installations.

## How to play

There are 2 methods:

### Vercel
Go to <PLACEHOLDER_LINK>

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


### Part A: Decompiling

We need to Decompile the game to get the Source Code from the `.exe`, to use to Build the Web Version.

1. Open GDRETools/gdsdecomp -> RE Tools -> Recover project -> Select the Game Executable -> Extract it somewhere.

### Part B: Godot Build Web Version

1. Open the Decompiled Game in Godot 4.2.2.
2. Editor > Manage Export Templates -> Download & Install.
3. Project -> Export -> Add -> Web.
4. In Options, in VRAM Texture Compression, Check For Mobile, and you will get a "Target platform requires 'ETC2/ASTC' texture compression. Enable 'Import ETC2 ASTC' to fix.", where you will just need to click "Fix Import" at the right of it.
5. Export Project somewhere.

### Part C: Running the Game Locally

You need to run them using a local web server. Simply opening the `.html` file directly will not work due to browser security restrictions. You will need a slightly complex server setup than the built-in one to run it via Python, since Godot 4.x games require the server to have Cross Origin Isolation & SharedArrayBuffer enabled.

1. Open a CMD/Terminal **inside the Web Exported project folder**.
2. Run the following command to start a web server:
    ```bash
    python server.py
    ```
3  Open your web browser and go to the following address:
    **http://localhost:8000/godot/TADC%20Fangame%20sequel.html**

The game should now start and be fully playable.


## Why did you make it?

For pure educational purposes, fun, and to make this game accessible on more platforms.


## Credits

- **Game:** All credit for the game's creation goes to **allhailthequeenuwu**. Please support the original creator by visiting the [official itch.io page](https://allhailthequeenuwu.itch.io/theamazingdigitalfangame2demo).
- **Web Port:** This web-based version was ported by [Nick088](https://linktr.ee/nick088).
- **Game Engine:** [Godot](https://godotengine.org/), specifically used version 4.2.2 for this game.