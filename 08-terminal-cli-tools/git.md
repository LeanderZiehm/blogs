# git

## git

remove files from a commit that wasn't pushed yet:

```
git reset --soft HEAD~1
```

if the change was already pushed then you can use `git push -f` but its quite dangerous because it could delete progress of other contributors so be careful when using force push.

```
git reset --hard HEAD~1
git logs
```

basics:

```
git add .
git commit -m "write commit message here"
git push
```

setup:

```
git config --global user. email "you@example.com"
git config --global user.name "yourusername"
```

### Purge Files from history:

<pre><code><strong>git filter-repo --path-glob '*.csv' --invert-paths
</strong></code></pre>

### File History:

```
git log --follow --patch -- path/to/file
```

### Vscode Problems

sometimes vscode does a bunch of requests to the git server with the wrong credentials and then the firewall blocks you for some hours. So better to use git mostly over cli with ssh. TODO research more.

***

## Tips

when using ssh with passphrase `ssh-add` can save time. [ssh](https://github.com/LeanderZiehm/blogs/blob/main/terminal-cli-tools/terminal-cli-tools/ssh.md)

[ssh](../04-system-administration/ssh.md)

***

## .gitignore

#### priority order

Git checks multiple sources, and the last matching rule wins:

1. Command-line ignore rules (if used)
2. `.gitignore` can exist in many folders
3. `.git/info/exclude` (local repo-only ignores)
4. Global ignore file (`core.excludesFile`, often `~/.config/git/ignore`)

#### Pattern basics

* `#`: comments
* `!`: re-include a file that was previously ignored
* `\`: escape special characters
* `/`: directory separator

#### Matching rules

* `*` → matches any characters except `/`
* `?` → matches a single character
* `[]` → character ranges (e.g. `[a-z]`)
* Patterns can match:
  * files
  * directories (depending on trailing `/`)

#### Special cases

* `/pattern` → matches only from repository root (or current `.gitignore`)
* `pattern/` → matches directories only
* `**` → powerful recursive matching:
  * `**/file` → file anywhere
  * `dir/**` → everything inside a directory
  * `a/**/b` → matches multiple nested levels

#### Important behavior notes

* Git does **not track ignored files if they are already tracked**
*   To stop tracking a file:

    ```
    git rm --cached file
    ```
* Ignored directories are not fully scanned for performance reasons.
