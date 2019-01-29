# Rebasing

Rebasing is the process of moving or combining a sequence of commits to a new base commit.

Put another way - rebasing is changing the base of your branch from one commit to another, making it appear as if you created your branch from a different commit.

## Common Scenarios

Here are some common uses for rebasing. For learning, you can fork this repo then create a feature branch to simulate these scenarios. I'll assume a feature branch called `rebase-me`.

### Scenario 1 - Pull with Rebase
First, let's create a file to do some work in. You can create it in github. Call the file `scenario1`. 

Now, let's pull the file down locally.

```
git checkout rebase-me && git pull
```

To simulate the brach progressing remotely, let's add your first name to the file in github by clicking the edit icon and typing yrou first name. Commit the changes in github with the message 'add first name'. 

Now, in branch you've pulled down, add your last name to the file. Commit your work locally.

```
git commit -am 'add last name'
```
Now your branch is in conflict - it has been edited remotely and locally. But instead of doing a pull with a merge (and having a merge commit), we can pull with rebase and preserve a clean history.

```
git pull --rebase
git mergetool
```

`mergetool` will allow you to use your favorite editor to address your conflict. In this case let's use our local changes so make sure to resolve with the correct name. Once your merge is complete you can `commit` your merge and look at your `log`.

```
git commit 
git log -2
```

Notice the 2 commits in history - no merge commit.

Bonus - you can change this behavior to be the default for pulls.

```
git config --global pull.rebase true
```

### Scenario 2 - Rebase against origin 

For this second scenario, let's create a new file locally and type in your favorite celebrity's name. Commit but don't push your changes.

```
echo 'Celebrity' > scenario2
git add --all && git commit -m 'add celebrity'
```

Now that your changes are staged, you decide that the celebrity you meant to choose was Alex Trebek, a national treasure for the US and Canada. Edit the file to use his name and then commit the changes with the message fixup but don't push your changes.

``` 
git commit -am 'fixup'
```

Start the interactive rebase process against your origin branch with `git rebase -i origin/rebase-me`. In the window that comes up you should see your 2 commits with the word pick to the left and the sha1 in the middle and your commit message on the right. Change the second commit to fixup to combine the first commit with the second and use the first commit's message.

```
pick <sha1> add celebrity
fixup <sha1> fixup
```

Save the file and watch the rebase start. Once completed, `git log` and notice the commit history has changed - add celebrity has a new sha1 and the fixup commit is not present. 

Lastly, push your changes in preperation for the last excercise: `git push`.

### Scenario 3 - Rebase agaist master

Now you've decided to squash all the commits on your branch together before merging to master. Let's start the interactive rebase.

```
git rebase -i master
```

Your window should have all the commits from your branch. You want to pick the first commit and squash the remaining ones. 

```
pick <sha1> add first name
squash <sha1> add last name
squash <sha1> add celebrity
```

This time when the interactive merge runs you will need to supply a commit message for the 2nd and 3rd operation. Let's go with 'add name and celebrity'.

Now, take a moment to look at your history on github. You should see all 3 commits on your branch. Let's push out our changes, but we need to re-write this branch's history. To do this, force push which will overwrite the history of the branch at the origin to take your local history. 

```
git push -f
```

Now observe the branch in github. Your 3 commits are now the 1 new commit. 

## More Information

Check out these resources to learn more.

[git rebase | Atlassian](https://www.atlassian.com/git/tutorials/rewriting-history/git-rebase)

The best resource for learnig git I've ever found - [Learn Git Branching](https://learngitbranching.js.org/)
