# Git & Unity Merge Conflict Guide

1. Checkout and merge:
    ```bash
    git checkout feature-branch
    git merge main
    ```

2. Resolve code conflicts in your preferred IDE.

3. Push changes:
    ```bash
    git push -u feature-branch
    ```

>note: It may be helpful to `git log --graph --oneline --all` to visualise the branches structure before merging.

## UnityYAMLMerge Setup
Edit `.git/config` and add:

```gitconfig
[merge]
  tool = unityyamlmerge

[mergetool "unityyamlmerge"]
  trustExitCode = false
  cmd = '"%PROGRAMFILES%\\Unity\\Hub\\Editor\\<Unity_Version>\\Editor\\Data\\Tools\\UnityYAMLMerge.exe" merge -p "$BASE" "$REMOTE" "$LOCAL" "$MERGED"'
  ```

  Refer to [Unity Manual](https://docs.unity3d.com/Manual/SmartMerge.html) For more info.