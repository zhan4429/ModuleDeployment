## Installation
```
mkdir 2.4.0
cd 2.4.0/
/apps/rcac/anaconda/2022.10/bin/conda create -p libexec python=3.10
libexec/bin/pip install hyper-shell
mkdir bin
cd bin/
ln -sf ../libexec/bin/hyper-shell .
```


## modulefile
```
local appname = 'hyper-shell'
local version = '2.4.0'
local modroot = '/apps/external/apps/hyper-shell/' .. version

whatis('Name: ' .. appname)
whatis('Description: Process shell commands over a distributed, asynchronous queue.')
whatis('Version: ' .. version)
whatis('Keywords: distributed-computing command-line-tool shell-scripting')
whatis('URL: https://github.com/glentner/hyper-shell')

-- XALT breaks argparse.py
-- PYTHONPATH has to be purged
if isloaded('xalt')
then
        unload('xalt')
        LmodMessage('lmod: xalt was automatically unloaded')
end

prepend_path('PATH', pathJoin(modroot, 'bin'))
prepend_path('MANPATH', pathJoin(modroot, 'share', 'man'))
```