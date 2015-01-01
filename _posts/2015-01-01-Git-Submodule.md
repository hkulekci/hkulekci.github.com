---
layout: post
title: Git Submodule
---

Working more than a year on git, i like that and i want to share some experience from here about submodule.

Submodule is great idea to attach exteernal repos to your project to specific path. In this project repo, there are some submodule in lib's subdirectiories. I am exampling to you how the submodules is adding to a repo. 


### Adding a Submodule to your repository

    git submodule add https://github.com/headjs/headjs.git lib/js/headjs
    git submodule add https://github.com/marcuswestin/store.js.git lib/js/storejs
    git submodule add https://github.com/cebe/smarty.git lib/template-engine/smarty
    git submodule add https://github.com/fabpot/Twig.git lib/template-engine/twig

We add submodules to our project now you can see your added library on your status
    
    git status

All your modules writing on ".gitmodules" file which is created by git. You can see with following code your gitmodules file content.

    cat .gitmodules

### Removing Submodule

If you want, you also can remove some submodules from your project. You can use to remove one or more of your submodule(s).

    git submodule rm lib/js/headjs

### Updating Submodule

There are some updates your submodule while you are working with your project. For example, Twig template engine release a new version. You can use to update your submodules following lines in to your submodule's path.

    git submodule update

That's all for now. To more information, you can look references part. 


References : 

 * http://git-scm.com/book/en/Git-Tools-Submodules
 * http://chrisjean.com/2009/04/20/git-submodules-adding-using-removing-and-updating/