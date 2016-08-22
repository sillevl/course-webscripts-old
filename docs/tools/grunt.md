# Grunt task runner

## Why use a task runner?
In one word: **automation**. The less work you have to do when performing repetitive
tasks like minification, compilation, unit testing, linting, etc, the easier your
job becomes. After you've configured it through a `Gruntfile`, a task runner can
do most of that mundane work for you—and your team—with basically zero effort.

Grunt website: <http://gruntjs.com/>

### Installation

Grunt makes use of two npm packages. 'grunt-cli' to make the *grunt* command globally
available, and 'grunt' which will add the task runner's code to your project.

#### grunt-cli
You must install the `grunt-cli` command line interface tool globally on your system.
This will enable you to call the *grunt* command from everywhere inside your commandline (such as powershell, bash,...)
```
npm install -g grunt-cli
```

#### grunt
This package needs to be installed in every project you where you want to use the Grunt task runner.
Don't forget to add the `--save-dev` option to the command to save the dependency into the `package.json` file
```
npm install grunt --save-dev
```

## Gruntfile.js

All tasks and their configuration is managed in the `Gruntfile.js` file.
This is an example of a minimal configuration.

```js
module.exports = function(grunt) {

    grunt.initConfig({

        // ... task configuration

    });
};
```

## Check if PHP is PSR-2 compliant
Make use of the PHP CodeSniffer package to check if php files comply to the PSR recommendations.
More information: <https://github.com/SaschaGalley/grunt-phpcs>

!!! attention
    PHP Code Sniffer must be installed on the system (preferably with composer).

1) Install the grunt-phpcs plugin
```
npm install grunt-phpcs --save-dev
```
2) Load the task in your projects Gruntfile. Add the following `gruntfile.js`
```
grunt.loadNpmTasks('grunt-phpcs');
```

### Example configuration
This example will check the index.php file and all the PHP files in the lib directory (even in subdirectories)

```js
module.exports = function(grunt) {

    grunt.initConfig({
        phpcs: {
          	application: {
          		src: ['./index.php', './lib/**/*.php']
          	},
          	options: {
          		bin: 'phpcs',
          		standard: 'PSR2'
          	}
        }
    });

    grunt.loadNpmTasks('grunt-phpcs');

    grunt.registerTask('default', ['watch']);
};
```

## Merging JavaScript files

Merge multiple JavaScript file's into a single file. These JavaScript files
can include files from libraries or custom scripts.

## Compiling sass

Compile Sass code into css, and merge libraries with custom code. The result
is a single css file to be referenced from html.

## Watch for file changes

When a file is changed and saved, automatically run grunt tasks.

## Example Gruntfile.js

```js
module.exports = function(grunt) {

    grunt.initConfig({
        phpcs: {
          	application: {
          		src: ['./index.php']
          	},
          	options: {
          		bin: 'phpcs',
          		standard: 'PSR2'
          	}
        },
        concat: {
            dist: {
              src: [
                  'bower_components/jquery/dist/jquery.js',
                  'bower_components/foundation-sites/dist/foundation.js',
                  'src/js/app.js'
              ],
              dest: 'js/app.js'
            }
        },
        sass: {
            options: {
                includePaths: ['bower_components/foundation-sites/scss/']
            },
            dist: {
                files: {
                    'css/app.css': 'src/scss/app.scss'
                }
            }
        },
        watch: {
          scripts: {
            files: ['index.php'],
            tasks: ['phpcs'],
            },
          js: {
              files: ['src/js/*.js'],
              tasks: ['concat']
          },
          sass: {
              files: ['src/scss/*.scss'],
              tasks: ['sass']
          }
        }
    });

    grunt.loadNpmTasks('grunt-phpcs');
    grunt.loadNpmTasks('grunt-contrib-concat');
    grunt.loadNpmTasks('grunt-sass');
    grunt.loadNpmTasks('grunt-contrib-watch');

    grunt.registerTask('default', ['watch']);
    grunt.registerTask('assets', ['sass', 'concat']);
};
```
