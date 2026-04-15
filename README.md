# Test: Symbolic Links in Git

Symbolic links are a good way for explicitly using shared resources. Unfortunately operating systems and / or versions of them differ in support and implementation of symbolic links. Cloning and pushing with Git across different machines means possible clashes between different symbolic link support, and Git tries to mitigate this under the hood what makes it even more difficult to predict what will happen where.

Instead of trying to find settings that will do the magic now and then and everywhere, you can clone this repo from its GitHub presence to a new local repo, check if the contained symbolic links work locally, push it to a new remote repo on GitHub or elsewhere and clone again from there to new local repo to check again that the symbolic links still work.


## Detailed steps

> * Bash shell and GitHub as remote with `gh` (the [GitHub CLI ](https://cli.github.com/)) installed

  * Go to a temp dir
  * Clone this repo
    ```
      git clone https://github.com/hh-lohmann/test-git-symbolic-links
    ```
  * Go to `first-dir` dir in the new repo dir `test-git-symbolic-links`
    ```
      cd test-git-symbolic-links/first-dir
    ```
  * Check if the file `second.md` is a link
    * Quit if result is not `OK`
    ```
      test -h second.md && echo 'OK (is a link)' || echo 'Error (not a link)'
    ```
  * Create a temporary remote and save its URL, e.g.
    ```
      my_temp_repo_name="test-git-symbolic-links-temp-to-be-deleted"  
      my_temp_remote=$(gh repo create $my_temp_repo_name --private)
    ```
  * Set temporary remote as Git remote "test"
    ```
      git remote add test $my_temp_remote
    ```
  * Push to temporary remote
    ```
      git push -u test
    ```
  * Delete current test clone
    * use `rm -rf` only if `gio trash` is not available
    ```
      cd ../../ && gio trash test-git-symbolic-links
    ```
  * Clone temporary remote
    ```
      git clone $my_temp_remote
    ```
  * Go to `first-dir` dir in the new repo dir `$my_temp_repo_name`
    ```
      cd $my_temp_repo_name/first-dir
    ```
  * Check if the file `second.md` is a link
    ```
      test -h second.md && echo 'OK (is a link)' || echo 'Error (not a link)'
    ```
  * Delete current test clone
    * use `rm -rf` only if `gio trash` is not available
    ```
      cd ../../ && gio trash $my_temp_repo_name
    ```
  * Delete the temporary remote => hand over to Browser for security dialogs, e.g.
      * **If GitHub credentials stored in the browser you habe to log in**
    ```
      google-chrome $my_temp_remote/settings/#danger-zone
    ```
