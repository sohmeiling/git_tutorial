# Git & GitHub — Student Lesson Guide

A step-by-step reference guide for setting up Git and GitHub using Visual Studio Code. Written for students with little or no prior experience with version control.

> **Platform notes:** Instructions are written primarily for **Windows** users. macOS equivalents are included where the steps differ.

---

## Table of Contents

1. [Prerequisites & Useful Shortcuts](#prerequisites--useful-shortcuts)
2. [Install Git](#step-1--install-git)
3. [Configure Git](#step-2--configure-git)
4. [Set Up SSH Authentication](#step-3--set-up-ssh-authentication)
5. [Prepare VS Code](#step-4--prepare-vs-code)
6. [Create a Repository on GitHub](#step-5--create-a-repository-on-github)
7. [Clone a Repository](#step-6--clone-a-repository)
8. [Stage, Commit & Push](#step-7--stage-commit--push)
9. [Branching](#step-8--branching)
10. [Merging & Resolving Conflicts](#step-9--merging--resolving-conflicts)
11. [Working with Python Virtual Environments](#step-10--working-with-python-virtual-environments)

---

## Prerequisites & Useful Shortcuts

Before you begin, make sure you have:
- A computer running **Windows 10/11** or **macOS**
- An internet connection
- A free [GitHub account](https://github.com)

### Keyboard Shortcuts

| Action | Windows | macOS |
|--------|---------|-------|
| Copy | `Ctrl + C` | `Cmd + C` |
| Paste | `Ctrl + V` | `Cmd + V` |
| Open terminal (VS Code) | `` Ctrl + ` `` | `` Cmd + ` `` |
| Command Palette (VS Code) | `Ctrl + Shift + P` | `Cmd + Shift + P` |

### Show Hidden Folders

Some Git files (e.g., `.ssh`, `.gitconfig`) are stored in hidden folders.

- **Windows:** File Explorer → View → tick **Hidden items**
- **macOS:** Finder → press `Cmd + Shift + .`

---

## Step 1 — Install Git

Git is the version control software that runs locally on your computer. Check whether it is already installed by opening a terminal and running:

```bash
git --version
```

If Git is not installed:

**Windows**
1. Download the installer from [git-scm.com](https://git-scm.com/download/win). The download starts automatically.
2. Run the `.exe` file and click **Next** through all screens — the default settings are fine.
3. When asked to choose a default editor, select **Visual Studio Code** from the dropdown.
4. Once installed, open a new terminal and run `git --version` to confirm.

**macOS**
1. Run `git --version` in the terminal. If Git is not present, macOS will prompt you to install the **Xcode Command Line Tools** — click **Install**.
2. Alternatively, if you have [Homebrew](https://brew.sh) installed:

```bash
brew install git
```

3. Run `git --version` again to confirm the installation.

> 📖 Reference: [Pro Git Book — Chapter 1.5: Installing Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)

---

## Step 2 — Configure Git

This is a one-time setup. Git needs your name and email so that every commit you make is correctly attributed to you.

> ⚠️ Use the **same email address** as your GitHub account.

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
git config --global init.defaultBranch main
```

Verify your settings:

```bash
git config --list
```

You should see `user.name` and `user.email` listed in the output. These values are stored in your global config file (`~/.gitconfig`).

> 📖 Reference: [Pro Git Book — Chapter 1.6: First-Time Git Setup](https://git-scm.com/book/en/v2/Getting-Started-First-Time-Git-Setup)

---

## Step 3 — Set Up SSH Authentication

SSH allows your computer to communicate with GitHub securely, without requiring you to enter a password every time you push or pull code.

### Generate an SSH Key Pair

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```

When prompted:
- Press `Enter` to accept the default file location
- Press `Enter` twice to skip setting a passphrase

Your keys will be saved to:
- **Windows:** `C:\Users\<username>\.ssh\`
- **macOS:** `/Users/<username>/.ssh/`

Two files are created: `id_ed25519` (private key — keep this on your computer only) and `id_ed25519.pub` (public key — this is what you share with GitHub).

### Copy Your Public Key

**Windows (PowerShell):**
```powershell
Get-Content $env:USERPROFILE\.ssh\id_ed25519.pub
```

**macOS:**
```bash
pbcopy < ~/.ssh/id_ed25519.pub
```

### Add the Key to GitHub

1. Go to [github.com/settings/keys](https://github.com/settings/keys)
2. Click **New SSH key**
3. Give it a title (e.g., `My Laptop`) and paste your public key into the **Key** field
4. Click **Add SSH key**

### Test the Connection

```bash
ssh -T git@github.com
```

If successful, you will see:
```
Hi your-username! You've successfully authenticated, but GitHub does not provide shell access.
```

> ℹ️ If asked "Are you sure you want to continue connecting?", type `yes` and press `Enter`. This only appears the first time.

> 📖 Reference: [GitHub Docs — Connecting to GitHub with SSH](https://docs.github.com/en/authentication/connecting-to-github-with-ssh)

---

## Step 4 — Prepare VS Code

### Install VS Code

Download from [code.visualstudio.com](https://code.visualstudio.com/) and follow the installer instructions.

### Install Recommended Extensions

Open the Extensions panel (`Ctrl + Shift + X`) and install the following:

| Extension | Publisher | Purpose |
|-----------|-----------|---------|
| **GitHub Pull Requests** | GitHub | Connect VS Code to GitHub; publish and manage repositories |
| **GitLens** | GitKraken | View commit history, blame annotations, and file comparisons |

> ⚠️ Always verify the publisher name before installing — there may be extensions with similar names from different authors.

### Open the Integrated Terminal

- **Windows:** `` Ctrl + ` `` or **View → Terminal**
- **macOS:** `` Cmd + ` `` or **View → Terminal**

The default terminal type is **PowerShell** (Windows) or **zsh/bash** (macOS). You can also switch to **Git Bash** on Windows via the terminal dropdown.

---

## Step 5 — Create a Repository on GitHub

1. Log in to [github.com](https://github.com)
2. Click the **+** icon (top right) → **New repository**
3. Fill in the repository name (use lowercase and hyphens, e.g., `my-project`)
4. Optionally add a description
5. Choose **Public** or **Private**
6. Tick **Add a README file**
7. Click **Create repository**

Once created, copy the repository's **SSH URL** (click the green **Code** button → **SSH** tab). It will look like:
```
git@github.com:your-username/your-repo-name.git
```

> ℹ️ Use the SSH URL, not HTTPS, since you have already set up SSH authentication in Step 3.

---

## Step 6 — Clone a Repository

Cloning downloads a copy of the repository from GitHub to your computer.

### Method A — VS Code GUI (Recommended)

1. In VS Code, press `Ctrl + Shift + P` (or `Cmd + Shift + P` on macOS) to open the **Command Palette**
2. Type `Git: Clone` and press `Enter`
3. Paste the SSH URL of your repository
4. A folder browser will open — navigate to where you want to save the project and click **Select as Repository Destination**
5. When prompted, click **Open** to open the cloned repository

### Method B — Terminal

Navigate to your chosen folder first, then run:

```bash
git clone git@github.com:<owner>/<repo>.git
cd <repo>
```

---

## Step 7 — Stage, Commit & Push

Every time you make changes you want to save, you will follow this three-step process: **stage → commit → push**.

| Step | What it means |
|------|--------------|
| **Stage** | Select which changed files to include in the next save |
| **Commit** | Save a labelled snapshot of the staged files |
| **Push** | Upload your commits to GitHub |

### Using the VS Code Source Control Panel

1. Click the **Source Control** icon in the left sidebar (`Ctrl + Shift + G`)
2. Hover over a file under **Changes** and click **+** to stage it, or hover over the **Changes** heading and click **+** to stage all files
3. Type a short, descriptive message in the **Message** box (e.g., `Add score variable`)
4. Press `Ctrl + Enter` (Windows) or `Cmd + Enter` (macOS), or click the **✓ Commit** button
5. Click **Sync Changes** or the push icon to upload to GitHub

### Using the Terminal

```bash
# Check which files have changed
git status

# Stage all changed files
git add .

# Stage a specific file only
git add filename.py

# Commit with a message
git commit -m "Add score variable"

# Push to GitHub
git push
```

For the first push of a new branch:
```bash
git push -u origin main
```

### Pulling Changes

Always pull before you start working, especially when collaborating with others:

```bash
git pull
```

> 📖 Reference: [Pro Git Book — Chapter 2.2: Recording Changes to the Repository](https://git-scm.com/book/en/v2/Git-Basics-Recording-Changes-to-the-Repository)

---

## Step 8 — Branching

Branches let you work on new features or fixes without affecting the stable `main` branch.

### Create a Branch

**VS Code GUI:**
1. Click the branch name in the **bottom-left corner** of VS Code (e.g., `main`)
2. Select **Create new branch…**
3. Type a descriptive name (e.g., `feature-score-system`) and press `Enter`

**Terminal:**
```bash
# Create a new branch and switch to it immediately
git checkout -b feature-score-system

# Check which branch you are currently on
git branch

# Switch to an existing branch
git checkout main
```

### Push a New Branch to GitHub

```bash
# First push of a new branch
git push -u origin feature-score-system

# Subsequent pushes
git push
```

After pushing, go to your repository on GitHub. You will see a banner — click **Compare & pull request** to open a Pull Request (PR) for review and merging.

> 📖 Reference: [Pro Git Book — Chapter 3.2: Basic Branching and Merging](https://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging)

---

## Step 9 — Merging & Resolving Conflicts

### Merge with No Conflicts

1. On GitHub, open the Pull Request and click **Merge pull request → Confirm**
2. Update your local `main` branch:

```bash
git checkout main
git pull origin main
```

### Resolving a Merge Conflict

A conflict occurs when two people edit the same part of the same file. Git marks the conflicting section like this:

```
<<<<<<< main
score = 0
=======
score = 100
>>>>>>> feature-score-system
```

**VS Code GUI:**
- Open the conflicted file — VS Code highlights the conflict and shows buttons: **Accept Current Change**, **Accept Incoming Change**, or **Accept Both Changes**
- Click the appropriate option, or edit the file manually to combine both changes
- Save the file, then stage and commit

**Terminal:**
1. Open the conflicted file in your editor and manually remove the conflict markers, keeping the correct code
2. Stage, commit, and push the resolved file:

```bash
git add filename.py
git commit -m "Resolve merge conflict in filename.py"
git push
```

> 📖 Reference: [Pro Git Book — Chapter 3.3: Branch Management](https://git-scm.com/book/en/v2/Git-Branching-Branch-Management)

---

## Step 10 — Working with Python Virtual Environments

A virtual environment keeps your project's Python packages isolated from other projects on your computer.

### Check for an Existing Virtual Environment

```bash
# Windows
dir .venv

# macOS/Linux
ls -a
```

Look for a `.venv` or `env` folder inside your project directory.

### Create a Virtual Environment

```bash
# Windows
python -m venv .venv

# macOS/Linux
python3 -m venv .venv
```

### Activate the Virtual Environment

```bash
# Windows (PowerShell)
.venv\Scripts\Activate.ps1

# Windows (Git Bash)
source .venv/Scripts/activate

# macOS/Linux
source .venv/bin/activate
```

When activated, your terminal prompt will show `(.venv)` at the start.

To deactivate:
```bash
deactivate
```

### Install Dependencies and Run Your Project

```bash
pip install pygame

# Run your script
python pingpong.py      # Windows
python3 pingpong.py     # macOS/Linux
```

### Select the Python Interpreter in VS Code

VS Code sometimes needs to be told which Python interpreter to use:

1. Open the Command Palette (`Ctrl + Shift + P` / `Cmd + Shift + P`)
2. Type `Python: Select Interpreter`
3. Choose the interpreter inside your project's `.venv`:
   - **Windows:** `.\.venv\Scripts\python.exe`
   - **macOS/Linux:** `./.venv/bin/python3`

The status bar at the bottom right of VS Code will confirm the active environment.

> 📖 References:
> - [Python venv documentation](https://docs.python.org/3/library/venv.html)
> - [Pygame installation guide](https://www.pygame.org/wiki/GettingStarted)

---

## Quick Reference — Common Git Commands

```bash
git status                        # Show changed files
git add .                         # Stage all changes
git add filename.py               # Stage a specific file
git commit -m "Your message"      # Commit with a message
git push                          # Push to GitHub
git pull                          # Pull latest from GitHub
git checkout -b branch-name       # Create and switch to a new branch
git checkout main                 # Switch back to main
git log --oneline                 # View recent commits
git restore filename.py           # Discard changes to a file
```

---

## Summary

| Step | What you set up |
|------|----------------|
| 1 | Git installed on your computer |
| 2 | Git configured with your name and email |
| 3 | SSH key created and added to GitHub |
| 4 | VS Code ready with Git extensions |
| 5 | Repository created on GitHub |
| 6 | Repository cloned to your computer |
| 7 | Files staged, committed, and pushed |
| 8 | Feature branches created and pushed |
| 9 | Pull Requests merged and conflicts resolved |
| 10 | Python virtual environment set up and activated |
