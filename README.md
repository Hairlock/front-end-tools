## Travel Republic - Gulp Guide

1. [Watch Tasks](#watch-tasks)
1. [Codebase Tasks](#codebase-tasks)
1. [Stylesheet Tasks](#stylesheets-tasks)
    * [Stylesheet Indexes](#less-indexes)
1. [Vendors Tasks](#vendors-tasks)
    * [VENDOR_VERSION](#vendor_version)
1. [Test Tasks](#test-tasks)

### Watch Tasks

There are two watch commands, one per device:

`gulp mobile`

`gulp desktop`

The first run on your machine will require around _16s_, this time is needed to transpile the whole
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

This two commands will build the correspondant codebase, the build is composed by the following steps:

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

Every device has an index that define the structure of the different stylesheets. These indexes
can be found in:

    client/css/{device-name}/indexes/

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
In order to keep this consistency, please, do not use `@import` inside any other `.less` file.

### Vendors Tasks

There are 3 development vendors tasks

`gulp desktopDevVendors`

`gulp mobileDevVendors`

`gulp buildDevVendors`


and 3 release vendors tasks

`gulp buildVendorsDesktop`

`gulp buildVendorsMobile`

`gulp buildVendors`

Unless the `VENDOR_VERSION` value is changed `gulp` will use the previously generated bundles.

#### VENDOR_VERSION

Vendors use a version value similar to the one used in DNS configuration files

    ddmmyynn

        dd -> 15
        mm -> 04
        yy -> 15
        nn -> 01 (number of times the value has changed in the single day)

    Example: 15041501

This value is supposed to be changed whenever a vendor library is added, removed, updated or edited.
Vendors libraries must be added to or removed from the specific directory:

    client/js/vendors/

in the `gulp.paths.vendors.js` and in `karma-common.js` files.

### Test Tasks

There are 2 development test tasks

`gulp testDesktop`

`gulp testMobile`

and 2 release test tasks

`gulp testDesktopRelease`

`gulp testMobileRelease`

The main difference between this two groups is the output produced (console vs teamcity)
