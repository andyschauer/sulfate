# How to contribute

This document describes two different ways to modify the sulfate.Rmd script; both workflows employ Git. One option lets you live entirely within RStudio with one hand comfortably on the mouse; the other option assumes you have Git installed on your local computer and enjoy working from the command line. Many other workflows exist, of course, so please feel free to use your own preferred approach. You will certainly need a GitHub account in any case.


## RStudio ONLY

Since our RStudio has Git installed, we are going to start by creating a project which only needs to be completed once.

### Create a sulfate project

>Click: File → New Project → Version Control → Git

>Paste the GitHub repo URL:

```
		https://github.com/andyschauer/sulfate.git
```

>Choose a folder in your server home directory. If you leave it as a default, you will end up with a sulfate directory in your home directory.

>Click Create Project.

>RStudio Server will:

>>Clone the repo on the server

>>Create a project

>>Enable the Git pane (upper right pane, Git tab)


### Daily workflow (all inside RStudio Server GUI):

1. Pull the latest version from GitHub:

Switch to branch main

Click Pull

2. Create a new branch for your changes:

Git pane → New Branch

Name: sulfate-updates-yymmdd

3. Edit files in the RStudio editor.

4. Stage + Commit

Use the Git pane checkboxes

Add a commit message

Click Commit

5. Push

Click the Push button

First push will set upstream automatically

6. Open pull request (PR) on GitHub

7. After merge → RStudio Server →

Switch to main

Pull


## git command line option:
0. One-time setup (already done, but for completeness)

	On your local machine:

		cd ~/sulfate      # or wherever the repo lives
		git remote -v     # should show origin -> github.com/andyschauer/sulfate


	If that shows origin pointing at GitHub, you’re good.


1. Start a new round of work (always begin this way)

	Goal: Make sure your local main matches GitHub’s main.

	On your local machine:

		cd ~/sulfate

		git checkout main     # switch to main
		git pull              # update main from GitHub


	Now local main is current.


2. Create a feature branch for your changes

	Goal: Don’t touch main directly; work on a branch.

	On your local machine:

	from inside the repo, with main up to date:

		git checkout -b feature-xyz

	feature-xyz is any name you choose that describes the work
	(e.g. sulfate-figure-improvements, add-perm-tube-section, etc.)

	You are now on feature-xyz, starting from the latest main.


3. Make changes locally and commit them

	Goal: Edit files, then create commits.

	On your local machine:

	edit files in RStudio / Sublime / etc.

		git status                   # see what changed
		git add sulfate.Rmd          # or specific files
	
	or to add everything you changed:
		
		git add .

		git commit -m "Describe what you changed"


	You can repeat this edit → add → commit cycle as many times as needed.


	4. Push the new branch to GitHub

	Goal: Get your branch onto GitHub so you can make a Pull Request.

	First push (new branch):

		git push -u origin feature-xyz


	-u sets the “upstream” so later you can just do git push.

	After this, your branch exists both:

	Locally: feature-xyz

	On GitHub: origin/feature-xyz


5. (Optional but important) If others may have updated main

	If time passes and other people may have merged things into main, update your branch before doing a PR.

	On your local machine:

	# Step 1: get latest main from GitHub
		git checkout main
		git pull

	# Step 2: go back to your branch, and merge main into it
		git checkout feature-xyz
		git merge main


	If there are no conflicts, you’ll just get a merge commit, then:

		git push


	If there are conflicts, you’ll see messages like:

	CONFLICT (content): Merge conflict in sulfate.Rmd


	Then:

		git status                  # see which files are conflicted
		# edit each conflicted file to resolve <<<<<<< ======= >>>>>> markers
		git add sulfate.Rmd         # mark conflicts resolved
		git commit                  # completes the merge
		git push                    # update branch on GitHub


	Now your feature branch is up to date with main + your changes.


6. Open a Pull Request and merge into main (maintainer does this on GitHub)

	This happens in the GitHub web UI.

	Go to your repo on GitHub: andyschauer/sulfate

	You’ll usually see a banner: “Compare & pull request” for feature-xyz
	– click it
	Or manually:

	Go to Pull requests → New pull request

	Base: main

	Compare: feature-xyz

	Review the diff (this is GitHub’s view of what you’re proposing to change)

	If GitHub shows conflicts, you can resolve them there, but you already know how to do it locally (which is often nicer).

	When happy, click Merge (e.g. “Create a merge commit”).

	After this, on GitHub:

	main now includes all changes from feature-xyz.


7. Update your local main to match GitHub

	Back on your local machine:

		git checkout main
		git pull


	Now local main == GitHub main (with your new changes included).


8. (Optional but tidy) Delete the feature branch

	After the PR is merged and you’re sure you’re done with it:

	On GitHub:

	On the PR page, click “Delete branch” (if you want).

	On your local machine:

	git branch -d feature-xyz         # safe delete (refuses if not merged)
	# and if you also want to remove it from GitHub (if not done via UI):
	git push origin --delete feature-xyz


	The commits you made are still in main’s history; you just cleaned up the extra branch name.



Condensed cheat sheet

You can paste this into a note:

# Start new work
git checkout main
git pull
git checkout -b feature-xyz

# Do work
#   (edit files)
git status
git add <files>
git commit -m "Message"
git push -u origin feature-xyz   # first push; later just: git push

# (Later) bring in latest main before PR if needed
git checkout main
git pull
git checkout feature-xyz
git merge main
#   (resolve conflicts if needed)
git add <files>
git commit
git push

# On GitHub:
#   Open PR: base=main, compare=feature-xyz
#   Review, merge

# After PR merged
git checkout main
git pull

# Clean up (optional)
git branch -d feature-xyz
git push origin --delete feature-xyz   # or use GitHub's "Delete branch" button


If you’d like, tell me a specific next change you plan to make in sulfate (“add new residuals plot,” “revise text in section X”), and I can walk through this workflow using that exact feature name so it matches what you’ll actually type.

