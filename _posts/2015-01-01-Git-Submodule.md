---
layout: post
title: Git Submodule
---

    I did not use git submodules in production. This article for only information. Sometimes it can be really good option.

Working more than a year on git, i like that and i want to share some experience from here about submodule.

Submodule is great idea to attach external repos to your project to a path. Now, we can try to create a lib directory and we put some submodules in lib's subdirectiories. I am exampling to you how the submodules is adding to a repo. 


### Adding a Submodule to your repository

    git submodule add https://github.com/headjs/headjs.git lib/js/headjs
    git submodule add https://github.com/marcuswestin/store.js.git lib/js/storejs
    git submodule add https://github.com/cebe/smarty.git lib/template-engine/smarty
    git submodule add https://github.com/fabpot/Twig.git lib/template-engine/twig

We add submodules to our project now you can see your added library on your status
    
    git status

All your modules writing on ".gitmodules" file which is created by git. You can see with following code your gitmodules file content.

    cat .gitmodules

For me, `.gitmodules` file content is following

    [submodule "lib/js/storejs"]
        path = lib/js/storejs
        url = git://github.com/marcuswestin/store.js.git
    [submodule "lib/js/headjs"]
        path = lib/js/headjs
        url = git://github.com/headjs/headjs.git
    [submodule "lib/template-engine/twig"]
        path = lib/template-engine/twig
        url = https://github.com/fabpot/Twig.git
    [submodule "lib/template-engine/smarty"]
        path = lib/template-engine/smarty
        url = https://github.com/cebe/smarty.git

### Removing Submodule

If you want, you also can remove some submodules from your project. You can use to remove one or more of your submodule(s).

    git submodule rm lib/js/headjs

### Updating Submodule

There are some updates your submodule while you are working with your project. For example, Twig template engine release a new version. You can use to update your submodules following lines in to your submodule's path.

    git submodule update

### Cloning a Project with Submodules

To clone a project with its submodules, you can use --recursive parameter of git. You can check following commands. 

    $ git clone --recursive git@github.com:hkulekci/git-submodule-test.git

That's all for now. To more information, you can look references part. 


References : 

 * [Git Tools Submodule](http://git-scm.com/book/en/Git-Tools-Submodules)
 * [Git Submodules: Adding, Using, Removing, Updating](http://chrisjean.com/2009/04/20/git-submodules-adding-using-removing-and-updating/)
