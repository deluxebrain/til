# Update commiter for previous pushed commit

1. Correct your repository author details

    ```sh
    git config user.name "[NAME]"
    git config user.email "[EMAIL]"
    ```

2. Rebase to the commit preceeding the commit that needs fixing

    So, if the commit history looks like A-->B-->C and the commit that needs fixing is *B* then you'll need to rebase to *A*.

    ```sh
    git rebase -i [SHA]
    ```

3. Change the lines for the commit that needs fixing

    The rebase will bring up the configued editor.
    For each commit that needs fixing, update the line from *pick* to *edit*.

4. Ammend the commit

    ```sh
    git commit --amend --author="NAME <EMAIL>" --no-edit

5. Complete the rebase

    ```sh
    git rebase --continue
    ```

6. Push up the changes

    ```sh
    git push origin master --force
    ```
