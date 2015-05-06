## Oracle Database 

It will download a minimal CentOS 6 image, Puppet 3.7 and all it dependencies

The Docker image will be big, and off course this is not supported by Oracle and like always check your license to use this software

Configures Puppet and use librarian-puppet to download all the modules from the Puppet Forge

Changes vs original :
- kit will be outside VM
- no db - only software. db will be created in a separat step.

### Result
- Oracle Database software on CentOS 6. ( no systemd ;) )

Optional, you can add your own DB things, just change the puppet site.pp manifest
- Add your own Tablespaces
- Add Roles
- User with grants
- Change database init parameters
- execute some SQL

### Software
Download Oracle DB for CentOS 6 64.
- 12.1.0.2 file 1 & 2 ( linuxamd64_12c_database_1of2.zip )

### Build image 
1. Clone git repo.
2. docker build -t myora .
This will create a image called "myora"
3. start image with ora kits provided as volume
4. install oracle software. save it as image

Maybe after the build you should compress it first, see the compress section for more info

### Start container
default, will start the listener & database server
- docker run -i -t -p 1521:1521 oracle/database12102:latest

with bash

docker run -i -t -p 1521:1521 oracle/database12102:latest /bin/bash
- /startup.sh

### Compress image (now ~7.6GB)
- ID=$(docker run -d oracle/database12102:latest /bin/bash)
- docker export $ID > database12102.tar
- cat database12102.tar | docker import - database12102
- docker run -i -t -p 1521:1521 database12102:latest /bin/bash
- /startup.sh
