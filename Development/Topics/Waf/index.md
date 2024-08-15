# Waf Build System

The Waf build system is used to build Pebble apps.

## Using Tools

### Loading Tools

```py
ctx.load('<TOOL NAME>', tooldir='waftools')
```

## Debugging Tips



### Using Zones

Waf includes multiple "zones" of debug output. One of the most useful zones is `deps`, which manages dependencies.

```bash
pebble build -- --zones=deps
```

#### Available Zones

| Zone     | Description                                                                  |
| -------- | ---------------------------------------------------------------------------- |
| runner   | command-lines executed (enabled when -v is provided without debugging zones) |
| deps     | implicit dependencies found (task scanners)                                  |
| task_gen | task creation (from task generators) and task generator method execution     |
| action   | functions to execute for building the targets                                |
| env      | environment contents                                                         |
| envhash  | hashes of the environment objects - helps seeing what changes                |
| build    | build context operations such as filesystem access                           |
| preproc  | preprocessor execution                                                       |
| group    | groups and task generators                                                   |

## Resources

### Guides

* `waf`, `wscript` & Your Pebble App
    * [Watch on YouTube](https://youtu.be/sjVnhEP94vM)
    * [Slideshow](http://www.slideshare.net/pebbledev/pdr15-waf-wscript-and-your-pebble-app)
    * [Examples from the presentation](https://github.com/pebble/pebble-waf-tools)
* Pebble uses Waf 1.7.11
* API documentation
    * [Waf 1.8 API documentation](https://web.archive.org/web/20150706045642if_/https://waf.io/apidocs/index.html)
    * [Current API documentation](https://waf.io/apidocs/)
    * [`ctx` (current)](https://waf.io/apidocs/Build.html#waflib.Build.BuildContext)
        * [`ctx.path`](https://waf.io/apidocs/Node.html)
        * [`ctx.env`](https://waf.io/apidocs/ConfigSet.html)
* [Waf 1.8 book](https://web.archive.org/web/20150706045556if_/https://waf.io/book/)
* [Current Waf book](https://waf.io/book/)