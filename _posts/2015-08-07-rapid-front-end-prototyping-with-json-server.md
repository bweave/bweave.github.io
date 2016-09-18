---
layout:     post
title:      JSON-Server
date:       2015-08-07
author:     Brian Weaver
summary:    Rapid front-end prototyping with JSON-Server.
category:   frontend-dev
thumbnail:  server
tags:
 - js
 - json
 - prototyping
---

<p class="lead"><strong>TLDR;</strong></p>

[JSON-Server](https://github.com/typicode/json-server) is a node module "created with &lt;3 for front-end developers who need a quick back-end for prototyping and mocking."

### Features

- In memory RESTful API
- Generate resources from JSON or programatically
- Basic resource relationships, ie. post comments
- Resource filtering and searching
- Static File Server
- Load remote schemas
- Add routes
- Use it as [Express](http://expressjs.com) Middleware

I feel like JSON-Server is worth a little deeper dive than we've been taking because it's just so stinkin' useful right out of the box! I've been using it this week at work to prototype a mobile app for our annual [Be Rich](http://howtoberich.org) giving campaign. The stakeholders involved have a rough idea of what they want, but it's still a little vague. In order to get something in their hands that we can iterate on, I fired up a new [Ionic](http://ionicframework.com) project and started work. I quickly realized that I needed more data to work with than I could managealby stuff into an [Angular](http://angularjs.org) "API" service. And since we're still in the prototype stage, I didn't want to take the time to setup an entire [Laravel](http://laravel.com) app with a DB and migrations, etc. So, JSON-Server.

----

### Getting Started

The docs and examples for JSON-Server are great. You can get started with just a simple JSON file as your DB. For me though, things really get cookin' when you generate your resources programatically. That's what we're going to take a look at now.

Install JSON-Server globally:

```bash
$ npm install -g json-server
```

Pull [Faker](https://github.com/Marak/faker.js) into your project for generating random data:

```bash
$ npm install --save-dev faker
```

Create ```db.js``` with the following content:

```javascript
var faker = require('faker');

module.exports = function() {
  // Each property added to the **data** object
  // will create a RESTfull resource
  var data = { users: [] }

  // Create 1000 users
  for (var i = 0; i < 1000; i++) {
    data.users.push({
      id: i + 1,
      fname: faker.name.firstName,
      lname: faker.name.lastName,
      job: faker.name.jobTitle,
      phone: faker.phone.phoneNumber,
      imgUrl: faker.image.imageUrl,
    })
  }

  return data
}
```

And fire it up:

```bash
$ json-server db.js
```

Open ```localhost:3000``` in your browser and you'll have access to these routes:

```bash
GET    /users
GET    /users/1
POST   /users
PUT    /users/1
PATCH  /users/1
DELETE /users/1
```

As you work with your data, typing ```s``` in the console will save a snapshot of your DB. This can be super handy for creating, "gold standard" data sets or edge case scenarios.

----

### Conclusion

JSON-Server makes it laughably easy to mock and mold the API data I need to get a prototype into my stakeholder's hands. So what are you waiting for? Give it a shot!
