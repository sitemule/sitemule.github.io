## ILEusion installation

There are two ways to install ILEusion for IBM i.

#### Install via SAVF

1. Download the latest release savefile from the [list of releases on GitHub](https://github.com/sitemule/ILEusion/releases/).
2. Upload the savefile to the IFS on the IBM i you'd like to install it on.
3. To install the release, run:
   1. `CRTLIB ILEUSION`
   2. `CPYFRMSTMF FROMSTMF('./release/release.savf') TOMBR('/QSYS.lib/ILEUSION.lib/RELEASE.FILE') MBROPT(*REPLACE) CVTDTA(*NONE)`
   3. `RSTLIB SAVLIB(ILEUSION) DEV(*SAVF) SAVF(ILEUSION/RELEASE)`

#### Build from source

ILEusion depends on ILEastic and noxDB, so you will need to follow their install guides first:

* [Installing ILEastic](/ileastic/about)
* [Installing noxDB](/noxdb/installation)

Then, follow these steps from a pase shell (over SSH, for example):

```
git clone https://github.com/sitemule/ILEusion.git
cd noxDB
gmake
```
