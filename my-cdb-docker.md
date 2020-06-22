
## steps 

```
docker image build -t drel6_cdb:1.0 .
docker container run -it -v /Users/pasqualespica/my_data/PAS7B/work/docker_cdb_machine/src/kit:/data drel6_cdb:1.0
```

### install 

cmake `cmake-2.8.12.2.tar.gz`
```bash
    ./bootstrap
    make
    make install
```

cppunit `cppunit-1.12.1.tar.gz`
```bash
    ./configure    
    make
    make install
```

## remove all containers
`docker container rm $(docker container ls -aq)`

## show all stopped containers
`docker container ls -q --filter "status=exited"`

## commands ...
```
>>> docker image build -t gcc_pasquale:4.0 .
>>> docker container run -v ~/container-data:/data gcc_pasquale:4.0
>>> docker container run -it gcc_pasquale:4.0 bash
```

```
docker image build -t cdb_docker:1.0 .
docker container run -it -v /Users/pasqualespica/my_data/PAS7B/work/docker_cdb_machine/src/kit:/data cdb_docker:1.0 bash
docker container exec -it <CONTAINER> bash
```

`docker container run -it cdb_docker:1.0 bash`


da `build` under 
```
cd /usr/src/docker_cdb/packCdb/build
./genCmakeTree.sh /usr/src/docker_cdb/fabbrica/ /usr/src/docker_cdb/fabbrica/pan1/
```

## folders..

### Docker-Mastery-Commands
`/Users/pasqualespica/Downloads/docker__`
### cdb dodcker kit
`/Users/pasqualespica/my_data/PAS7B/work/docker_cdb_machine/src/kit`
### cdb docker file
`/Users/pasqualespica/docker_tutorial/node-bulletin-board/my_folder`

>>>

```c++
#include "pan/MaraWrapper/MsgCdbItfSerialized.h"
#include "cdb_global.hxx"
#include "itf2clts/DfmInterface.hxx"
```

## cmake

## cppunit

## openldap

```c++
#include <>
lber.h
lber_types.h
ldap.h
ldap_cdefs.h
ldap_features.h
```


