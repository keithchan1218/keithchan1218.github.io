---
layout: post
title: REST API with Express
subtitle: Normal API services
tags: [API]
comments: true
---

API service is a back-end technique that providing clients database information with interface.
Express is a npm module for us to create API services quickly.

***
#### CRUD

{: .box-note}
CRUD-like approach: GET POST PUT DELETE with HTTP request

```javascript
  app.get('/api/transport',(req,res)=>{
    res.send('Success');
  });
```
