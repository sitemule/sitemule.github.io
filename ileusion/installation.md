## ILEusion installation

There are two ways to install ILEusion for IBM i.

#### Build from source

ILEusion depends on ILEastic and noxDB, so you will need to follow their install guides first:

* [Installing ILEastic](/ileastic/about)
* [Installing noxDB](/noxdb/installation)

Then, follow these steps from a pase shell (over SSH, for example):

```
git clone git@github.com:sitemule/ileusion.git
cd noxDB
gmake
```

#### Install via SAVF

Save file installation instructions coming soon.