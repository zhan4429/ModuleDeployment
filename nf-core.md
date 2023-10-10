Notes for my colleague Geoffrey Lentner from Purdue RCAC.
> I always recommend containing the environment in an underlying libexec directory, and creating a higher level bin folder with a  symlink, ns-core -> ../libexec/bin/ns-core.  This is really important because otherwise, you'll be exposing people who load the module to the Python used by ns-core which could interfere with the tasks they are trying to run.

> Also, if you look at /apps/rcac/anaconda I have a dedicated base environment to use for our apps that isn't tied to the user-facing anaconda module (so we can update things, do as we please).

## Create the space
```
$ mkdir -p /apps/external/apps/nf-core/2.7.2
$ cd /apps/external/apps/nf-core/2.7.2
```
## Create the virtual environment
```
$ /apps/rcac/anaconda/2022.10/bin/conda create -p libexec python=3.10
$ libexec/bin/pip install nf-core
```
## Expose the executable
```
$ mkdir bin
$ cd bin
$ ln -sf ../libexec/bin/nf-core
```

## Structure of directories
```
$ find /apps/external/apps/nf-core/2.7.2/ -maxdepth 2
/apps/external/apps/nf-core/2.7.2/
/apps/external/apps/nf-core/2.7.2/bin
/apps/external/apps/nf-core/2.7.2/bin/nf-core
/apps/external/apps/nf-core/2.7.2/libexec
/apps/external/apps/nf-core/2.7.2/libexec/include
/apps/external/apps/nf-core/2.7.2/libexec/ssl
/apps/external/apps/nf-core/2.7.2/libexec/share
/apps/external/apps/nf-core/2.7.2/libexec/x86_64-conda_cos7-linux-gnu
/apps/external/apps/nf-core/2.7.2/libexec/man
/apps/external/apps/nf-core/2.7.2/libexec/x86_64-conda-linux-gnu
/apps/external/apps/nf-core/2.7.2/libexec/conda-meta
/apps/external/apps/nf-core/2.7.2/libexec/bin
/apps/external/apps/nf-core/2.7.2/libexec/compiler_compat
/apps/external/apps/nf-core/2.7.2/libexec/lib
```

## Modulefile in lua
```
local version = '2.7.2'
local modroot = pathJoin('/apps/external/apps/nf-core', version)
depends_on("nextflow")
whatis('Name: nf-core')
whatis('Version: ' .. version)
whatis('Description: A community effort to collect a curated set of analysis pipelines built using Nextflow and tools to run the pipelines.')

prepend_path('PATH', pathJoin(modroot, 'bin'))
```

