## Gulp Task Builder

[![Version][version-svg]][package-url] [![Build Status][travis-svg]][travis-url] [![License][license-image]][license-url]  [![Downloads][downloads-image]][downloads-url]
[version-svg]: https://img.shields.io/npm/v/gulp-task-builder.svg?style=flat-square
[package-url]: https://npmjs.org/package/gulp-task-builder
[travis-svg]: https://img.shields.io/travis/Emallates/gulp-task-builder/master.svg?style=flat-square
[travis-url]: https://api.travis-ci.org/Emallates/gulp-task-builder.svg?branch=master
[license-image]: https://img.shields.io/badge/license-MIT-green.svg?style=flat-square
[license-url]: LICENSE.txt
[downloads-image]: https://img.shields.io/npm/dm/gulp-task-builder.svg?style=flat-square
[downloads-url]: http://npm-stat.com/charts.html?package=gulp-task-builder


## Description

Node module task builder which can be configured by a JSON Object containing the relevant tasks to build.


## Table of Contents

**Getting Started **

1.[setup]()
 - [Install]()
 - [Usage]()

2.[Features]()
 - [Compress]()
 - [Concatenate]()
 - [Replace]()
 - [Rename]()
 - [Wrapper]()


 Setup
============
### Install
```npm install gulp-task-builder --save ```

### usage
```sh 
var builder = require('gulp-task-builder')
var tasks = {
    "task1":{src:"path/to/source/files", dest:"path/to/save"}
}
builder.loadTasks(tasks);
builder.runTasks(); 
```

** [Task Options]() ** 

Examples
==========
### Basic Example

with **required options**
```sh
var builder = require('gulp-task-builder')
var tasks = {
    "task1":{src:"./src/*.js", ext:".js", dest:"dest"},
    "task2":{src:"./packages/*.js", ext:".js", dest:"dest/lib"}
}
builder.loadTasks(tasks);
builder.runTasks();

```

### Compress
compress your files with **compress** option.This function is using [gulp-uglify]() for javascript , [gulp-htmlmin]() for html and [gulp-clean-css]() for css files.

```sh
{src:"./src/*.js", ext:".js", dest:"dest", compress:true}
{src:"./src/*.html", ext:".html", dest:"dest", compress:{collapseWhitespace: true}}
{src:"./src/*.css", ext:".css", dest:"dest", compress:{compatibility: 'ie8'}}

```

for more **css** options [clickhere]()

### Concatenate
Concatenate (join) your files with the concat option.

```sh
//File name will be task1.js which is the task name
task1:{src:"./src/*.js", ext:".js", dest:"dest", concat:true}

//File name will be jsbundle.js which is the task name
task1:{src:"./src/*.js", ext:".js", dest:"dest", concat:true, name:"jsbundle"}

``` 

### Filter
Filter your source files with the **filter** option.
```sh
{src:"./src/*.js", ext:".js", dest:"dest", filter:'!src/vendor'}
{src:"./src/*.js", ext:".js", dest:"dest", filter:['*', '!src/vendor']}
{src:"./src/*.js", ext:".js", dest:"dest", filter:{match:['*', '!src/vendor'], options:{restore:true, passthrough:true, dot:true}}}
{src:"./src/*.js", ext:".js", dest:"dest", filter:function(file){ /*You can access file.cwd, file.base, file.path and file.contents */ }}
```

restore and passthrough will come very soon.

### Rename
Rename your destination file or path.You can provide **String|Function|Object**.
```sh
{src:"./src/*.js", ext:".js", dest:"dest", rename:"main/text/ciao/goodbye.md"}
{src:"./src/*.js", ext:".js", dest:"dest", rename:function (path) { path.dirname += "/ciao"; path.basename += "-goodbye"; path.extname = ".md" }}
{src:"./src/*.js", ext:".js", dest:"dest", rename:{dirname: "main/text/ciao", basename: "aloha", prefix: "bonjour-", suffix: "-hola", extname: ".md"}}
```

### Wrapper
wrap your files or target files with the given headers and footer **Object|Array**
```sh
{src:"./src/*.js", ext:".js", dest:"dest", wrapper:{header:"this will be header", footer:"this will be footer"}}
{src:"./src/*.js", ext:".js", dest:"dest", wrapper:[{header:"header1", footer:"footer1"}{header:"headerN", footer:"footerN"}]
``` 

### Log Contents
You can also log paths contents and other stram options.In case set to rule the default value will be **contents**
```sh
{src:"./src/*.js", ext:".js", dest:"dest", log:true}
{src:"./src/*.js", ext:".js", dest:"dest", preLog:true}
{src:"./src/*.js", ext:".js", dest:"dest", preLog:'path'}// Console Paths
```

### Disable Save
You can also disable the save option by setting **save:false**
```sh
{src:"./src/*.js", ext:".js", dest:"dest", save:false}
```

### Task Options 
Each task Contains **REQUIRED** options which can be passed along with **OPTIONALS** (Flow Control , Plugins , Log Options)

Required 
===========
### Required Options
 - **src (string)** Gulp [src]() parameter.Path of your success files.It can be also regEx.[More Details]()
 - **dest (string)** Gulp [dest]() parameter.Path where you want to save your files.[More Details]()
 - **ext (string)** extension of file which is defined in ``src`` option.


 Optional
 ==========
 ### Flow control

 - **run before** (string | Array(string)) Define task dependencies which will run before this task.
 - **save (bool)** Set ``true`` if you want to save your output.Default ``true``.
 - **name (string)** ``Recomended``.Define Unique name of gulp task.
 - **order (Array(string))** Define flow of execution. Like ['log','filter','compress','concat','wrapper']


 ### Plugins

 - **filter (Objects | string | array | functions)** To filter your files.if you are sending object then that object should have two properties [match]() and options. See [gulp-filter]() for more details.
 - **Concat (string | object) ** object containes two properties name and ext.See [gulp-cocncat]() for more details.
 - **replace (object | array(objects))** object can be one of these two objects ``{target:"" , src:""}`` this will send to [gulp-replace]() and second ``[build Name : replacement]`` buildName (string | Array | Object).
 - **debug(bool)** Enable [gulp-number]().
 - **compress (bool | object) ** this will use [gulp-uglify]() if ``ext`` is ``.js`` [gulp-htmlmin]() if ``ext`` is ``.html`` and [gulp-clean-css]() if ``css`` is ``.css``.
 - **wrapper (object | array)** Each object has to options header and footer.[More Details]().
 - **rename (String | Object | Function) ** You can edit the name or edit the path of your detination file.[More Details]().


 ### Log Options

 - **Log (string | bool)** log / console content of stream. All options of [globe-stream]() are supported.Default value is ``contents``.
 - **PreLog (bool)** console stram before processing.same as ``log`` above.
 - **postLog (bool)** console stream just before save (``gulp.dest`` function) stream.same as ``log`` above.
 - **get (function)** just in case if you want to get stream.**Note** it will not effect the stream.


 License
 =============

**MIT**   
