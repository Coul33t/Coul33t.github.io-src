Title: Quick Github setup
Date: 2019-04-09 17:00
Modified: 2019-04-09 17:00
Tags: Git Github
Category: Github
Slug: Quick-Github-setup
Authors: Quentin Couland
Summary: A quick tutorial about using Github's core functions

This article is all about using Github's core functions without going too much into all of the details. The commands you will see here are the one you're going to use 80-90% of the time.

Github is a really, really, REALLY, __REALLY__ useful tool used for control versioning. It is mostly used for computer code, but it can be used for everything you want. It allows for:
- tracking changes into your codes (-> commits made)
- collaborative developement (-> Pull requests tab)
- Conflict handling (for example, if two persons modified the same file, when uploading, you can merge the different parts together to avoid erasing code)
- Bug tracking (-> Issues tab)

And much more. Indeed, not everyone will use every functionnality, and tutorials usually cover everything it can offers to you. But what if you only want a quick startup? For example, for most of my project, I only want to upload my code so I can access it everywhere, and track changes I made.

## What will you need for this tutorial
- A Git client (you can grab one [here](https://git-scm.com/))
- Optionnal: a better terminal than Window's default one, like [CMDer](https://cmder.net/)

## Let's go!
### The beginning
Uploading your code to Github (which is called _pushing_) requires three main steps: `add`, `commit`, `push`. If you want to create a new repository, an initial `init` step is required too.

Let say that you have an existing codebase on your computer, and you want to upload it on Github. Your codebase looks like this:

```
Python/
└── my_project/
    ├── main.py
    ├── tools.py
    └── misc.py
```

### Creating the repository on Github
First, we have to create a repository on Github. This repository will contains the files you want to upload. You can do so by: going on the Github frontpage, then clicking " New " on the left panel. You will be asked to:
- Choose a name for the project
- Write an optionnal small description
- Decide to make it public or private

Once you're done, click " Create ". Tada, your repository is now live! You'll be redirected to another page with a lot of commands. Don't panic! Since your repository is empty, Github is giving you ways to fill it.

### Creating the local repository and pushing (uploading) the local code to the remote repository
In our example, we have a pre-existing codebase that we want to upload. He have to initialise a Git repository locally. Using the git prompt:

1. Navigate to your code base top folder location using the `cd` command (e.g. `cd C:\Users\YourName\Documents\Programming\Python`) OR open a Git console directly in the desired folder (`right click -> Git Bash here`).
2. write `git init`. This will create a hidden `.git` folder, containing all of the required files.
3. OPTIONAL: if you want to select which files must (and must not) be uploaded, you have to use a `.gitignore` file. For more information about this, go to the Tips section below.
4. write `git add *`. This will add all the files and folders of the current folder (`*` is the _wildcard_ operator, which means _all_) to the next commit.
5. write `git commit -m "YOUR MESSAGE HERE"`. This will create the commit.
6. write `git remote add origin https://github.com/YourName/YourRepo.git`. Here, we're telling our local repository where to upload the code (in our case, `https://github.com/YourName/YourRepo.git`). Note that `origin` is an alias for the url, allowing us to only write `origin` every time we want to push the code (instead of the whole URL).
7. finally, write `git push -u origin master`. This will push the commit to origin on the `master` branch. For more informations about branches, go to the Tips section below.

If you refresh your repository webpage, the code should be there. Congratulations!

### Pushing the code to the created repository
The next time you want to push your code to your remote repository, only the steps 1, 4, 5 and 7 will be required:

1. `cd C:\Users\Kontin\Documents\Programming\Python` OR `right click -> Git Bash here`
2. `git add *`
3. `git commit -m "YOUR MESSAGE HERE"`
4. `git push origin master` OR `git push`

Note that in the 7th step, you can use `git push`, as the `-u origin master` tells Git that the default location for pushing your code is at `origin master`.

### TL;DR
First time:

1. `cd C:\Users\YourName\Documents\Programming\Python` OR `right click -> Git Bash here`.
2. `git init`
3. OPTIONAL: `.gitignore` file.
4. `git add *`
5. `git commit -m "YOUR MESSAGE HERE"`
6. `git remote add origin https://github.com/YourName/YourRepo.git`
7. `git push -u origin master`

After:

1. `cd C:\Users\YourName\Documents\Programming\Python` OR `right click -> Git Bash here`
2. `git add *`
3. `git commit -m "YOUR MESSAGE HERE"`
4. `git push origin master` OR `git push`