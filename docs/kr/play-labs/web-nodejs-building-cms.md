---
layout: default
title: Building a CMS
nav_order: 1
parent: web
grand_parent: play-labs
---


~~~ java
//dependencies
    "async": "2.6.0",
    "body-parser": "~1.12.0",
    "cookie-parser": "~1.3.4",
    "cookie-session": "2.0.0-beta.3",
    "ejs": "~2.5.9",
    "excel": "^1.0.0",
    "express": "~4.16.3",
    "moment": "~2.22.1",
    "mysql": "~2.15.0",
    "xlsx": "^0.12.12"
~~~ 
<br/>
~~~ javascript
//services.js
var express = require('express');
var http = require('http');
var path = require('path');

var cookieParser = require('cookie-parser');
var session = require('cookie-session');
var bodyParser = require('body-parser');
var login = require('./routes/login');
var customer = require('./routes/customer');
var services = express();
services.set('port', process.env.PORT || 8813);
services.set('views', path.join(__dirname, 'views'));
services.set('view engine', 'ejs');
services.use(bodyParser.urlencoded({ extended: false }));
services.use(bodyParser.json());
services.use(cookieParser());
services.use(session({keys: ['services_uid']}));
services.use(express.static(path.join(__dirname, 'public')));
...
services.get('/', login.login);
services.get('/logout', login.logout);
services.post('/onLogin', login.onLogin);
...
services.use(function(req, res, next) {
    next(createError(404));
});

// error handler
services.use(function(err, req, res, next) {
    // set locals, only providing error in development
    res.locals.message = err.message;
    res.locals.error = req.app.get('env') === 'development' ? err : {};

    // render the error page
    res.status(err.status || 500);
    res.render('error');
});

http.createServer(services).listen( services.get('port'), function(){
    console.log('Express server listening on port ' + services.get('port'));
});

module.exports = services;
~~~ 
<hr/>

~~~ java
//database
tb_datas
tb_users
tb_stores
~~~ 


Agency List
<br/>
![](../../../../assets/images/web-node/other.png)

Agency List Edit
<br/>
![](../../../../assets/images/web-node/tenpo_edit.png)
<br/>

Agency List 
<br/>
![](../../../../assets/images/web-node/agency_search_bar.png)



{: .fs-6 .fw-300 }
