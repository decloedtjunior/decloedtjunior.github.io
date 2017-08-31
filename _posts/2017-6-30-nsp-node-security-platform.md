---
layout: post
sitemap: true
noindex: false
title: Node Security Platform (nsp)
categories: node npm nsp cheatsheet
---

**nsp** is the main command line interface to the Node Security Platform. It allows you to audit a package.json _or npm-shrinkwrap.json_ file, scanning and detecting the use of libraries with known vulnerabilities. You can run a security check manually, or on each _npm install_, _npm start_, _pull request_, etc... 

# Installation
```
npm install nsp --global
```

# Example Usage

From inside your project directory
```
nsp check
```

# gulp-nsp

A gulp plugin that runs the Node Security Platform audit on your package.json or npm-shrinkwrap.json

## Installation

```
npm install gulp-nsp --save-dev
```

Then in your gulpfile, add the following task like so:

```js
var gulpNSP = require('gulp-nsp');
 
//To check your package.json
gulp.task('nsp', function (cb) {
  gulpNSP({package: __dirname + '/package.json'}, cb);
});
 
//To check your shrinkwrap.json
gulp.task('nsp', function (cb) {
  gulpNSP({shrinkwrap: __dirname + '/npm-shrinkwrap.json'}, cb);
});
 
//If you don't want to stop your gulp flow if some vulnerabilities have been found use the stopOnError option:
gulp.task('nsp', function (cb) {
  gulpNSP({
    package: __dirname + '/package.json',
    stopOnError: false
  }, cb);
});
 
//For enterprises building behind a proxy (HTTP_PROXY or HTTPS_PROXY), use the proxy option:
gulp.task('nsp', function (cb) {
  gulpNSP({
    shrinkwrap: __dirname + '/npm-shrinkwrap.json',
    proxy: process.env.HTTPS_PROXY
  }, cb);
});
```

# Retire.js - another alternative

[http://retirejs.github.io/retire.js/](#http://retirejs.github.io/retire.js/)

Retire.js can be used in many ways:

 - As command line scanner
 - As a grunt plugin
 - As a gulp task
 - As a Chrome extension
 - As a Firefox extension
 - As a Burp and OWASP Zap plugin
