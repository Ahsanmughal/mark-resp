# Gulp Task Builder

## Description
Node module task builder which can be configured by a json object contianing the relevant tasks to build.


Table of Contents
-----------------
**Getting Started**

1.[setup]()
  - [install]()
  - [usage]()
   
2.[Features]()
  - [Compress]()
  - [Concatinate]()
  - [Replace]()
  - [Rename]()
  - [Wrapper]()
  
Setup
============

### Install
```npm install gulp-task-builder --save ```

### Usage
```sh
var builder = require('gulp-task-builder')
var tasks = {
  "task1":{src:"[path/to/source/files" , dest:"path/to/save"}
  builder.loadTasks(tasks);
  builder.runTasks();
}
```

[Task Options]()

Examples
=============

### Basic Example
with **Required Options**

```sh
var builder = require('gulp-task-builder')
var tasks = {
  "task1":{src:'./src/*.js' , ext:".js" , dest:"dest"},
  "task2":{src:'./packages/*.js' , ext:".js" , dest:"dest/lib"}
}
builder.loadTasks(tasks);
builder.runTasks();
```
### Compress
Compress your files with the ``compress`` option.This function is using [gulp-uglify]() for javascript, [gulp-htmlmin]() for html and [gulp-clean-css] for css files. 

```sh
{src:"./src/*.js", ext:".js", dest:"dest", compress:true}
{src:"./src/*.html", ext:".html", dest:"dest", compress:{collapseWhitespace: true}}
{src:"./src/*.css", ext:".css", dest:"dest", compress:{compatibility: 'ie8'}}
```
for more ``css`` options [Click here]().

### Concatenate

Concatenate (join) your files with the ``concat`` here.

```sh
//File name will be task1.js which is the task name
task1:{src:"./src/*.js", ext:".js", dest:"dest", concat:true}

//File name will be jsbundle.js which is the task name
task1:{src:"./src/*.js", ext:".js", dest:"dest", concat:true, name:"jsbundle"}
```

### Filter
Filter your source files with the ``filter`` option.

```sh
{src:"./src/*.js", ext:".js", dest:"dest", filter:'!src/vendor'}
{src:"./src/*.js", ext:".js", dest:"dest", filter:['*', '!src/vendor']}
{src:"./src/*.js", ext:".js", dest:"dest", filter:{match:['*', '!src/vendor'], options:{restore:true, passthrough:true, dot:true}}}
{src:"./src/*.js", ext:".js", dest:"dest", filter:function(file){ /*You can access file.cwd, file.base, file.path and file.contents */ }}
```
restore and pass through will come very soon.

### Rename
Rename your destination file or path.You can provide **String | Function | Object **
```sh
{src:"./src/*.js", ext:".js", dest:"dest", rename:"main/text/ciao/goodbye.md"}
{src:"./src/*.js", ext:".js", dest:"dest", rename:function (path) { path.dirname += "/ciao"; path.basename += "-goodbye"; path.extname = ".md" }}
{src:"./src/*.js", ext:".js", dest:"dest", rename:{dirname: "main/text/ciao", basename: "aloha", prefix: "bonjour-", suffix: "-hola", extname: ".md"}}
```

### Wrapper
Wrap your files or target files with the given header and footers ** Object | Array **.

```sh
{src:"./src/*.js", ext:".js", dest:"dest", wrapper:{header:"this will be header", footer:"this will be footer"}}
{src:"./src/*.js", ext:".js", dest:"dest", wrapper:[{header:"header1", footer:"footer1"}{header:"headerN", footer:"footerN"}]}
```
### Log Contents
You can also log paths contents and other stream options.In case set to true the default value will be ``contents``.
```sh
{src:"./src/*.js", ext:".js", dest:"dest", log:true}
{src:"./src/*.js", ext:".js", dest:"dest", preLog:true}
{src:"./src/*.js", ext:".js", dest:"dest", preLog:'path'}// Console Paths
```
### Disable save
You can also disable the save option by setting `save:false`
```sh
{src:"./src/*.js", ext:".js", dest:"dest", save:false}
```

Task Options
-------------

Each Task contains **Required** options which can be passed along with **optionals** (Flow Contorl , Plugins, Log options)

### Required

#### Required Options

- **src** (string) Gulp [src]() parameter.Path of your source files.It can also be regEx.[More Details]().
- **dest** (string) Gulp [dest]() parameter.Path where you want to save your files.[More Details]().
- **ext** (string) extension of file which is defined in ``src`` option.

### Optionals

#### Flow Control

- **runBefore** (string | Array(string)) Define Task dependincies which will run before this task.
- **Save** (bool) set ``true``.If you want to save your output.Default ``True``.
- **name** string **recomended**.Define Unique name of gulp task.

#### Plugins

- **filter** (Object|string|array|function) To filter your files. if you are sending Object then that object should have two properties [match]() and options. See [gulp-filter]() for more details.
- **concat** (string|object) object contains two properties name and ext. See [gulp-concat]() for more details.
- **replace** (object|array(objects)) Object can be one of these two objects ``{target:"", src:""}`` this will send to [gulp-replace]() and second ``{buildName:replacement}`` buildName (string|RegExp) replacement (String|Array|Object).
- **debug**(bool) Enable [gulp-plumber]()
- **compress** (bool | object) this will use [gulp-uglify]() if ``ext`` is ``.js`` [gulp-htmlmin]() and [gulp-clean-css]() if ``css`` is ``.css``.
- **wrapper** (object | Array) Each object has two options header and footer.[More Details]().
- **rename** (string | object | function) you can edit the name or edit the path of your destination file.[More Details]().
 
#### Log Options
- **log** (string | bool) log/console content of stream.All options of [gulp-stream]() are supported.Default value is ``contents``.
- **PreLog** (bool) console stream before processing.same as ``log`` above.
- **postLog** (bool) console stream just before save ``(gulp.dest)`` function stream.same as ``log`` above.
- **get** (function) just in case if you want to get stream.Note it will not effect the stream.

License
------------
**MIT**



