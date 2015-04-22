## Travel Republic - Gulp Guide

1. [Watch Tasks](#watch-tasks)
1. [Codebase Tasks](#codebase-tasks)

### Watch Tasks

There are two watch commands, one per device:

`gulp mobile`

`gulp desktop`

The first run on your machine will require around _16s_, this time is needed to tanspile the whole
`.js` codebase and generate the cache file for `babel`.

After the first run the `watch` initialization time should take around _4.5s_

On every `.js` file change the codebase will be incrementally rebuilt, this should take around
_0.7s_, when completed all the linked browser tabs will be reloaded.

`jshint` errors will be displayed on the watch console, same goes for `babel` errors.

On every `.less` file change all the [stylesheet indexes](#less-indexes) file will be recompiled,
this should take around _0.8s_, when completed all the linked browser tabs will be updated through
_live reload_ (no page refresh).

`less` compilation errors will appear on the watch console.

### Codebase Tasks

There are two codebase tasks

`gulp desktopBundle`

`gulp mobileBundle`

This two commands will build the relative codebase, the build is composed by the following steps:

1. **jshint check**
1. **babel** -> from ES6 to ES5
1. **code concatenation**
1. **angular annotation** -> in order to preserve DI after minification
1. **minification**
1. **sourcemap generation**

Any error during the previous steps will halt the build process and log the issue to screen.

### Stylesheets Tasks

There are two stylesheets tasks

`gulp mobileCssBundle`

`gulp desktopCssBundle`

This two command will build the relative CSS bundle, the build is composed by the following steps:

1. **compilation from index files**
1. **minification**

#### Less Indexes

Every device have an index that define the structure of the different stylesheets. These indexes
can be found in:

    /client/css/{device-name}/indexes

The name of an index is structured like this:

    {tenant}.{culture}.less

    dnata.en-gb.less

A index file look like this:

    @import (inline) '../../common/normalize.css';
    @import (inline) '../../common/owl-carousel.css';
    @import (inline) '../../common/jquery-ui-datepicker.css';
    @import '../../common/main.less';
    @import '../themes/dnata/dnata.less';
    @import '../app.less';
    @import '../themes/dnata/en-gb.less';

All the call to `@import` in the `.less` files have been moved here, therefore a index file
describes the structure of the final stylesheet.
In order to keep this consistency do not use `@import` inside any other `.less` file, unless
strictly necessary.
