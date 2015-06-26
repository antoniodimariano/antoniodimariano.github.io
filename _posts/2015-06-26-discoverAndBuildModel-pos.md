---
layout: post
title: Discover and Build Model from relational database with LoopBack Framework
description: "Just about everything you'll need to style in the theme: headings, paragraphs, blockquotes, tables, code blocks, and more."
modified: 2013-05-31
tags: [intro, beginner, jekyll, tutorial]
image:
  feature: texture-feature-05.jpg
  credit: Texture Lovers
  creditlink: http://texturelovers.com
---


## Introduction to LoopBack Model concept

First of all . What are Models in Loopback? They are just JavaScript object with both Node.js and REST APIs. Models are the core of LoopBack framework and represent backend data sources such as databases REST, SOAP and so on. 
One of the powerful feature of LoopBack is the functionality to automatically create REST API (with a full set of CRUD operations)  at the moment we define a model.  
In LoopBack each model comes with REST API unless we have a good reason to disable this feature. 

By default  LoopBack has a  **Basic model object**  which has properties and methods come and mixed from 


1. Model object, the base class for all models 

   ``var MyModel = loopback.Model.extend(‘MyModel’,properties,options) ``

2. Inclusion object (based on jugglerdb) ,  which enables you to load relations of several objects and optimize numbers of requests

3. Validateable (based on jugglerdb) object which provides a set of validations methods



##  built-in modules

Every LoopBack application comes with a set of **predefined built-in modules**,  _User_, _Role_, _Application_, in order to make life easier to developers, so we do not have to create these common models from scratch.
Of course in the real life our application needs more than 3 models. We can define our own custom model in various ways. We can extend built-in models and build our model on top of predefined functionalities or we can create a model from scratch.
LoopBack lets us create model with the model generator (inserire link) but what I like is the ability to build  models from an existing relational database using, what they call, model discovery.
Suppose we have data in a postgreSQL database. Loopback makes it surprisingly simple to create models from existing data and expose them as APIs. We have to 

1. code sets up the data source
2. call to discoverAndBuildModels() to create models from the database tables


When we attach a model to a persistent data source it becomes a **connect model** with CRUD operations. 


The Discovery Models process is supported only by **relational database connectors**, while for NoSQL database such as MongoDB, we have to use instance introspection process instead. 
I will take a look at models by instance introspection in the next posts.


## Code Snippets

{% highlight css %}

var path = require('path');
var app = require(path.resolve(__dirname, '../server'));

var repoDB = app.dataSources.repoDB;
repoDB.discoverAndBuildModels('verga', {schema: 'public'},
  function(err,models){
    if(err) throw err;
    console.log(typeof models);
    var nomeModello = 'Verga';
    var mymodel = app.model(models[nomeModello]);

    mymodel.find(function(err,records){
      if(err) throw err;
      console.log('Records:',records);
    })
    var models = app.models();

    models.forEach(function(Model) {
      console.log(Model.modelName); // Abbiamo la lista dei modelli e vedremo pure Verga
    });
        repoDB.disconnect();
  })

{% endhighlight %}

   
