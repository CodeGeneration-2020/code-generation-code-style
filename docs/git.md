# Git 🖥

[Return to Table of Contents](../README.md)

## Branch best practices

> Here is a best practices for naming and committing changes to your branch

### Use different branch types ✅

> Use basic types in your branch name such as : `feature`, `environment`, `bugfix`, `refactoring`  
> That gives us ability to have better understanding of brunch purposes

```git
    git checkout -b feature/<task-id>/add-user-auth 
```

### Use separators in your branch name ✅

> Use `/` separator in your branch name
> If helps us to group our branches in directories when we use  
> [Source tree](https://www.sourcetreeapp.com/) or any other tool for git visualization

```git
    git checkout -b feature/user/list
```

```text
-feature
--user
---list
```

## Commits

### Use commit types✅

> For commits use : `feat:`, `fix:`, `ref:`, `env`  
> Commit types helps us to track progress of main branch  
> also it autogenerate title of your `PR`

```git
    git commit -m "feat: Created user login form"
```

### Split your task into few commits ✅

> You should split your task into a logic commits.  
> It saves a lot of time for `PR` reviewers.

```git
    git add ./components/user-login
    git commit -m "feat: User Login Component"
    git add ./store/user
    git commit -m "feat: User actions, reducer, selectors "
```

### Attach task links to your commits ✅

> Attach task links (Trello, Jira, etc.) to your commits

```git
    git commit -m "
    feat: User Login Component

    Task Description 

    Task: http://faketrello/fake-task
    "
```

### Add additional commit description for complex features/bugs ✅

> If you feace with some braking changes for code base  
> you should add additional description for those changes with the reasons (especially for refactoring)

```git
    git commit -m "
    ref: User Reducer Rework

    Old implementation of user reducer made our code looks like spaghetti : )`

    Task: http://faketrello/fake-task
    "
```

### Don't create commits for minor changes ✅

> If you are received minor comments in your `PR`.  
> Don't create any additional commits for that.  
> Much better to apply changes to existent commit.

```git
    git add ./some-files-with-minor-changes/
    git commit --amend
```

### Don't create additional with merge ✅

> If you work on some huge feature you should keep your up to date.  
> Instead of merge your master brunch directly you should merge it with `--no-commit` flag.  
> Looks much better when your brunch doesn't have any trash commits, isn't it ? : )

```git
    git fetch origin master
    git merge master --no-commit
```

## PRs 

### Pr titles and descriptions ✅

> Always attach description and proper title to your `PR`.  
> Also try to attach images for UI tasks.  
> For backend task always attach API documentation links.  
> Always attach task link to the bottom of your description.  

UI:  

```text

   Description of your UI task

   Images:

    <images with your work>

   Task: http://faketrello/fake-task
```

Backend:  

```text

   Description of your UI task

   API: http://some-fake-api-link-with-your-docs/fake-user-api

   Task: http://faketrello/fake-task
```
