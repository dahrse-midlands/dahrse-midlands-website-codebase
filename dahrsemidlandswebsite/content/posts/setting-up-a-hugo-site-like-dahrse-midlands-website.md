---
title: "Setting Up a Hugo Site Like DAHRSE-Midlands Website"
date: 2025-01-24T15:10:37Z
draft: false
---
Setting up a Hugo site with the Box template on GitHub Pages using GitHub Codespaces and a new repository can streamline your web development process. This guide walks you through the necessary steps, from installing Hugo in VSCode to creating and configuring your repositories, and finally deploying your site. By following these instructions, you'll have a fully functional Hugo site hosted on GitHub Pages, just like the DAHRSE-Midlands website.

## Assumptions
- Visual Studio Code (VSCode) is already installed. This example uses VSCode within an Ubuntu (WSL2) environment.
- Git is already installed.
- This example used new newly created repositories called “dahrse-midlands.github.io” and "dahrse-midlands-website-codebase".

## Install Hugo in VSCode
Open the terminal in the Codespaces environment and install Hugo:
```bash
sudo apt-get update
sudo apt-get install hugo
```

## Create a New GitHub Repository as a Codebase
1. Go to GitHub and create a new repository named (in this example) `dahrse-midlands-website-codebase`.
2. Make sure the repository is set to public before creating it.

## Create a New GitHub Repository
1. Go to GitHub and create a new repository named `dahrse-midlands.github.io`.
2. Make sure the repository is public before creating it. This repository will be used for the deployment.
3. Do not initialize the repository with a README, .gitignore, or license.

## Clone the Codebase Repository Locally
```bash
git clone [repository link]
# E.g.:
git clone https://github.com/dahrse-midlands/dahrse-midlands-website-codebase.git
```

## Change Directory to Cloned Repository and Create Hugo Site
1. Change directory to the cloned repository:
   ```bash
   cd [repository name]
   # E.g.:
   cd dahrse-midlands-website-codebase/
   ```
2. Create a new Hugo site:
   ```bash
   hugo new site [your_site_name]
   # E.g.:
   hugo new site dahrsemidlandswebsite
   ```

## Change Directory to Newly Created Directory
1. Change directory to the newly created directory:
   ```bash
   cd [directory name]
   # E.g.:
   cd dahrsemidlandswebsite
   ```
   You can use the `ls` command to see different files (e.g., `config.toml`) and sub-folders (e.g., `themes`).

## Change Directory to Newly Created Themes Directory
1. Change directory to the themes directory:
   ```bash
   cd themes
   ```

## Clone the Theme Repository Locally
```bash
git clone [theme repository link]
# E.g.: Using “mainroad” theme:
git clone https://github.com/vimux/mainroad.git themes/mainroad
```

## Set the Newly Cloned Theme, BaseURL, and Title in the `config.toml` File
1. Open the `config.toml` file.
2. Set the theme option to `mainroad`, baseURL, and title (in this example):
   ```toml
   theme = "mainroad"
   baseURL = 'https://dahrse-midlands.github.io/'
   languageCode = 'en-us'
   title = 'DAHRSE-Midlands'
   theme = 'mainroad'
   ```

## Preview the Created Hugo Site
Use the following command:
```bash
hugo server
```
Follow the given localhost link (e.g., `http://127.0.0.1:1313/`) to preview the website via the browser.

## Create a New Post
Use the following command:
```bash
hugo new posts/this-is-my-post.md
# E.g.:
hugo new posts/setting-up-a-hugo-site-like-dahrse-midlands-website.md
```
Open the file and edit:
```markdown
---
title: "Setting Up a Hugo Site Like DAHRSE-Midlands Website"
date: 2025-01-25T15:10:37Z
draft: false
---

Coming soon.
```

## Check Git Status
Use the following command:
```bash
git status
```

## Why Create Two Repositories?
- We want to have a codebase different from where we will deploy the generated static site pages.
- The other repository will be the space for the generated static site pages.

## Add Another Repository as Submodule
1. Ensure at least one commit and main branch in the other new repository.
2. In VSCode, change directory to the folder containing the first repository. In this example, that is `dahrse-midlands-website-codebase`:
   ```bash
   .../github-workspaces/dahrse/dahrse-midlands-website-codebase$
   ```
3. Git clone the other repository meant for deployment:
   ```bash
   .../github-workspaces/dahrse$ git clone https://github.com/dahrse-midlands/dahrse-midlands.github.io.git
   ```
4. Change directory to the new folder:
   ```bash
   cd dahrse-midlands.github.io/
   ```
5. Check out a new branch:
   ```bash
   git checkout -b main
   ```

### Explanation:
The command `git checkout -b main` is used in Git to create and switch to a new branch named `main`. Here's a breakdown of what each part of the command does:
- `git checkout`: This command is used to switch branches or restore working tree files.
- `-b`: This option tells Git to create a new branch before switching to it.
- `main`: This is the name of the new branch you want to create and switch to.

So, when you run `git checkout -b main`, Git will create a new branch called `main` and then switch your working directory to this new branch. This is a common way to start working on a new feature or make changes without affecting the main codebase.

### Create a README File and Commit
```bash
touch README.md
git add .
git commit -m "added readme.md"
```

### Set Your Account's Default Identity
If you get an unknown identity error, run the following commands to set your account's default identity (omit `--global` to set the identity only in this repository):
```bash
git config --global user.email "dahrse.midlands@gmail.com"
git config --global user.name "dahrse-midlands"
```

### Push to the Repository
```bash
git push origin main
```

### Add the Main Branch of the Other Repository as Submodule
1. Change directory to the Hugo site in the first repository:
   ```bash
   .../github-workspaces/dahrse/dahrse-midlands-website-codebase/dahrsemidlandswebsite$ ls
   archetypes  config.toml  content  data  layouts  public  resources  static  themes
   ```
2. Add the main branch of the other repository as submodule:
   ```bash
   git submodule add -b main https://github.com/dahrse-midlands/dahrse-midlands.github.io.git public
   ```

### Note:
- This means that the static files generated from the “codebase” will be added to the public folder (in the other repository) automatically.
- If the public folder exists due to reasons (such as perhaps site generations), you can delete the public folder and re-run the submodule command.
- Check the public folder. You should see the `README.md` file created in the second repository.

### Check if You Like the Already Generated Static Site
```bash
hugo server
```

### Generate Static Files for Deployment
Use the following command:
```bash
hugo -t mainroad
```
Check the public folder to see generated files. You may also check that the git remote setup is correct:
```bash
.../dahrsemidlandswebsite/public$ git remote -v
origin  https://github.com/dahrse-midlands/dahrse-midlands.github.io.git (fetch)
origin  https://github.com/dahrse-midlands/dahrse-midlands.github.io.git (push)
```

### Allow All Actions and Reusable Workflows
Before adding, go to GitHub settings -> Actions -> General -> Allow all actions and reusable workflows. You can update this setting once you complete your deployment.

### Add, Commit, and Push Generated Static Files
```bash
git add .
git commit -m "init commit"
git push origin main
```

### Check Your Deployed Site
Go to your deployed site to see if GitHub Pages worked successfully. In this example, it was `https://dahrse-midlands.github.io/`.

### Further Development
Further develop your site while leveraging the `config.toml` example in the Hugo mainroad theme (if you decide to go the dahrse-midlands way!). We are all learners, so get in touch if necessary!
```

Feel free to adjust any details as needed! If you have any questions or need further assistance, just let me know.

