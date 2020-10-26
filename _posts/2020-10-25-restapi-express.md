---
layout: post
title: REST API with Express
subtitle: MySQL and Express
tags: [API, MySQL]
comments: true
---

API service is a back-end technique that providing clients database information with interface.
Express is a npm module for us to create API services quickly.

***
#### CRUD

CRUD-like approach: GET POST PUT DELETE with HTTP request

```javascript
  app.get('/api/transport',(req,res)=>{
    res.send('Success');
  });
```

#### Architecture
1. model
2. controller
3. route

First, you need to design what is the purpose of your API service.

For example, I choose transportation as my topic. So I defined the class for them.
> **Bus** 🚌🚏
>
> - Name of transportation
> - How many people hold by the bus
> - Avaliablity (maybe service in maintenance or other issue)

The next mission: make the configuration with MySQL connection

...

Then use Sequelize module to make CRUD methods

...

See more on my GitHub project: [https://github.com/keithchan1218/rest-api-mysql](https://github.com/keithchan1218/rest-api-mysql)

Credit: [bezkoder](https://bezkoder.com/node-js-express-sequelize-mysql/)
