---
title: "Git worktrees aren't the problem"
source: "https://www.youtube.com/watch?v=A2f3T1JELbo"
cover: "https://www.youtube.com/img/desktop/yt_1200.png"
author:
  - "The Modern Coder"
description: "Worktrees are conceptually simple, but a handful of small details get overlooked — and they can make a surprisingly big difference between worktrees feeling fiddly, or frictionless. This video covers"
created: 2026-04-28
---
```vid
https://www.youtube.com/watch?v=A2f3T1JELbo
Title: Git worktrees aren't the problem
Author: The Modern Coder
Thumbnail: https://i.ytimg.com/vi/A2f3T1JELbo/mqdefault.jpg
AuthorUrl: https://www.youtube.com/@themoderncoder
```

![](https://www.youtube.com/watch?v=A2f3T1JELbo)

Worktrees are conceptually simple, but a handful of small details get overlooked — and they can make a surprisingly big difference between worktrees feeling fiddly, or frictionless. This video covers three that matter most: creating them properly, where to put them on disk, and handling ignored files.  
  
🔗 New to worktrees? Start here: https://youtu.be/jmAJQEooFhg  
  
🧠 Build a full mental model of Git → https://learngit.io  
  
I built LearnGit.io to be a more complete curriculum focusing on how Git actually works, rather than memorizing commands. No pressure, just wanted to get the word out. It's also free for students.  
  
⏱️ Chapters  
0:00 Intro - worktree basics  
0:44 Creating worktrees properly  
2:03 Worktree creation shorthand  
2:23 Where to put worktrees on disk  
4:30 Handling ignored files  
5:42 Recap  
  
Commands  
"fatal: 'main' is already used by worktree at..."  
git worktree add \[path\] \[branch\] -b \[new-branch\]  
  
#git #gitworktree #gitworktrees #worktrees #gittutorial #programming #softwaredevelopment #developer #webdev #devtools

## Transcript

### Intro - worktree basics

**0:00** · The beauty of worktrees is really that they're dead simple.

**0:03** · They're essentially just additional working directories that allow you to leave any messy in progress work right where it is, and then swap instantly to a clean working tree ready to do something else. No stashing or throwaway commits to slog through when you want to context switch. That's because work trees have their own folder on disk with a copy of the project files. You just do some work, commit and then swap to another worktree and do the same thing.

**0:27** · They all share the same repository, and conceptually that's all there is to it. But even if you're comfortable with the basics, it's worth revisiting a few key aspects of worktrees, because there are a handful of small optimizations that are easy to miss, but can make a surprisingly big difference. Let's start with creating work trees, because if we start off on the right foot, we can make our lives easier down the road. Whether you're in the terminal or your IDE, Git needs to know three things to create a work tree.

### Creating worktrees properly

**0:57** · First, the worktree's name, which it also uses as the directory name, and we'll come back to that later.

**1:04** · Secondly, it needs to know where in git history you want your work tree to start. "git worktree add hotfix main" that's pretty simple.

**1:12** · I want a worktree called hotfix that starts the latest commit from main. But this doesn't actually work.

**1:20** · The reason is that git needs a third critical piece of information, and that's what branch to check out inside that worktree. We didn't explicitly specify this, so git tried to use 'main'. Since all the work trees share the same repository, Git is going to enforce a rule one branch for one work tree. So if main is already checked out in your default worktree, which is typical, no other worktree can check it out, so make sure you're familiar with the "-b" flag. Instead of checking out 'main', you can create a new branch off of it.

**1:48** · So I'll use 'hotfix' as my branch name, since that'll make it easier to tell which branch is associated with which work tree at a glance. Taking a second to consider your branch pointers when creating worktrees is going to go a long way, but the good news is if that command feels too long, there's actually shorthand, which I'm assuming most people are going to use. "git worktree add hotfix" this creates a worktree and anchored where your currently selected branch is pointing, and then It'll also create a branch with the same name as that worktree. So now we've got our worktree.

### Worktree creation shorthand

### Where to put worktrees on disk

**2:23** · But remember that the name argument is actually a file path, which means that wherever you run, this command determines where that work tree folder is actually going to live on disk. So I'm in the root of my project directory now, and that means that's where my worktree folder was created.

**2:40** · See, it's right here. There's nothing wrong with this. By default, most people start off creating worktrees inside their project directory, and this works totally fine. But you run into two snags pretty quickly.

**2:51** · First, your IDE's file tree is going to start collecting these work tree folders, and that can lead to a lot more visual clutter.

**2:58** · The more work trees you use starts to feel like the antithesis of clean isolation that worktrees promised in the first place.

**3:06** · And secondly, git sees that worktree folder as untracked files, and that really makes sense. If new directories or files are created inside your git project, you want Git to track them. And Git will try to.

**3:17** · But see, now you're adding worktree folders to your .gitignore every time you create a new worktree. Neither of these is necessarily a big deal, but it can just add up over time. And there's a really simple fix, which is just to create worktrees as siblings of your project directory instead of children.

**3:33** · Kind of like this. One small change to the path "../" now puts this work tree next to my project folder on disk rather than inside it.

**3:41** · Clean separation. There's no IDE clutter, and then you don't have to do any git ignore management, and this works, and it'll serve you well.

**3:48** · The thing to keep in mind about this whole discussion is that there's no accepted best practice for organizing worktrees on disk. And this is genuinely a debate.

**3:56** · Some people create a dedicated worktrees folder and then git ignore it, and and Claude Code does something similar. It puts your work trees in the ".claude/worktrees/" directory inside your project folder. But then there is the bare repo setup that does away with all the initial checkout of the project files in favor of organizing your project as a collection of worktrees from the get go. And that's a really interesting topic on its own. But in any case, the most important thing about work tree organization is just to be intentional about it. Pick a convention and just stay consistent. But don't optimize too soon.

**4:27** · So now you've created your worktree you've put it in the right place and you open it up and some of your files are missing. This happens almost every time you create a worktree. And if this is your first time noticing this, it can feel like you did something wrong. But you didn't do anything wrong.

### Handling ignored files

**4:46** · Work trees only get a copy of your tracked file, so anything you're .gitignore doesn't come along. This is important, and it means that two categories of files are deliberately missing. First, that's going to be any sort of build artifacts, something like a node, modules, folder, compiled output, build directories, and things like that. These need to be rebuilt inside each new worktree. But rebuilding these things is the right behavior.

**5:09** · Any code changes you make are going to change compiled output anyway.

**5:13** · You want a clean slate, not stale dependencies from another workspace. And if you're more adventurous and you want to try to work around this problem, you can look into things like symlinking build directories. Anyway, the second category of missing files is going to be secrets. That's .env files, API keys, and then local configuration. Stuff that you've deliberately kept out of version control. These need to be copied manually. It's a small setup cost, but it's one time per worktree. Don't be tempted to commit secrets to your repository to get around this. Worktrees are simple, but they're not without trade offs.

### Recap

**5:43** · You're trading setup friction for fast, clean context switching. If that trade off doesn't work for you, that's exactly what stashing was designed for. Understanding when worktrees are going to be the right tool and when they're not quite the right fit. That's what separates somebody who knows a command or two from somebody who has a really deep understanding of git. A deep understanding you can build over at LearnGit.io If you like this video, you'll love what I made over there.




