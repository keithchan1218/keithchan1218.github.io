---
title: REST API with Express
subtitle: MySQL and Express
tags:
  - API
comments: true
---

# REST API with Express

API service is a back-end technique that providing clients database information with interface. Express is a npm module for us to create API services quickly.

***

### Brief

#### Basic API

CRUD-like approach: GET POST PUT DELETE with HTTP request

```javascript
  app.get('/api',(req,res)=>{
    res.send('API is running');
  });
```

When you type `localhost/api` on the browser, you will see a sentence 'API is running' in a white page

If you are not familiar with this basic API setup, please go to my [Basic API project](https://github.com/keithchan1218/rest-api-express). It will give you a full explanation of REST API and how it works.

If you know how to make regular API, let's keep going 🔥🔥🔥

***

### Get Start

#### Architecture

1. model
2. controller
3. route

\


**Step 1 - Model**

First, you need to design the purpose of your API service.

For example, I choose transportation as my topic. So I defined the **class** for them.

e.g. **Bus** 🚌🚏

| Item     | Detail                                               |
| -------- | ---------------------------------------------------- |
| name     | Name of transportation                               |
| capacity | How many people hold by the bus                      |
| enabled  | Availability (service in maintenance or other issue) |

```javascript
module.exports = (sequelize, Sequelize) => {
    const Transportation = sequelize.define("transportation", { // "transportation" will be your table name
        name: {
            type: Sequelize.STRING
        },
        capacity: {
            type: Sequelize.STRING
        },
        enabled: {
            type: Sequelize.BOOLEAN
        }
    });

    return Transportation;
};
```

\


**Step 2 - DB Connection**

After making this module, export it for other files in this project.

Make the configuration with MySQL connection and use Sequelize ORM that you don't need to write query to do normal CRUD action.

```
npm install sequelize mysql2 --save
```

```javascript
const sequelize = new Sequelize('database', 'username', 'password', {
  host: 'localhost',
  dialect: 'mysql'
});
```

\


**Step 3 - Controller**

Then use Sequelize CRUD methods

* exports.create (POST)
* exports.findAll (GET)
* exports.findOne (GET with name)

```javascript
exports.create = (req, res) => {
    // validation
    if (!req.body.name) {
        res.status(400).send({
            message: "Name cannot be empty!"
        });
        return;
    }

    // create the transportation item
    const item = {
        name: req.body.name,
        capacity: req.body.capacity,
        enabled: req.body.enabled ? req.body.enabled : false
    };

    // DB Action: save the item to table
    Transportation.create(item)
        .then(data => {
            res.send(data);
        })
        .catch(err => {
            res.status(500).send({
                message:
                    err.message || "Some error occurred while creating the transportation."
            });
        });
};

exports.findAll = (req, res) => {
    const name = req.query.name;
    var condition = name ? { name: { [Op.like]: `%${name}%` } } : null;

    Transportation.findAll({ where: condition })
        .then(data => {
            res.send(data);
        })
        .catch(err => {
            res.status(500).send({
                message:
                    err.message || "Some error occurred while retrieving transportation."
            });
        });
};

exports.findOne = (req, res) => {
    const name = req.params.name;

    Transportation.findByPk(name)
        .then(data => {
            res.send(data);
        })
        .catch(err => {
            res.status(500).send({
                message: "Error retrieving Transportation with name=" + name
            });
        });
};
```

\


**Step 4 - Setup routes**

```javascript
module.exports = app => {
    const transportation = require("../controllers/transportation.controller");

    var router = require("express").Router();

    router.post("/", transportation.create);
    router.get("/", transportation.findAll);
    router.get("/:name", transportation.findOne);

    app.use('/api/transportation', router);
};
```

Test your API with [Postman](https://www.postman.com/)

See more on my GitHub project: [https://github.com/keithchan1218/rest-api-mysql](https://github.com/keithchan1218/rest-api-mysql)

Credit: [bezkoder](https://bezkoder.com/node-js-express-sequelize-mysql/)
