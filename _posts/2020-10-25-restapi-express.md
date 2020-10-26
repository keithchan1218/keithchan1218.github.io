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

See more on my GitHub project: [https://github.com/keithchan1218/rest-api-express](https://github.com/keithchan1218/rest-api-express)

Credit: [bezkoder](https://bezkoder.com/node-js-express-sequelize-mysql/)
