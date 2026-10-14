# Git
- merge multiple commits into one commit 
```
git reset --soft HEAD~3
```
where (3) is the # of previous commits you want to combine 
```
git add .
git commit -m 
git push -f 
```

- square all commits in the current branch into one 
	- can do local `main` or `origin/main`
```
git reset --soft main
git add .
git commit -m 
git push -f 
```

# Gradle

Update external library / dependencies 
```
./gradlew gL sL
./gradlew build --refresh-dependencies 
```


# Vim 
reverse lines from 1 to 25 starting from line 0: 
`:1,25g/^/m0` 
replace all “pick” with “f” in the entire document
`:%s/^pick/f/g`


# Linux

Find pid for process at port 3000:  `sudo lsof -i :3000`
Kill process:  `kill -9 ${PID}``
