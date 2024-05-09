### Create the folder
```
mkdir /cluster/tufts/hpc/tools/monitor
cd monitor
mkdir 2.3.1
cd 2.3.1
```

### Create conda env
```
/cluster/tufts/hpc/tools/anaconda/202111/bin/conda create -p libexec python=3.10
```

### Install monitor
```
libexec/bin/pip install resource-monitor
```

### Create softlink
```
mkdir bin
cd bin
ln -sf ../libexec/bin/monitor .
```