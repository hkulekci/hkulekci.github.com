---
layout: post
title: Usage of BasicMVC and Slim Framework
---

[BasicMVC](https://github.com/hkulekci/basicmvc) is a library to create MVC structure with [Slim Framework](http://slimframework.com/). It use Slim framework router to reach to controller and model and views are called from the controller. 

In this text, i am trying to help bootstraping a project with BasicMVC and Slim Framework. Firstly, let's talk about the file structure. You can use what you want in your structure. Because, in the BasicMCV, all the directories configurable. For example,

    app/
        public/
            controllers/
                ...
            models/
                ...
            views/
                ...
    .htaccess
    index.php
    
in this structure, application directory is `app/` directory and represented as `APP_DIR`. And your controllers is at `app/public/controllers/` and your models is at `app/public/models/` etc. When you create your BasicMVC object in index, you can set them like that: 

    $basicmvc = new BasicMVC($app, array(
        "controllers_path"   => APP_DIR . "public/controllers/",
        "models_path"        => APP_DIR . "public/models/",
        "library_path"       => APP_DIR . "public/system/library/",
        "template_constants" => array()
    ));

`template_constants` is a variable to send data directly template. I am using it to send some constant of my project. For example, I define a `BASE_URL` which is base url of my project. I send to template file and use.

In fact, you must set them. They are required, too. I am using [Twig Template Engine](http://twig.sensiolabs.org/) to render views. In this example, view files is at `app/public/views/` folder. Firstly, i am creating twig object 

    $twigView = new \Slim\Views\Twig();

    $app = new \Slim\Slim(
        array(
            'mode'              => 'development', // development, test, and production
            'debug'             => true,
            'view'              => $twigView,
            'templates.path'    => APP_DIR . "public/views/"
            )
        );
        
You can use [Smarty Template Engine](http://www.smarty.net/) as a template engine, use following lines to add your project:

    $view = new \Slim\Views\Smarty();
    $view->parserDirectory = APP_DIR . "public/views/";
    $view->parserCompileDirectory = APP_DIR . 'public/cache/';
    $view->parserCacheDirectory = APP_DIR . 'public/cache/';

Thats all. You can ready to run your project. One more thing, I will explain your controller, model and view file contents following days. 
