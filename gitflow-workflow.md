# Gitflow Workflow Steps:

1. git config --global user.name "Your Name"
2. git config --global user.email your@email.com
3. git pull develop-website-redesign (Update your local develop branch with remote server)
4. gcb feature/small-work-branch-name (Create new feature/work branch)
5. Make changes and ensure they work as expected
6. gd (git diff to self review changes)
7. gs (git status to see all changed files)
8. ga (git add any new files)
9. gcam "thoughtful comment". Always ensure commit is on the branch created in step 4
10. keep repeating step 5 to 9 on every logical unit of work. Go to step 11 when feature is ready
11. git push -u origin feature/small-work-branch-name
12. Create merge request and assign it for peer review
13. Peer can merge the request (and delete the branch) or give review comments
14. Make review comments related changes on same branch created in step 4 and repeat step 5 to 11
15. Pick next feature you want to work on and start from step 3
16. Enjoy!

# Git command shortcuts:

vi ~/.profile

alias c="clear"
alias gs="git status"
alias gl="git log"
alias glp="git log -p"
alias gc="git checkout"
alias gcm="git commit -m"
alias ga="git add"
alias gcb="git checkout -b"
alias glo="git log --oneline"
alias gb="git branch"
alias gbr="git branch -r"
alias gd="git diff"
alias gcam="git commit -am"
alias gc="git checkout"
