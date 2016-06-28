## Oracle Database 12.1.0.2 with ASM on NFS


ASM is based on NFS

### Software ( 12.1.0.2 )
You need to create a directory software and put the next files into it:

- linuxamd64_12102_database_1of2.zip
- linuxamd64_12102_database_2of2.zip
- linuxamd64_12102_grid_1of2.zip
- linuxamd64_12102_grid_2of2.zip

You can download the files here:

- http://www.oracle.com/technetwork/database/enterprise-edition/downloads/database12c-linux-download-2240591.html
- https://updates.oracle.com/download/6880880.html

### Vagrant

Startup the box
- vagrant up dbasm

Login
- vagrant ssh dbasm

### Accounts
- root password vagrant
- vagrant password vagrant, has sudo rights
- grid password oracle
- oracle password oracle

