---
tags:
  - git
  - sphinx
---

# Publish sphinx-generated docs on GitHub Pages

This guide provides instructions for seting up repository on github which will store any data on `master` branch and host documentation from separeate `gh_pages` branch. [Original manual](https://github.com/daler/sphinxdoc-test) was written by [Ryan Dale](https://github.com/daler).

## Set up main repository

- First set up main repository, any files can be stored there.  Following commands can be used:

```
mkdir sphinxdoc-test
cd sphinxdoc-test
git init
touch README
git add README
git commit -m 'first commit'
git remote add origin git@github.com:daler/sphinxdoc-test.git
git push origin master
```

## Set up sphinx within main repository

- Make a `docs` dir that will store documentation source from Sphinx:

```
mkdir sphinxdoc-test/docs
```

- Set up Sphinx from the `docs` dir:

```
cd docs
sphinx-quickstart
```

## Set up separate repository for hosting

- Set up a completely new directory that will serve as the build directory for Sphinx. Name used in guide - `sphinxdoc-test-docs`. Use following commands:

```
cd ..
mkdir sphinxdoc-test-docs
cd sphinxdoc-test-docs
```

- Clone the repo which was set up on github into a dir called `html`. Use following command to create it:
    
```
git clone git@github.com:daler/sphinxdoc-test.git html
cd html
```

The `html` dir now has a clone of the repository.  

- Create a new branch called `gh-pages`. This is a special branch name that github looks for in order to build static html pages:

```
git branch gh-pages
```

- Use following commands to switch to the `gh-pages` branch and clean it from any data:

```
git symbolic-ref HEAD refs/heads/gh-pages  # auto-switches branches to gh-pages
rm .git/index
git clean -fdx
```

 After this `gh-pages` branch is empty and ready to get html files from sphinx-build. After pushing this branch to github documentation site will be avaliable on github pages.

## Makefile changes

- Change `BUILDIR` and set it to folder, in which separate repository was configured:

```
BULDDIR = ../../sphinx-test-docs
```

- Add following step if you wnat to automate deployment to githubpages:
	
```
cd $(BUILDDIR)/html 
git add . 
git commit -m "rebuilt docs"
git push origin gh-pages
```

## Initial creation and commit workflow

- Commit all code in the main repository:

```
git add .
git commit -m "added all files and docs"
```

- Recreate the docs:

```
cd docs
make html
```

- Change to the `gh-pages` repository dir and commit the stuff that the `make html` command made:

```
cd ../sphinxdoc-test-docs/html
git add .
git commit -m "rebuild docs"
```

- Publish updated docs on github pages:

```
git push origin gh-pages
```

## Add a .nojekyll file

- Add an empty file called `.nojekyll` in the docs repository.  This file tells github's default parsing software to ignore the sphinx-generated pages that are in the gh-pages branch:

```
cd sphinxdoc-test-docs/html
touch .nojekyll
git add .nojekyll
git commit -m "added .nojekyll"
```

## Setting up cloned repositoriess on another machine

The only requirement is that the folder name that will hold the docs repository must have the same relative path name as is referred to in the Makefile.
If these repositories were set:
- `/data/repos/sphinxdoc-test`
- `/data/repos/sphinxdoc-test-docs` 

Paths could be set as `~/sphinxdoc-test-docs` and `~/sphinxdoc-test` respectively.

- Set up the main repo:

```
cd ~
git clone git@github.com:daler/sphinxdoc-test.git
```

- Set up the docs repository.  For this create a dir first, then change to it and clone the ``html`` part of the repository:

```
cd ~
mkdir sphinxdoc-test-docs
cd sphinxdoc-test-docs
git clone git@github.com:daler/sphinxdoc-test.git html
```

- Create a local tracking branch to the `gh-pages` branch:

```
git checkout -b gh-pages remotes/origin/gh-pages
```

Now the directories are set up the same way they were in the original setup described above.