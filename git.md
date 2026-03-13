> ## Git SSH Key
>> * -> Git GUI -> Help Show SSH key -> Generate Key -> Copy to clipboard
>> * -> GitLab -> User -> Preferences -> SSH Keys -> Add new key
>>   * Usage type: Authentication & Signing
>>   * Expiration date: null
> ## Pull changes with temp stash
>> `git stash save "TEMP_$(Get-Date -Format "yyyy-MM-dd_HH:mm")"; git pull; git stash apply`
> ## Checkout / crate branch
>> `git checkout -b us/0000000000`
> ## Crate commit
>> `git commit -m "Commit Name"`
> ## Push to new branch
>> `git push --set-upstream origin (git branch --show-current);`
> ## (GitHub) Go to create new PR (Pull Request) page
>> `start "$((git remote get-url origin).Replace('.git', ''))/pull/new/$(git branch --show-current)"`
> ## (GitHub) Go to PR (Pull Request) page
>> `start "$((git remote get-url origin).Replace('.git', ''))/pulls"`
> ## Branches naming
>> * `bug/AB#[ID] [Title]`
>> * `merge-release-[V.er.si.on]-to-develop`
>> * `spike/AB#[ID] [Title]`
>> * `story/AB#[ID] [Title]`
>> * `task/AB#[ID] [Title]`
>> * `technical/AB#[ID] [Title]`
> ## Add all new files and tracked changes
>> * `git add .`
> ## Change branch
>> * `git checkout [branchName]`
> ## Change commit date
>> ### CommitDate / AuthorDate / GIT_COMMITTER_DATE
>>> * `git commit --amend --date "Mon Mar 1 01:00:00 2020 +0000" --no-edit`
>> ### Check commit history:
>>> * `git log --pretty=fuller`
> ## Change git editor
>> * `git config --global core.editor {editorName/editorPath}`
>> * `git config --global core.editor     "'C:\Program Files\Microsoft VS Code\Code.exe' -n -w"`
>> * `git config --global sequence.editor "'C:\Program Files\Microsoft VS Code\Code.exe' -n -w"`
>> * `-c "core.editor=code --wait --reuse-window" -c "sequence.editor=code --wait --reuse-window"`
> ## Check commits history
>> * `git log`
> ## Check git version
>> * `git --version`
> ## Remove untracked files
>> * `git clean -xdf --dry-run`
>> * `-x` -> Remove files that are ignored by .gitignore.
>> * `-d` -> Remove untracked directories.
>> * `-f` -> Force removal.
>> * `--dry-run` -> Lists files to delete.
> ## Clear not existing remote references
>> * `git fetch --prune` \ `git fetch -p`
> ## Clone repository
>> * `git clone [httpsAdders / sshAdders]`
> ## Config List
>> * `git config -l`
>> * `git config --global -l`
> ## Config User Name & eMail
>> * `git config user.name  "Casper J. Sledz"`
>> * `git config user.email "casper.j.sledz@gmail.com"`
>> * `config --global` for global config
> ## Connect Origin Repository
>> * `git remote add origin [urlOfNewRepository]`
> ## Connect Origin Branch
>> * `git branch --set-upstream-to=origin/[branchName]`
> ## Create local branch and switch to it
>> * `git checkout -b [newLocalBranchName]`
> ## Create new commit
>> * `git commit -m [commitName]`
> ## Commands list
>> * `git --help`
> ## Check if repository exist (current folder)
>> * `git status`
> ## Create new local repository
>> * `git init`
> ## Delete branch
>> * `git branch --delete [brachName]`
>> * `-d` => `--delete`
>> * `-D` => `--delete --force`
> ## Delete origin branch
>> * `git push origin --delete origin [originBrachName]`
> ## Disable SSL verify (globally) ERR: SSL certificate problem: self signed certificate
>> * `git config --global http.sslVerify [false / true]`
> ## Discard everything permanently
>> * `git reset --hard`
> ## Discard file to HEAD
>> * `git reset HEAD [fileName]`
> ## Discard last commit - return to edition
>> * `git reset --soft HEAD~1`
> ## Discard last tracked (added / staged) changes
>> * `git reset HEAD`
> ## Disconnect Origin
>> * `git remote rm origin`
> ## Files size (git bash)
>> * `git ls-tree -r --long HEAD | sort -k 4 -n -r`
> ## Get origin url
>> * `git remote get-url origin`
> ## Ignore file / untrack
>> * `git update-index --assume-unchanged [FileName]`
> ## Ignore file / untrack (revert)
>> * `git update-index --no-assume-unchanged [FileName]`
> ## LFS (Large File Storage)
>> 1. Download and install [LFS](https://git-lfs.com/)
>> 2. `git lfs install`
>> 3. `git lfs track "example\path\to\e.g.\large.dll"`
> ## Moving repository
>> 1.
>>> 1. `git fetch -p`
>>> 2. `git fetch --tags`
>>> 3. `git remote rm origin`
>>> 4. `git remote add origin [urlOfNewRepository]`
>>> 5. `git push --set-upstream origin [originBranchName] --force` -> (for each branch to migrate )
>>> 6. `git push --tags`
>> 2.
>>> 1. -> Create temp directory
>>> 2. -> Open terminal in created director
>>> 3. -> Clone repository to move
>>> 	`git clone {urlOfOldRepository}`
>>> 4. -> May require go catalogue up to cloned repo
>>> 5. -> Check out to all beaches which you would like to move
>>> 	`git checkout {branch-name}`
>>> 6. -> Fetch tags
>>> 	`git fetch --tags`
>>> 7. -> you can verify beaches and tags
>>> 	`git tag`
>>> 	`git branch -a`
>>> 8. -> Disconnect old repo
>>> 	`git remote rm origin`
>>> 9. -> Connect with new repo
>>> 	`git remote add origin {urlOfNewRepository}`
>>> 10. -> Push main branch
>>> 11. -> Push rest of branches
>>> 	`git push origin --all`
> ## Open online method documentation
>> * `git help [commandName]`
> ## Override local changes
>> * `git reset --hard origin/development`
> ## Push to new origin branch
>> * `git push --set-upstream origin [newOnlineBranchName]`
> ## Push to repository (existing one)
>> * `git push -u origin [branchName]`
> ## Rebase commits
>> * `git rebase -i HEAD~[numOfLastCommits]`
> ## Remove files from the working tree
>> * `git rm -r [config/example-configuration]`
c-r` -> Allow recursive removal when a leading directory name is given
> ## Remove files (Large)
>> 1. -> [Install JDK](https://adoptium.net/)
>> 2. -> [Download bfg.jar](https://rtyley.github.io/bfg-repo-cleaner/)
>> 3. -> Remove file bigger than:
>>    1. `java -jar "$env:UserProfile\Downloads\bfg-1.14.0.jar" --strip-blobs-bigger-than 100M`
>>    1. `git reflog expire --expire=now --all`
>>    1. `git gc --prune=now --aggressive`
>> 4.  Change brach and repeat (CURRENT BRACH CANNOT BE EDITED)
>> 3. -> Remove specifics file:
>>    1. `java -jar bfg-1.14.0.jar --delete-files oraociei12.dll`
>>    1. `git reflog expire --expire=now --all`
>>    1. `git gc --prune=now --aggressive`
> ## Remove git repository
>> * `Remove-Item -Force -Recurse –path ./.git`
> ## Rename commit
>> * `git commit --amend -m [newCommitName]`
> ## Rename merge
>> * `git commit --amend` -> Renames merge if it's last commit
> ## Reverting single file
>> * `git checkout [commitID] --[filePath]`
>> * `git checkout 933cd760 --"\App\src\API\Example.cs"`
> ## Save changes as temp (stash)
>> * `git stash`
> ## Submodule Add
>> `git submodule add --branch main [repositoryURL]`
> ## Submodule init sub repositories
>> `git submodule update --init --recursive`
> ## Submodule pull all changes
>> `git submodule foreach git pull`
> ## Unstage and remove from index
>> * `git rm --cached [example.cs]`
>> * `rm --cached`
>>   * Use this option to unstage and remove paths only from the index.
>>   * Working tree files, whether modified or not, will be left alone.
> ## GitHub Actions
>> ### YML Environment Values References
>>> #### Environment secrets
>>>> `secrets.{{ name }}`
>>> #### Environment variables
>>>> `vars.{{ name }}`
>>> #### Print values
>>>> ```yaml
>>>> - name: "Echo"`
>>>>   run: echo "{{ value }}"`
> ## SSH Config
>> ```powershell
>> ssh-keygen -t ed25519 -C "[userEmail]"
>> ssh-keygen -t ECDSA   -C "[userEmail]"
>>
>> ssh -i [userDirectory]/.ssh/id_ecdsa [userEmail]
>> ssh [userEmail] mkdir -p .ssh
>>
>> # ssh-rsa
>> #	-go to-> GitLab
>> #	-go to-> User Settings
>> #	-go to-> SSH Keys
>> # 	-copy key-> [userDirectory]/.ssh/id_rsa.pub
>> #	-> pase and add key
>>
>> git config --global credential.helper WinCred
> ## GitHub Branching Rules
>> ```json
>> {
>>   "id": 1,
>>   "name": "Secure release branches against deletions and updates",
>>   "target": "branch",
>>   "source_type": "Repository",
>>   "source": "github",
>>   "enforcement": "active",
>>   "conditions": {
>>     "ref_name": {
>>       "exclude": [],
>>       "include": [
>>         "refs/heads/Release_Branches/1.0.0.0"
>>       ]
>>     }
>>   },
>>   "rules": [
>>     {
>>       "type": "deletion"
>>     },
>>     {
>>       "type": "non_fast_forward"
>>     },
>>     {
>>       "type": "update"
>>     }
>>   ],
>>   "bypass_actors": []
>> }
>> /////////////////////////////////////////////////////////////////////////////////////////////////
>> {
>>   "id": 2,
>>   "name": "Secure core branches against edition expect TeamId=101",
>>   "target": "branch",
>>   "source_type": "Repository",
>>   "source": "github",
>>   "enforcement": "active",
>>   "conditions": {
>>     "ref_name": {
>>       "exclude": [],
>>       "include": [
>>         "~ALL"
>>       ]
>>     }
>>   },
>>   "rules": [
>>     {
>>       "type": "update"
>>     },
>>     {
>>       "type": "creation"
>>     }
>>   ],
>>   "bypass_actors": [
>>     {
>>       "actor_id": 101,
>>       "actor_type": "Team",
>>       "bypass_mode": "always"
>>     }
>>   ]
>> }
>> /////////////////////////////////////////////////////////////////////////////////////////////////
>> {
>>   "id": 3,
>>   "name": "Secure core branches against deletions",
>>   "target": "branch",
>>   "source_type": "Repository",
>>   "source": "github",
>>   "enforcement": "active",
>>   "conditions": {
>>     "ref_name": {
>>       "exclude": [],
>>       "include": [
>>         "~DEFAULT_BRANCH",
>>         "refs/heads/test"
>>       ]
>>     }
>>   },
>>   "rules": [
>>     {
>>       "type": "deletion"
>>     },
>>     {
>>       "type": "non_fast_forward"
>>     }
>>   ],
>>   "bypass_actors": []
>> }
>> /////////////////////////////////////////////////////////////////////////////////////////////////
>> {
>>   "id": 4,
>>   "name": "Pull request requirements - Test branch",
>>   "target": "branch",
>>   "source_type": "Repository",
>>   "source": "github",
>>   "enforcement": "active",
>>   "conditions": {
>>     "ref_name": {
>>       "exclude": [],
>>       "include": [
>>         "refs/heads/test"
>>       ]
>>     }
>>   },
>>   "rules": [
>>     {
>>       "type": "pull_request",
>>       "parameters": {
>>         "required_approving_review_count": 0,
>>         "dismiss_stale_reviews_on_push": false,
>>         "require_code_owner_review": false,
>>         "require_last_push_approval": false,
>>         "required_review_thread_resolution": false
>>       }
>>     },
>>     {
>>       "type": "required_deployments",
>>       "parameters": {
>>         "required_deployment_environments": [
>>           "development"
>>         ]
>>       }
>>     }
>>   ],
>>   "bypass_actors": []
>> }
>> /////////////////////////////////////////////////////////////////////////////////////////////////
>> {
>>   "id": 5,
>>   "name": "Pull request requirements - Development branch",
>>   "target": "branch",
>>   "source_type": "Repository",
>>   "source": "github",
>>   "enforcement": "active",
>>   "conditions": {
>>     "ref_name": {
>>       "exclude": [],
>>       "include": [
>>         "~DEFAULT_BRANCH"
>>       ]
>>     }
>>   },
>>   "rules": [
>>     {
>>       "type": "pull_request",
>>       "parameters": {
>>         "required_approving_review_count": 1,
>>         "dismiss_stale_reviews_on_push": true,
>>         "require_code_owner_review": false,
>>         "require_last_push_approval": true,
>>         "required_review_thread_resolution": true
>>       }
>>     },
>>     {
>>       "type": "required_linear_history"
>>     }
>>   ],
>>   "bypass_actors": []
>> }
>> /////////////////////////////////////////////////////////////////////////////////////////////////
>> // TODO
>> //  Working Branches Naming Rule
>> //    Working_Branches[/]TeamPrefix-[0-9]{5}_?.*
>> //      Working_Branches/TeamPrefix-18742
>>
>> // Release Branches Naming Rule
>> //    ^((?!(Release_Branches\/TeamPrefix_[0-9]{1,2}([.][0-9]{1,2}){3})|(Working_Branches\/TeamPrefix-[0-9]{5}_?.*))).*$
>> //      Release_Branches/TeamPrefix_2.4.1.0
>> //      Release_Branches/TeamPrefix_2.3.21.0
>> //      Release_Branches/TeamPrefix_2.3.20.0
>> //      Release_Branches/TeamPrefix_2.4.1.0
>> //      Release_Branches/TeamPrefix_2.3.16.0
>> //      Release_Branches/TeamPrefix_2.3.18.0
>>
>> //      Working_Branches/TeamPrefix-19903_sample_pipeline
>> //      Working_Branches/TeamPrefix-20300_only_ods
>> //      Working_Branches/TeamPrefix-20302_update_date
>> //      Working_Branches/TeamPrefix-00000
>>
>> //      Working_Branches/TeamPrefix_20300
>> //      Working_Branches/update_date
>>
>> //      Release_Branches/TeamPrefix_2.4.1
>>
>> //      Test_Branches/Test
