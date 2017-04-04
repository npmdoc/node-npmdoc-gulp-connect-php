# api documentation for  [gulp-connect-php (v0.0.8)](https://github.com/micahblu/gulp-connect-php#readme)  [![npm package](https://img.shields.io/npm/v/npmdoc-gulp-connect-php.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-gulp-connect-php) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-gulp-connect-php.svg)](https://travis-ci.org/npmdoc/node-npmdoc-gulp-connect-php)
#### Starts a php server

[![NPM](https://nodei.co/npm/gulp-connect-php.png?downloads=true)](https://www.npmjs.com/package/gulp-connect-php)

[![apidoc](https://npmdoc.github.io/node-npmdoc-gulp-connect-php/build/screenCapture.buildNpmdoc.browser._2Fhome_2Ftravis_2Fbuild_2Fnpmdoc_2Fnode-npmdoc-gulp-connect-php_2Ftmp_2Fbuild_2Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-gulp-connect-php/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-gulp-connect-php/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-gulp-connect-php/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "author": {
        "name": "micahblu"
    },
    "bugs": {
        "url": "https://github.com/micahblu/gulp-php/issues"
    },
    "dependencies": {
        "bin-version-check": "^2.1.0",
        "opn": "^1.0.0"
    },
    "description": "Starts a php server",
    "devDependencies": {
        "mocha": "^2.1.0",
        "supertest": "^0.15.0"
    },
    "directories": {},
    "dist": {
        "shasum": "b8532f84488e0f215e3debd8ed288475636c6aae",
        "tarball": "https://registry.npmjs.org/gulp-connect-php/-/gulp-connect-php-0.0.8.tgz"
    },
    "gitHead": "15a0f6519c1accdda66cf8d268ab7f4d25e41fc4",
    "homepage": "https://github.com/micahblu/gulp-connect-php#readme",
    "keywords": [
        "gulp",
        "php",
        "server",
        "connect"
    ],
    "license": "MIT",
    "main": "index.js",
    "maintainers": [
        {
            "name": "micahblu",
            "email": "micahblu@gmail.com"
        }
    ],
    "name": "gulp-connect-php",
    "optionalDependencies": {},
    "readme": "ERROR: No README data found!",
    "repository": {
        "type": "git",
        "url": "git+https://github.com/micahblu/gulp-connect-php.git"
    },
    "scripts": {
        "test": "echo \"Error: no test specified\" && exit 1"
    },
    "version": "0.0.8"
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module gulp-connect-php](#apidoc.module.gulp-connect-php)
1.  [function <span class="apidocSignatureSpan">gulp-connect-php.</span>closeServer (cb)](#apidoc.element.gulp-connect-php.closeServer)
1.  [function <span class="apidocSignatureSpan">gulp-connect-php.</span>server (options, cb)](#apidoc.element.gulp-connect-php.server)



# <a name="apidoc.module.gulp-connect-php"></a>[module gulp-connect-php](#apidoc.module.gulp-connect-php)

#### <a name="apidoc.element.gulp-connect-php.closeServer"></a>[function <span class="apidocSignatureSpan">gulp-connect-php.</span>closeServer (cb)](#apidoc.element.gulp-connect-php.closeServer)
- description and source-code
```javascript
closeServer = function (cb) {
		var child = exec('lsof -i :' + workingPort,
		  function (error, stdout, stderr) {
		    //console.log('stdout: ' + stdout);
		    //console.log('stderr: ' + stderr);
		    if (error !== null) {
		      console.log('exec error: ' + error);
		    }

		    // get pid then kill it
		    var pid = stdout.match(/php\s+?([0-9]+)/)[1];
		    if (pid) {
		    	exec('kill ' + pid, function (error, stdout, stderr) {
		    		//console.log('stdout: ' + stdout);
		   			//console.log('stderr: ' + stderr);
		    		cb();
		    	});
		    } else {
		    	cb({error: "couldn't find process id and kill it"});
		    }
		});
	}
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.gulp-connect-php.server"></a>[function <span class="apidocSignatureSpan">gulp-connect-php.</span>server (options, cb)](#apidoc.element.gulp-connect-php.server)
- description and source-code
```javascript
server = function (options, cb){
		if (!cb) {
			cb = function(){};
		}

		options = extend({
			port: 8000,
			hostname: '127.0.0.1',
			base: '.',
			open: false,
			bin: 'php',
			root: '/',
			stdio: 'inherit'
		}, options);

		workingPort = options.port;
		var host = options.hostname + ':' + options.port;
		var args = ['-S', host, '-t', options.base];

		if (options.ini) {
			args.push('-c', options.ini);
		}

		if (options.router) {
			args.push(require('path').resolve(options.router));
		}

		binVersionCheck(options.bin, '>=5.4', function (err) {
			if (err) {
				cb();
				return;
			}
			var checkPath = function(){
			    var exists = fs.existsSync(options.base);
			    if (exists === true) {
			        spawn(options.bin, args, {
						cwd: '.',
						stdio: options.stdio
					});
			    }
			    else{
				setTimeout(checkPath, 100);
			    }
			};
			checkPath();
			// check when the server is ready. tried doing it by listening
			// to the child process 'data' event, but it's not triggered...
			checkServer(options.hostname, options.port, function () {
				if (options.open) {
					open('http://' + host + options.root);
				}
				cb();
			}.bind(this));
		}.bind(this));
	}
```
- example usage
```shell
...
## Usage

'''js
var gulp = require('gulp'),
    connect = require('gulp-connect-php');

gulp.task('connect', function() {
	connect.server();
});

gulp.task('default', ['connect']);
'''

## Examples
...
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
