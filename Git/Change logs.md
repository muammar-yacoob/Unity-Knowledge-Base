``` bash
git log --pretty=format:'- %s' $(git merge-base main HEAD)..HEAD >> CHANGELOG.md
```

