# Advanced Git Topics

Below is a list of tasks if you want to deepen your understanding of Git.




## Top 20 Git Commands

You should be able to explain what each of these do:

*Starting up*
- [ ] [`git config`](# "Set the author name and email to be used with your commits")
- [ ] [`git init`](# "Create a new local repository")
- [ ] [`git clone <url>`](# "Check out a repository")

*Workflow*
- [ ] [`git status`](# "List the files you've changed and those you still need to add or commit")
- [ ] [`git diff`](# "Preview changes, before merging")
- [ ] [`git add <filenames>`](# "Add one or more files to staging")
- [ ] [`git commit -m "message"`](# "Commit changes")

*Remote*
- [ ] [`git push`](# "Send changes to the master branch of your remote repository")
- [ ] [`git pull`](# "Fetch and merge changes on the remote server to your working directory")
- [ ] [`git remote`](# "List all currently configured remote repositories")

*Branching*
- [ ] [`git branch`](# "List all the branches in your repo, and also tell you what branch you're currently in")
- [ ] [`git checkout -b <branchname>`](# "Create a new branch and switch to it")
- [ ] [`git checkout <branchname>`](# "Switch from one branch to another")
- [ ] [`git merge`](# "To merge a different branch into your active branch")

*Maintaining order*
- [ ] [`git reset`](# "Unstages files, preserving the file contents")
- [ ] [`git rm <filename>`](# "Delete the file from your working directory and stages the deletion")
- [ ] [`git log`](# "List the version history for the current branch")
- [ ] [`git show`](# "Show the metadata and content changes of the specified commit")
- [ ] [`git tag`](# "You can use tagging to mark a significant changeset")
- [ ] [`git stash`](# "Temporarily stores all the modified tracked files")





## Fetch from Upstream
You should be able to sync a fork of a repository to keep it up-to-date with the upstream repository.

- [ ] With Github Desktop...
  - [ ] While in the default branch (`master`) switch to the history tab
  - [ ] Select the branch called `upstream/master` and click "Merge into master"
- [ ] Or, on command line
  - [ ] Fetch project branches from the upstream repository to get all the commits:
  ```
  git fetch upstream
  ```
  - [ ] Check out the master branch from your local fork
  ```
  git checkout master
  ```
  - [ ] Merge the changes from `upstream/master` into your local `master` branch. Your fork’s master branch will be in sync with the upstream repository. You will not lose your local changes:
  ```
  git merge upstream/master
  ```



## Publish to Github from a Remote Server or Raspberry Pi

You can push and pull commits from any computer, including a headless remote server or Raspberry Pi, over SSH. Give each device its own SSH key added to your Github account so it can authenticate on its own.

### Create and install an SSH key

1. On the remote server/Pi, check whether a key already exists
```bash
ls -al ~/.ssh
```
2. If none exists, generate a new key pair (replace the email with your own; press return to accept the default file location, and add a passphrase if you'd like one)
```bash
ssh-keygen -t ed25519 -C "youraddress@example.com"
```
3. Start the ssh-agent and add your new key
```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```
4. Display (and copy) the public key
```bash
cat ~/.ssh/id_ed25519.pub
```
5. Add it to Github: Settings > [SSH and GPG keys](https://github.com/settings/keys) > New SSH key. Give it a descriptive title (e.g. `raspberry-pi-classroom`) so you know which device it belongs to.
6. Test the connection from your server
```bash
ssh -T git@github.com
# -> Hi username! You've successfully authenticated...
```

> Generate a **separate key on each device** (laptop, Pi, remote server) rather than copying one private key around. That way you can revoke a single device's access (delete its key from Github) without affecting the others.

### Clone and configure the repository

1. On the remote server/Pi, clone using the SSH url, not the HTTPS one (Github.com repo page > Code button > SSH)
```bash
git clone git@github.com:username/repo-name.git
```
2. Confirm the remote is using SSH
```bash
git remote -v
```
3. Set your name and email on this device too (see [Install & Configure Git](how-to/install-configure.md))
```bash
git config --global user.name "Your Name"
git config --global user.email youraddress@example.com
```

### Keep content consistent across computer, Github, and remote server/Pi

Treat Github.com as the single source of truth, and sync every device through it rather than having devices talk to each other directly.

```
your computer  <-->  Github.com  <-->  remote server / Pi
```

- [ ] `git pull` at the start of a session, on whichever device you're using, before making changes
- [ ] `git push` as soon as you finish a change, so the other devices can pull it later
- [ ] `git status` before pulling, to make sure you don't have uncommitted local changes that could conflict
- [ ] Avoid editing the same files on two devices without pulling in between — if you do, `git pull` will attempt an automatic merge and may leave you a conflict to resolve by hand
- [ ] If a device has been offline a while, `git fetch` then `git diff main origin/main` to preview incoming changes before merging them in with `git pull`
- [ ] If the remote server/Pi needs to always run the latest `main` (e.g. it's hosting something live), consider automating `git pull` there with a cron job or a post-push webhook, instead of pulling by hand


