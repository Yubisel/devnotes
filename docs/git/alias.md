# Alias

Git aliases are shortcuts or custom commands that you can create to make your Git workflow more efficient. While there are no universally "popular" Git aliases since they can vary depending on individual preferences and workflows, I can provide you with some commonly used Git aliases that are generally helpful:

1. **Basic Aliases**:
      - `co` for `checkout`
      - `ci` for `commit`
      - `br` for `branch`
      - `st` for `status`
      - `df` for `diff`
      - `lg` for `log --oneline --decorate --all --graph`
      - `pul` for `pull`
      - `ps` for `push`

2. **Custom Commands**:
      - `unstage` to unstage changes: `git config --global alias.unstage 'reset HEAD --'`
      - `last` to show the last commit: `git config --global alias.last 'log -1 HEAD'`
      - `amend` to amend the last commit: `git config --global alias.amend 'commit --amend'`
      - `undo` to undo the last commit: `git config --global alias.undo 'reset HEAD~1 --mixed'`
   
3. **Listing Aliases**:
      - `aliases` to list all aliases: `git config --get-regexp alias`

4. **Shortcuts for Branching**:
      - `b` to create and checkout a new branch: `git config --global alias.b 'checkout -b'`
      - `bd` to delete a branch: `git config --global alias.bd 'branch -d'`

5. **Clean Up**:
      - `prune` to remove remote branches that no longer exist on the remote: `git config --global alias.prune 'fetch --prune'`
      - `clean` to remove untracked files and directories: `git config --global alias.clean 'clean -df'`

6. **Commit History**:
      - `graph` to display a pretty Git history graph: `git config --global alias.graph 'log --oneline --graph --all --decorate'`

7. **Interactive Rebase**:
      - `ir` for interactive rebase: `git config --global alias.ir 'rebase -i'`

8. **Show Diffs**:
      - `wdiff` to see word-based diffs: `git config --global alias.wdiff 'diff --color-words'`

9.  **Show Branches with Last Commit Date**:
       - `branches` to list branches with their last commit date: `git config --global alias.branches 'for-each-ref --sort=-committerdate --format='"'"'%(committerdate:short) %04h %d %s'"'"' refs/heads/'`

To add these aliases globally, you can use the `git config --global alias.alias-name 'command'` syntax as shown above.

```bash
# Basic Aliases
git config --global alias.co 'checkout'
git config --global alias.ci 'commit'
git config --global alias.br 'branch'
git config --global alias.st 'status'
git config --global alias.df 'diff'
git config --global alias.lg 'log --oneline --decorate --all --graph'
git config --global alias.pul 'pull'
git config --global alias.ps 'push'

# Custom Commands
git config --global alias.unstage 'reset HEAD --'
git config --global alias.last 'log -1 HEAD'
git config --global alias.amend 'commit --amend'
git config --global alias.undo 'reset HEAD~1 --mixed'

# Listing Aliases
git config --get-regexp alias

# Shortcuts for Branching
git config --global alias.b 'checkout -b'
git config --global alias.bd 'branch -d'

# Clean Up
git config --global alias.prune 'fetch --prune'
git config --global alias.clean 'clean -df'

# Commit History
git config --global alias.graph 'log --oneline --graph --all --decorate'

# Interactive Rebase
git config --global alias.ir 'rebase -i'

# Show Diffs
git config --global alias.wdiff 'diff --color-words'

# Show Branches with Last Commit Date
git config --global alias.branches 'for-each-ref --sort=-committerdate --format="%(committerdate:short) %04h %d %s" refs/heads/'
```