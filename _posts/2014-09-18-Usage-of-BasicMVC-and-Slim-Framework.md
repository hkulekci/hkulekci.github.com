---
layout: post
title: Usage of BasicMVC and Slim Framework
---

[BasicMVC](https://github.com/hkulekci/basicmvc) is a library to create MVC
structure with [Slim Framework](http://slimframework.com/). It uses Slim
framework router to reach the controllers.

In this text, I will try to help bootstraping a project with BasicMVC and Slim
Framework. First, let's talk about the file structure. You can use what you want
in your structure. Because, in the BasicMCV, all the directories configurable.
For example,

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

In this structure, application directory will be `app/` directory and
represented as `APP_DIR`. Your controllers will be at `app/public/controllers/`
and your models will be at `app/public/models/` etc. When you create your
BasicMVC object with index.php, you can set them like:

    $basicmvc = new BasicMVC($app, array(
        "controllers_path"   => APP_DIR . "public/controllers/",
        "models_path"        => APP_DIR . "public/models/",
        "library_path"       => APP_DIR . "public/system/library/",
        "template_constants" => array()
    ));

You must set your path of controllers, models and views.

`template_constants` is a variable to send data directly to view. I will use it
to send some constant of my project. For example, I define a `BASE_URL` which
is base url of my project. I will send it to view and use it there.

I will use [Twig Template Engine](http://twig.sensiolabs.org/) to render views.
View files are at `app/public/views/` folder. First, I will create twig object.
See example below.

    $twigView = new \Slim\Views\Twig();

    $app = new \Slim\Slim(
        array(
            'mode'              => 'development', // development, test, and production
            'debug'             => true,
            'view'              => $twigView,
            'templates.path'    => APP_DIR . "public/views/"
            )
        );

You can also use [Smarty Template Engine](http://www.smarty.net/) as template
engine. See example below.

    $view = new \Slim\Views\Smarty();
    $view->parserDirectory = APP_DIR . "public/views/";
    $view->parserCompileDirectory = APP_DIR . 'public/cache/';
    $view->parserCacheDirectory = APP_DIR . 'public/cache/';

That's all. You can ready to run your project. One more thing, I will explain
your controller, model and view file contents following days.
