# The backup manual for AFuture's Blog

> It's also a tip to backup your Hexo Blog.

## structure 
There are two branches in this repositories named "hexo" and "master".

1. The "hexo" branch is the backup which contains your blog's resources.

2. The "master" branch has main files for your Github Pages.

## The backup instructions

1. set a extra remote repositories.
    You can use other git service such as gitlab, I use Coding, a Chinese git service, to make sure my files are secure.

>git remote add coding git@git.coding.net:AndyFrancis/andyfrancis.coding.me.git

2. checkout

After you added an remote repo connected to this repo. You can chceckout remote version with command ` git remote --verbose `.
Then you may get lines like follows:

>coding	git@git.coding.net:AndyFrancis/andyfrancis.coding.me.git (fetch)
coding	git@git.coding.net:AndyFrancis/andyfrancis.coding.me.git (push)
origin	https://github.com/AFutureD/AFutureD.github.io.git (fetch)
origin	https://github.com/AFutureD/AFutureD.github.io.git (push)

3. push repo

>git push origin hexo
>git push coding hexo



## Recovery operation
1. npm install hexo-cli -g
2. cd {your folder}
3. npm install
4. npm install hexo-deployer-git


