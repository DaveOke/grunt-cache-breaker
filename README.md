# grunt-cache-breaker [![Build Status](https://travis-ci.org/shakyShane/grunt-cache-breaker.png?branch=master)](https://travis-ci.org/shakyShane/grunt-cache-breaker)


    <!-- Turn these -->
    <script src="/js/dist/combined.min.js"></script>
    <link href="/css/style.css"></link>

    <!-- Into these -->
    <script src="/js/dist/combined.min.js?rel=123456"></script>
    <link href="/css/style.css?rel=123456"></link>

    <!-- Or these -->
    <script src="/js/dist/combined.min.123456.js"></script>
    <link href="/css/style.123456.css"></link>

    <!-- Or these (md5 hash of file contents) -->
    <script src="/js/dist/combined.min.ow23de343.js"></script>
    <link href="/css/style.ow23de343.css"></link>


## Getting Started

This plugin requires Grunt `~0.4.1`

If you haven't used [Grunt](http://gruntjs.com/) before, be sure to check out the [Getting Started](http://gruntjs.com/getting-started) guide, as it explains how to create a [Gruntfile](http://gruntjs.com/sample-gruntfile) as well as install and use Grunt plugins. Once you're familiar with that process, you may install this plugin with this command:

```shell
npm install grunt-cache-breaker --save-dev
```
Once the plugin has been installed, add this to your Gruntfile.js

    grunt.loadNpmTasks('grunt-cache-breaker');

And then add one of the following to your list of tasks

##Examples

**Append timestamps as query strings**

Very useful in the development stages, not to be used in production though (see the other examples)

```js
// Append a timestamp to 'all.min.js' & 'core.min.js' which are both located in 'index.html'
cachebreaker: {
    dev: {
        options: {
            match: ['all.min.js', 'core.min.css'],
        },
        files: {
            src: ['index.html']
        }
    }
}
```

**Append timestamps as filename changes**

A great idea if you have server rewrites. The file names are not changed, just changed in the markup.

File urls will be rewritten to `all.min.4252425.js`, for example

```js
// Append a timestamp to 'all.min.js' & 'core.min.js' which are both located in 'index.html'
cachebreaker: {
    dev: {
        options: {
            match: ['all.min.js', 'core.min.css'],
            position: 'filename'
        },
        files: {
            src: ['index.html']
        }
    }
}
```

**Append timestamps as filename changes in certain positions**

Again, to be used with server rewrites, this allows you specify which 'piece' of the filename is rewritten.

File urls will be rewritten to `all.4252425.js`, for example.

```js
// Append a timestamp to 'all.min.js' & 'core.min.js' which are both located in 'index.html'
cachebreaker: {
    dev: {
        options: {
            match: ['all.*.js', 'core.*.css'],
            position: 'overwrite'
        },
        files: {
            src: ['index.html']
        }
    }
}
```

##MD5 hash
Use the contents of a file to generate a hash instead of a timestamp. Works in all positions mentioned above.

This example will create links like this: `all.min.js?rel=hetweyj332` - which is useful as your templates only change
when the contents of the file change.

```js
// Append a md5 hash to 'all.js' which is located in 'index.html'
cachebreaker: {
    dev: {
        options: {
            match: ['all.js'],
            replacement: 'md5',
            src: {
                path: 'app/all.js'
            }
        },
        files: {
            src: ['index.html']
        }
    }
}
```

##Custom replacement
You can also provide your own replacement if the Timestamps or MD5 hashes are not right for you.

This example would change the URL of app.js, to `app.js?rel=v_2_0`

```js
// Append a custom string to 'all.js' which is located in 'index.html'
cachebreaker: {
    dev: {
        options: {
            match: ['all.js'],
            replacement: function (){
                return "v_2_0"
            }
        },
        files: {
            src: ['index.html']
        }
    }
}
```

Of course, as with all examples, you're free to mix & match the options. For example, you could use
the custom method above, but to change the filename in the markup instead of a query string.

```js
// Change filename in the markup to include custom string
cachebreaker: {
    dev: {
        options: {
            match: ['all.js'],
            position: 'filename',
            replacement: function (){
                return "v_2_0"
            }
        },
        files: {
            src: ['index.html']
        }
    }
}
```


## Contributing
In lieu of a formal styleguide, take care to maintain the existing coding style. Add unit tests for any new or changed functionality. Lint and test your code using [Grunt](http://gruntjs.com/).

