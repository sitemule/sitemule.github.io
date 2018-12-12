## ILEastic

[ILEastic](https://github.com/sitemule/ILEastic) is

* made for IBM i (7.2+)
* a web server (tah tah apache)
* router

It can

* Serve static or dynamic content
* Handle different HTTP methods
* Make extending your RPG easy-peasy

### Installation

**One**. Download

```
git clone git@github.com:sitemule/ILEastic.git
```

**Two**. Compile (using GNU Make, available via `yum`)

```
cd ILEastic
gmake
```

**Three**. Build & run an example (from 5250)

```
addlible ileastic
cd 'ilastic'
CRTBNDRPG PGM(DEMO2) SRCSTMF('./examples/demo2.rpgle')
SBMJOB CMD(CALL PGM(DEMO02)) JOB(ILEASTIC2) JOBQ(QSYSNOMAX) ALWMLTTHD(*YES) 
```

**Four**. Enjoy

### Pro tips

* Check out the [documentation](http://iledocs.rpgnextgen.com/)
* `il_getRequestResource(request)` to get the endpoint.
* `il_getRequestMethod(request)` to get the HTTP method.
* `il_getContent(request)` to get the request body.
* [noxDB](https://github.com/sitemule/noxDB) to parse and generate JSON.
* [apug](https://github.com/WorksOfLiam/apug) can be used as a templating engine.
