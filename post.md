Synopsis
> So you keep seeing Meteor mentioned around the web and you are wondering if it is worth your time to dive in and learn? Maybe you gave Meteor a look and did like what you saw? Take a look at how easy it is to switch out parts to the platform that you don't like.

## The power of Isomorphic code

"Isomorphic JavaScript" is a term coined by [Charlie Robbins at Nodejitsu](http://blog.nodejitsu.com/scaling-isomorphic-javascript-code/) and popularized by [Spike Brehm of Airbnb](http://nerds.airbnb.com/isomorphic-javascript-future-web-apps/). Isomorphic code is the idea that you have code that can be run on both the server and the client. This is a more recent idea because in web development, you can't run things like Ruby or Python directly in the browser. With the advent of Node.js, you can run javascript on the server instead.

Think about this situation, you are [writing a rails app, but you need to add Angular](http://fdietz.github.io/recipes-with-angular-js/backend-integration-with-ruby-on-rails/consuming-rest-apis.html) on to write a more modern reactive interface. You start by writing a REST API endpoint using a model and some type of controller, this requires you to query the data using rails. Now you can go setup the $resource and do something like `Contact.show({ id: $routeParams.id })` to grab the data you need from the api. We've now gone from thinking about Rails ActiveRecord and ActionController syntax to working with Angular and javascript to get 1 little piece of a more modern web app hooked up.

What if instead you could just write `Contacts.find({_id: 'a1b2c3'})` in both the client and server? You've cut down drastically on syntax/brain context switching. On top of that, you can start writing things like [collection-helpers to cut down on code duplication](http://joshowens.me/fat-models-skinny-templates/) in both the client and server.

## Using node.js for the backend

Javascript has become the only language that is truly universal, being supported on the server, in browsers, and on mobile devices. The rise of javascript as a language to build compelling, complex, and dynamic client applications means we need something on the backend to allow us to code in javascript. 

A few interesting things happen when you choose to use Node.js as the backend server. The first thing is that you get access to all the NPM packages. NPM packages open up a whole world of possibilities for quickly getting code up and running for any backend services you want to build. The second thing is that Node is asynchronous in nature and has a notorious reputation for leaving you with [callback spaghetti](http://callbackhell.com/). There are patterns to help deal with this and new features like Promises are coming out with ES2015 to make it better. In the meantime, Meteor actually uses Fibers to wrap up everything in the server and make it synchronous.

## DDP as a fulcrum

While I think node.js is a great starting point for building apps, there are times in the application lifecycle that you may stumble upon a design pattern or framework that meets part of your application needs a little better. Luckily Meteor comes with a built-in separation point called DDP. DDP is a protocol built on top of JSON and Websockets to deliver real-time data back and forth. As a standard DDP has very clear documents that show you the type of API you need to implement to make your own back-end. Need Go, Scala, or Erlang to scale out your contact syncronization service? Just write the backend and serve up web socket based data directly to your Meteor app. In the main app just use the `DDP.connect()` call to create a new socket.

DDP gives Meteor users the key to unlocking microservices for your applications. Once you conform to the API, you are free to create any service using different programming languages, databases, hardware and software environment, depending on what fits best.

I believe that DDP is a fundamental part of the Meteor platform and a key innovation that will hopefully spread to other platforms and frameworks as a standard.

## Blaze, the default front-end  option

Meteor 1.0 shipped with a Handlebars based template rendering system called Blaze. Blaze uses jQuery and Backbone style event maps to power the interactive front-ends that people build. Blaze comes with Spacebars, a reactive version of handlebars that works with the client-side Mongo cache to ensure things get updated in the UI when the mongo data changes on the back end.

Blaze has been great for helping people get acclimated into the work of updating front-end applications, while building Meteor apps. Frankly, the team has done a great job at building something with the resources they have available to do it. I would never expect Meteor Development Group to be 100% as good as huge teams at Google or Facebook working on other front-end client rendering options.

## Adding Angular or React as front-end options

The world of javascript is vast and always expanding at a rapid pace. Recently we started seeing some great options for building interactive front-ends for connected client apps. The Meteor Development Group realized it would make a lot more sense to support additional front-end options as a way to encourage more people to try and build apps with Meteor.

With Meteor 1.2, you get options to add and utilize React or Angular as your front-end stack choice. That means you can easily migrate your app on to the Meteor stack. Or you can go build something brand new using things like React Components and the real-time data updating abilities of Meteor and it's minimongo cache.

## MongoDB and the lack of other 'official' options

The database is another area that is ripe for adding extra technologic options. Currently only MongoDB is 'officially' supported as a package developed directly by MDG. Plenty of community efforts have already started to push the options of what is available with options like MySQL and RethinkDB. Developers from Meteor Development Group have also started on an [official package for Postgres](https://github.com/meteor/postgres-packages).

While DDP gives us the ability to bypass this particular point of extensibility, the work currently going on to add postgres to the officially supported DB list will yield enough core changes to make this process easy to repeat for others down the road.

## Improving the build tool for things like Cordova

Our last point in the ultimate Meteor extensibility guide has to do with the build tool itself. When you system is build on top of NPM and it's purpose is to output html/css, you start to get some interesting options available. With the 1.0 release looming, Slava at MDG took two weeks back in August of 2014 to work on getting a deeper integration directly with Cordova. Having this centralized point of building apps gave them an easy way to hook in Cordova straight into the build system.

Outputting an Xcode project is as simple as `meteor add-platform iOS` and running `meteor run ios-device`, it will open up Xcode and point it at the outputted Xcode project from the build tool. Currently you can build apps for iOS and Android.