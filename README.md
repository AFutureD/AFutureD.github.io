# The instruction manual for Francis‘s Blog

## structure 
There are two branches in this repositories which is named "hexo" and "master".

1. The "hexo" branch is the backup contained my blog's resources.

2. The "master" branch has main files for my Github Pages.

## The backup instructions

1. set git remote.

>git remote add coding git@git.coding.net:AndyFrancis/andyfrancis.coding.me.git

2. checkout

Then I could add an remote repo connected to this repo. When I chceckout remote version,you must get things like this:

>coding	git@git.coding.net:AndyFrancis/andyfrancis.coding.me.git (fetch)
coding	git@git.coding.net:AndyFrancis/andyfrancis.coding.me.git (push)
origin	https://github.com/DongFrancis/DongFrancis.github.io.git (fetch)
origin	https://github.com/DongFrancis/DongFrancis.github.io.git (push)

3. push repo

>git push origin hexo
>git push coding hexo



## Recovery operation
1. npm install hexo
2. npm install
3. npm install hexo-deployer-git


