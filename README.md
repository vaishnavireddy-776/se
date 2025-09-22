1. Secure Authentication for Git (Avoid Username/Password Prompts)
•	Goal: Use SSH keys or a token-based system to avoid repeatedly entering your username and password when interacting with GitHub.
Steps:
1.	Generate SSH key pair if you don't already have one:
2.	ssh-keygen -t ed25519 -C "your_email@example.com"
3.	Add the SSH key to the SSH agent:
4.	eval "$(ssh-agent -s)"
5.	ssh-add ~/.ssh/id_ed25519
6.	Add the SSH public key to GitHub:
o	Copy the public key:
o	cat ~/.ssh/id_ed25519.pub
o	Go to GitHub -> Settings -> SSH and GPG keys -> New SSH key -> Paste the key.
7.	Verify your configuration (check if SSH is being used):
8.	git config --list
You should see something like url = git@github.com:username/repository.git.
________________________________________
2. Fast-forward Merge Conflict Resolution (Keep main as Source of Truth)
•	Goal: Resolve a conflict by keeping the main branch history intact and showing the merge clearly.
Steps:
1.	Make changes on a new branch (feature-branch), then try to merge:
2.	git checkout -b feature-branch
3.	# make changes, commit them
4.	git add .
5.	git commit -m "Work on new feature"
6.	git push origin feature-branch
7.	Merge into main but encounter a conflict:
8.	git checkout main
9.	git merge feature-branch
10.	Resolve the conflict:
o	Open the conflicted files (e.g., index.html, etc.), and manually resolve the conflict.
o	Stage the resolved file:
o	git add <resolved-file>
11.	Finalize the merge:
12.	git commit -m "Merge feature-branch into main, keeping main as the source of truth"
13.	git push origin main
Now the main branch has the final merged version, and the history reflects this.
________________________________________
3. Rearranging Commit History (Parent Before Child)
•	Goal: You committed child.txt before parent.txt but want to rearrange them.
Steps:
1.	Check your commit history:
2.	git log --oneline
3.	Rebase to reorder the commits:
o	Start an interactive rebase:
o	git rebase -i HEAD~2
o	Swap the order of the commits in the editor, changing pick to pick but switching the order:
o	pick <commit-id for parent.txt>
o	pick <commit-id for child.txt>
4.	Finish the rebase and resolve any conflicts if they arise.
5.	Push the updated history:
6.	git push --force origin main
________________________________________
4. Fix Commit Message (Change "fix stuff" to "Fix null pointer issue")
•	Goal: Update a commit message without changing the commit ID.
Steps:
1.	Use git rebase to modify the commit message:
2.	git rebase -i HEAD~1
o	This opens an editor; change pick to reword for the relevant commit.
3.	Edit the commit message in the editor to be more descriptive:
4.	Fix null pointer issue in Service.java
5.	Complete the rebase:
6.	git rebase --continue
7.	Push the commit with the new message:
8.	git push --force origin main
________________________________________
5. Roll Back to a Previous File Version (Test v2, Keep v4)
•	Goal: Temporarily revert a file to its version 2 but keep the latest changes in version 4 intact.
Steps:
1.	Check the file history:
2.	git log --oneline -- <file-path>
3.	Checkout the version 2 of the file:
4.	git checkout HEAD~2 -- <file-path>
5.	Test your changes with the file in its v2 state.
6.	When done, restore the latest (v4) version:
7.	git checkout HEAD -- <file-path>
Now, you've temporarily rolled back the file to v2 without changing v4.
________________________________________
6. Ignore Files Permanently Without gitignore
•	Goal: Prevent certain files (logs, build artifacts, etc.) from being tracked without modifying .gitignore.
Steps:
1.	Set up global git ignore rules:
2.	git config --global core.excludesfile ~/.gitignore_global
3.	Create the .gitignore_global file and add your file patterns:
4.	# ~/.gitignore_global
5.	*.log
6.	*.class
7.	*.iml
8.	Ensure these files are excluded from all your repositories.
________________________________________
7. Save Unfinished Changes Temporarily (Switch Branches Safely)
•	Goal: Stash your changes, switch branches, then restore them later.
Steps:
1.	Stash your changes:
2.	git stash push -m "WIP: Unfinished pom.xml changes"
3.	Switch branches:
4.	git checkout <other-branch>
5.	Apply the stash when you're ready:
6.	git stash pop
________________________________________
8. Create a Patch and Apply It to Another Repo
•	Goal: Create a patch from the latest commit on GitHub and apply it to your local repository.
Steps:
1.	Create a patch file from the GitHub repo:
2.	git format-patch -1 <commit-hash> --stdout > patch.diff
3.	Transfer the patch to your local system, either by downloading it or using a scp/rsync command.
4.	Apply the patch to your local repo:
5.	git apply patch.diff
________________________________________
9. Fetch and Merge (Pulling as Two Separate Steps)
•	Goal: Demonstrate that git pull is a combination of fetch and merge.
Steps:
1.	Fetch updates from the remote repo:
2.	git fetch origin
3.	Merge the fetched changes into your current branch:
4.	git merge origin/main
Verification:
•	git pull is equivalent to:
•	git fetch origin
•	git merge origin/main
________________________________________
10. Add readme.md to Previous Commit
•	Goal: Add readme.md to your previous commit instead of creating a new one.
Steps:
1.	Stage the readme.md file:
2.	git add readme.md
3.	Amend the last commit:
4.	git commit --amend --no-edit
This will keep the same commit message but include the readme.md file in the previous commit.

