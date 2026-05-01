# GitHub Starter Guide

A step-by-step guide for absolute beginners. No prior experience required.

---

## Table of Contents

1. [Create a GitHub Account](#1-create-a-github-account)
2. [Install Git](#2-install-git)
3. [Install the GitHub CLI](#3-install-the-github-cli)
4. [Log In with the GitHub CLI](#4-log-in-with-the-github-cli)
5. [Verify Everything Works](#5-verify-everything-works)
6. [Open a Pull Request (PR)](#6-open-a-pull-request-pr)
7. [What Now?](#7-what-now)

---

## 1. Create a GitHub Account

1. Open your web browser and go to **https://github.com**
2. Click the **Sign up** button in the top-right corner.
3. Enter your **email address** and click Continue.
4. Choose a **password** (at least 15 characters, OR at least 8 characters with a number and a lowercase letter).
5. Pick a **username** — this will be your public identity on GitHub (e.g. `janedoe42`). It can only contain letters, numbers, and hyphens.
6. GitHub will ask if you want to receive email updates — type `y` or `n` and continue.
7. Solve the puzzle to prove you're a human, then click **Create account**.
8. Check your email for a **verification code**, enter it on the GitHub page, and you're in.

> **Tip:** Bookmark https://github.com — you'll be coming back a lot.

---

## 2. Install Git

Git is the version-control tool that runs on your computer. GitHub is the website that hosts your code. You need both.

### Mac

Open the app called **Terminal** (search for "Terminal" in Spotlight — press `Cmd + Space` and type `Terminal`).

Paste this command and press Enter:

```
git --version
```

- If Git is already installed, you'll see a version number like `git version 2.39.0`. You can skip ahead.
- If Git is **not** installed, macOS will pop up a dialog asking you to install the **Command Line Developer Tools**. Click **Install**, agree to the license, and wait. This gives you Git (and other useful tools) without needing Homebrew or anything else.

After installation finishes, run `git --version` again to confirm.

### Windows

1. Go to **https://git-scm.com/download/win** — the download should start automatically.
2. Run the installer (`.exe` file).
3. Click **Next** through each screen. The defaults are fine for beginners — don't change anything unless you know what it does.
4. On the "Adjusting your PATH" screen, make sure **"Git from the command line and also from 3rd-party software"** is selected (it's the default).
5. Keep clicking **Next**, then **Install**, then **Finish**.
6. Open the app called **Git Bash** (search for it in the Start menu). Type:

```
git --version
```

You should see a version number. You're good.

### Linux (Ubuntu / Debian)

Open a terminal and run:

```
sudo apt update
sudo apt install git -y
```

Then verify:

```
git --version
```

### Linux (Fedora)

```
sudo dnf install git -y
```

---

## 3. Install the GitHub CLI

The GitHub CLI (`gh`) lets you do GitHub things — create repos, open pull requests, manage issues — right from your terminal instead of switching to the browser.

### Mac (no Homebrew needed)

1. Go to **https://github.com/cli/cli/releases/latest**
2. Scroll down to **Assets**.
3. Download the file that ends in `_macOS_amd64.zip` (Intel Mac) or `_macOS_arm64.zip` (Apple Silicon — M1/M2/M3/M4 Mac).

   > **Not sure which Mac you have?** Click the Apple logo in the top-left of your screen → **About This Mac**. If it says "Apple M1" or "Apple M2" (or similar), you have Apple Silicon. If it says "Intel", download the `amd64` version.

4. Double-click the `.zip` file to unzip it — you'll get a folder.
5. Open **Terminal** and run these commands (adjust the folder name if the version number is different):

```
cd ~/Downloads
```

If the extracted folder is named something like `gh_2.74.0_macOS_arm64`, run:

```
sudo cp gh_*_macOS_*/bin/gh /usr/local/bin/
```

Type your Mac password when prompted (you won't see characters as you type — that's normal).

6. Verify it worked:

```
gh --version
```

You should see something like `gh version 2.74.0`.

### Windows

1. Go to **https://github.com/cli/cli/releases/latest**
2. Scroll down to **Assets**.
3. Download `gh_X.X.X_windows_amd64.msi` (the `.msi` file).
4. Run the installer. Click **Next** through each screen, then **Install**, then **Finish**.
5. **Close and reopen** Git Bash (or Command Prompt / PowerShell).
6. Type:

```
gh --version
```

You should see the version number.

### Linux (Ubuntu / Debian)

Run these commands one at a time:

```
sudo mkdir -p /etc/apt/keyrings

curl -fsSL https://cli.github.com/packages/githubcli-archive-keyring.gpg | sudo tee /etc/apt/keyrings/githubcli-archive-keyring.gpg > /dev/null

sudo chmod go+r /etc/apt/keyrings/githubcli-archive-keyring.gpg

echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" | sudo tee /etc/apt/sources.list.d/github-cli.list > /dev/null

sudo apt update

sudo apt install gh -y
```

Then verify:

```
gh --version
```

### Linux (Fedora)

```
sudo dnf install 'dnf-command(config-manager)' -y
sudo dnf config-manager --add-repo https://cli.github.com/packages/rpm/gh-cli.repo
sudo dnf install gh -y
```

---

## 4. Log In with the GitHub CLI

Now you need to connect the CLI to your GitHub account.

In your terminal, run:

```
gh auth login
```

You'll be asked a series of questions. Here's what to pick:

| Prompt | Choose |
|---|---|
| Where do you use GitHub? | **GitHub.com** |
| What is your preferred protocol? | **HTTPS** |
| Authenticate Git with your GitHub credentials? | **Yes** |
| How would you like to authenticate? | **Login with a web browser** |

It will show you a one-time code. Press **Enter**, and your browser will open a GitHub page. Paste the code there, click **Authorize**, and you're done.

Back in your terminal, you should see a success message.

> **Tip:** To confirm you're logged in at any time, run `gh auth status`.

---

## 5. Verify Everything Works

Run these three commands:

```
git --version
gh --version
gh auth status
```

If all three give you output without errors, you are fully set up. Nice work.

---

## 6. Open a Pull Request (PR)

A **pull request** is a request to merge your changes into another branch (usually `main`). It lets others review your work before it becomes part of the shared project.

**Before you start:** Clone the repo and `cd` into it, or open a folder that is already a Git repo. If you are not sure, run `git status` — if you see branch info and file lists, you are in a repo.

### Step 1: Get the latest code

```
git checkout main
git pull origin main
```

(If your default branch is named `master`, use that instead of `main`.)

### Step 2: Create a branch for your work

Pick a short, descriptive branch name (for example `add-sandbox-readme` or `fix-typo-in-guide`):

```
git checkout -b your-branch-name
```

### Step 3: Make your changes

Edit files, add new ones, or move things around. The `sandbox/` folder is a good place for experiments that should not mix with official team areas.

### Step 4: Save your work in Git

```
git status
git add .
git commit -m "Describe your change in one short sentence"
```

Use a clear commit message so reviewers (and future you) know what changed.

### Step 5: Push your branch to GitHub

The first time you push this branch:

```
git push -u origin your-branch-name
```

After that, `git push` is enough for the same branch.

### Step 6: Open the PR on GitHub

**Option A — In the browser:** After `git push`, GitHub often shows a yellow banner with **Compare & pull request**. Click it, add a title and a short description of what you did and why, then click **Create pull request**.

**Option B — From the terminal** (if you use the GitHub CLI from [section 3](#3-install-the-github-cli)):

```
gh pr create
```

Follow the prompts for title and body, or pass them directly:

```
gh pr create --title "Your PR title" --body "What changed and why."
```

### After the PR is open

- Respond to review comments and push more commits to the same branch; they appear on the PR automatically.
- When it is approved, someone with permission merges it (or you do, depending on the repo).
- Locally, switch back to main and pull the merged work:

```
git checkout main
git pull origin main
```

---

## 7. What Now?

Here are some things to try:

**Create your first repo from the terminal:**

```
gh repo create my-first-repo --public --clone
cd my-first-repo
echo "# Hello World" > README.md
git add README.md
git commit -m "first commit"
git push
```

**Clone someone else's repo:**

```
gh repo clone owner/repo-name
```

**Learn more:**

- [GitHub's official "Hello World" tutorial](https://docs.github.com/en/get-started/start-your-journey/hello-world)
- [Git handbook](https://docs.github.com/en/get-started/using-git/about-git)
- [GitHub CLI manual](https://cli.github.com/manual/)

---

## Troubleshooting

**"command not found: git"** — Git isn't installed or isn't on your PATH. Re-follow the Git installation steps for your OS.

**"command not found: gh"** — The GitHub CLI isn't installed or isn't on your PATH. On Mac, make sure you copied `gh` to `/usr/local/bin/`. On Windows, make sure you reopened your terminal after installing.

**"permission denied"** on Mac/Linux — You probably need `sudo` in front of the command. That runs it as an administrator.

**My password isn't showing when I type it in Terminal** — That's normal. Terminal hides passwords for security. Just type it and press Enter.

**"fatal: not a git repository"** — You need to `cd` into a folder that has a Git repo, or run `git init` to start one.

---

*Made with care for first-timers.*
