# 12factor-config #

A config module which only reads the environment. For Node.js that means `process.env`.

We will not ever use `NODE_ENV` in any example here since each environment should specify
everything it needs and nothing should be dependent on being a particular
environment (such as 'development', 'testing', 'staging' or 'production').

## Synopsis ##

Firstly, set some environment variables that your program will look for. You don't need
to set any that have a 'default' but you do need to set any that are 'required'.

```bash
$ export REDIS_URL=redis://user:password@hostname:port/db
$ export APPNAME_PORT=8080
```

Then, in your program:

```javascript
var config = require('12factor-config');

var cfg = config({
    redisUrl : {
        env      : 'REDIS_URL',
        type     : 'string', // default
        required : true,
    },
    logfile : {
        env      : 'APPNAME_LOG_FILE',
        type     : 'string',
        default  : '/var/log/appname.log',
        required : true,
    },
    port : {
        env      : 'APPNAME_PORT',
        type     : 'integer',
        default  : '8000',
        required : true,
    },
    debug : {
        env      : 'APPNAME_DEBUG',
        type     : 'boolean',
        default  : false,
    },
    env : {
        env      : 'NODE_ENV',
        type     : 'enum',
        values   : [ 'development', 'test', 'stage', 'production', ],
    },
});

console.log(cfg);
```

Should output something like:

```
{
  redisUrl: 'redis://user:password@hostname:port/db',
  logfile: '/var/log/appname.log',
  port: 8080,
  debug: false,
  env: 'development'
  }
```

It is advisable to prefix your environment variables with a prefix related to your application
name as shown in the later config vars above. Mainly this is to namespace your vars and not stomp
over others already defined. Of course you don't need to use the prefix in the local name.

## What I Do ##

I usually have a `lib/cfg.js` such as the following:

```javascript
var config = require('12factor-config');

var cfg = config({
    // ... environment config here ...
});

module.exports = cfg;
```

By doing this, all other files in your application can just `require('lib/cfg.js')` and obtain
the exact same configuration.

## Author ##

Written by [Andrew Chilton](http://chilts.org/) - [Blog](http://chilts.org/) -
[Twitter](https://twitter.com/andychilton).

## License ##

MIT - http://chilts.mit-license.org/2013/

(Ends)
