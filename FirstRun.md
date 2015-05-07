## Travel Republic - Gulp First Configuration

In order to have a working environement plese run the following commands:

    npm prune               // removes unused packages
    npm install             // adds and update packages

    npm install -g gulp     // adds gulp as a global command

    gulp build              // builds release files
    gulp buildDevVendors    // build vendor files for developing

    gulp mobile             
        or                  // start the watch best suited for the device you are working on
    gulp desktop
