//Package Managers

Debian/Ubuntu /etc/apt/sources.list

apt-get

apt-get update // update package cache
apt-get cache search nginx // looking for nging package available
apt-get install nginx // install nginx package
apt-get remove nginx // remove nginx package
apt-get remove --purge nginx // purge any configuration files
apt-get autoremove // remove any useless package dependencies
apt-get autoremove apache2 // remove dependencies for apache2 package
apt-get upgrade // upgrade all packages in the system
apt-get dist-upgrade // update system distribution

dpkg - for installing *.deb files

dpkg -i *.deb // install package - error dependencies could occur

Trick how to resolve error dependencies:
apt-get update // update cache
apt-get -f upgrade // fix flag, install what's dpkg required

dpkg -i *.deb // now it should be installed succesfull
dpkg --get-selections // show all installed packages
dpkg -remove *.deb // remove
dpkg --purge *.deb // remove all configuration files


RedHat/CentOS7 /etc/yum.repos.d/*

yum

yum remove nginx
yum update // update repositiry
yum search httpd // looking for httpd package
yum check-update httpd // checking if there's any updates for package
yum upgrade // updating all packages
yum deplist httpd // see dependency list
yum clean packages // delete instalation files, remove all temporary files
yum makecache // refresh yum cache
yum clean all // clean all cache

rpm

rpm -ihv *.rpm // i - install, h - show progress, v - verbose
rpm -q nano // query if nano installed
rpm -qi nano // give more information about nano package 
rpm -e // remove package
rpm -qR nano // list required packages for nano




Check the package location
which package_name
whereis package_name - finding binary, source and man files

Basic Shell:

echo $SHELL

List:

ls - list files
ls -a // list all files, even hidden
ls -l // list file with details
ls -p // appended slash for folders
ls -R // list recursive
ls * // list similar to recursively



mkdir // create directory
cd // change directory
touch // create file

clear // clear screen
halt // shutdown os
reboot // restart
shutdown // halt, poweroff, reboot, - c - cancel a pending shutdown
exit // closing current shell session, terminating current running process
su username // switch user, super user 
su - // become root user
env // print environment variables
top // shows spplication and processes running on the system
netstat // shows the status of the network
ifconfig // shows the network configurations that are set on out network devices
ip addr // shows breakdown of all of the network ip addresses
which // shows the location of application
whoami // shows the current user logged in
route // view out routing table
eval // run the arguments as a command in the current shell

EXEC 

What exec cmd does, is exactly the same as just running cmd, except that the current shell is replaced with the command, instead of a separate process being run. Internally, running say /bin/ls will call fork() to create a child process, and then exec() in the child to execute /bin/ls. exec /bin/ls on the other hand will not fork, but just replaces the shell.

Compare:

$ bash -c 'echo $$ ; ls -l /proc/self ; echo foo'
7218
lrwxrwxrwx 1 root root 0 Jun 30 16:49 /proc/self -> 7219
foo
with

$ bash -c 'echo $$ ; exec ls -l /proc/self ; echo foo'
7217
lrwxrwxrwx 1 root root 0 Jun 30 16:49 /proc/self -> 7217
echo $$ prints the PID of the shell I started, and listing /proc/self gives us the PID of the ls that was ran from the shell. Usually, the process IDs are different, but with exec the shell and ls have the same process ID. Also, the command following exec didn't run, since the shell was replaced

GPASSWD

gpasswd is used to administer the /etc/group file (and /etc/gshadow file if compiled with SHADOWGRP defined). Every group can have administrators, members and a password. System administrator can use -A option to define group administrator(s) and -M option to define members and has all rights of group administrators and members.

gpasswd -a tracy hrd // Add user (tracy) to the group (hrd)
gpasswd -d rakesh sqa // Remove user (rakesh) from group (sqa)
gpasswd -A joy managers // Set user (joy) and group administrator for (managers)
gpasswd managers // Set password for group manages

-A option to define group administrator(s)
-M option to define members

Uname:

uname // retun information what kind of operating system
uname -s // display linux kernal name
uname -n // display hostname
uname -r // display linux kernel release number
uname -v // version number
uname -m // dispaly hardware architecture
uname -p // display processor type
uname -i // display hardware platform
uanem -o // display operating system
uname -a // all information in one line

Tar:

tar -zxvf *.tar.gz // unpack

tar -cvf myarchive.tar mydirectory/ //create simple tar archive
tar -cvzf file.tar.gz inputfile1 inputfile2 // create compressed archive

cd // change directory
pwd // print working directory

.bash_logout // user specific shell preferences, only call when you are logging out
.bash_profile // login shell, user specific shell preferences
.bashrc // non login shell , user specific functions and aliases
/etc/profile // settings for every single users

Env variables:
PATH=$PATH:/Downloads/something/
Variables is set just for one session

Export make variable available for all next login shells:
export PATH // it will be cear after system reboot

User defined variables:
variable=value
varaiable1="stefan" // variable can not begin with the number


Devices:

- /dev/full // virtual file/device for unix similar OS, it returns "No space left on device (symbol ENOSPC)" when there is attempt to write out something on that device. When reading it providing zeros data, similar to /dev/zero. It is used for testing behaviour of programs once there is no space left on disk

- /dev/null // virtual file/device for unix similar OS, it removes all data that are forwarded to and does not generate data for process that reading from. It is used for remove data that you do not want that comming from another processes or it is treat like source of the empty input data.

- /dev/zero // virtual file/device for unix similar OS, it provides null characters - 0, NULL sign. Most often use case is providing NULL sign to overwrite data. It is used to create empty files with specified size. Unlike /dev/null, which is used for remove data, /dev/zero is a source of empty data. Example: Creating zero file with 100kB size specified: dd if=/dev/zero of=przyklad bs=1k count=100

- /dev/random // virtual file/device for unix similar OS, it generates random numbers, it based on hardware drivers and other sources. Once reaidng from /dev/random random bytes will be generated. /dev/random is required when random numbers are crusial and when it is hard to predict them, for example to create cryptographic keys. /dev/random gets data from buffer. Once all data is read , reading thread stop reading from that buffor. It can be easily verify by running: cat /dev/random. /dev/random can not be used for generate random numbers in short period of time - /dev/urandom is used for that.

- /dev/urandom // generate large amount of numbers, but they are less random. It uses hash functions. Most of the cryptographic tools like (np. OpenSSL, PGP i GnuPG) uses their own random generation mechanism but they are get "seed" from /dev/random.

TCPDUMP:
tcpdump -i eth1 // monitor all packets on eth1 interface
tcpdump -i eth1 'port 80' // monitor all traffic on port 80 ( HTTP )
tcpdump -vv -x -X -s 1500 -i eth1 'port 25' // monitor all traffic on port 25 ( SMTP )

-s 1500 // snarf snaplen bytes of data from each packet rather than the default of 68.
-x // when parsing and printing, in addition to printing the headers of each packet, print the data of each packet.
-X // when parsing and printing, in addition to printing the headers of each packet, print the data of each packet (minus its link level header) in hex and ASCII. This is very handy for analysing new protocols.

Docker:
docker images -a // list all images
docker rmi Image // remove image
docker images -f dangling=true // list danling images
docker rmi $(docker images -f dangling=true -q) // remove danling images
docker rmi $(docker images -a -q) // remove all images

docker ps -a // list containers
docker rm ID_or_Name ID_or_Name // remove container
docker ps -a -f status=exited // list all exited containers
docker rm $(docker ps -a -f status=exited -q) // remove all exited containers
docker ps -a -f status=exited -f status=created // list containers with more than one filer
docker rm $(docker ps -a -f status=exited -f status=created -q) // remove containers with more than one filer
docker stop $(docker ps -a -q) // stop all containers
docker rm $(docker ps -a -q) // remove all containers
docker volume ls // list volumes and location
docker volume rm volume_name // remove volumes
docker volume ls -f dangling=true // list dangling volumes
docker volume rm $(docker volume ls -f dangling=true -q) // remove dangling volumes

How to get bash or ssh into a running container in background mode?
docker attach container_id
If we use attach we can use only one instance of shell. So if we want open new terminal with new instance of container's shell, we just need run the following:
sudo docker exec -i -t container_id /bin/bash ( or just bash )
sudo docker exec -it -u root <<container_id_z_docker_ps>> bash

docker exec -it -u root <<container_id_z_docker_ps>> bash


Terraform:
terraform plan -destroy -target=aws_instance.jump -target=aws_instance.build

terraform plan -destroy -target=module.lambda.aws_lambda_function.daily_rds_snapshot -target=aws_instance.build

Terraform with exclude:
terraform plan $(for r in `terraform state list | fgrep -v module.network.aws_route_table.mgmt_rt` ; do printf "-target ${r} "; done)

Ansible:
ansible-playbook foo.yml --check --diff --limit foo.example.com
ansible-playbook example.yml --tags "configuration,packages"

ansible-playbook example.yml --check --diff --limit foo.example.com --tags "configuration,packages"
ansible-playbook site.yml --check --diff --limit build.surecert.internal --tags "backup_scripts"
ansible-playbook -i inventories/mgmt/ec2.py ./playbooks/site.yml --private-key=/tmp/surecert --check --diff --limit build.surecert.internal --tags "backup_scripts"


ansible-playbook -i inventories/mgmt/ec2.py ./playbooks/site.yml --private-key=/tmp/surecert
ansible-playbook -i inventories/mgmt/ec2.py ./playbooks/jenkins_restore.yml --private-key=/tmp/surecert --limit build.surecert.internal


Lufthansa:
ansible-playbook -i inventories/dev/dev_mgmt ./playbooks/site.yml --limit build --tags "jenkins" --check --diff

OpemSSL:


openssl genrsa -out ./skeyos.com.key 4096 // create rsa key
openssl genrsa -des3 -out server.key 1024 // create rsa key with des3 encryption (passphrase)
openssl rsa -in server.key.org -out server.key // remove encryption (passphrase)
openssl rsa -in ./skeyos.com.key -pubout > ./skeyos.com.key.pub
openssl req -new -sha256 -key ./skeyos.com.key -out ./skeyos.csom.key.csr // create sha256 CSR
openssl req -new -key server.key -out server.csr // create CSR
openssl x509 -req -days 365 -in server.csr -signkey server.key -out server.crt // generate self-signed certificate
openssl req -in ./skeyos.com.key.csr -noout -text // see the content of the certificate

openssl x509 -in skeyos.com.chained.crt -text // print certificate 

cat mycert.crt intermediate.cert.pem > skeyos.com.chained.crt


PEM format - If they begin with -----BEGIN and you can read them in a text editor (they use base64, which is readable in ASCII, not binary format), they are in PEM format.

sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout ./logstash.key -out ./logstash.crt

Rsync:
sudo rsync -az /vagrant/ /surecert/

AWSCLI:
aws iam get-instance-profile --instance-profile-name eb_inst_profile_demo
aws iam delete-instance-profile --instance-profile-name eb_inst_profile_demo

WC:
wc -l / count number of lines


DU:
du -sh ./* / check how many space is busy per specific catalogs

Detail description of redirection operator in Unix/Linux: (https://stackoverflow.com/questions/6674327/redirect-all-output-to-file)

Use this - "require command here" > log_file_name 2>&1

> file redirects stdout to file
1> file redirects stdout to file
2> file redirects stderr to file
&> file redirects stdout and stderr to file

That part is written to stderr, use 2> to redirect it. For example:

foo > stdout.txt 2> stderr.txt

or if you want in same file:

foo > allout.txt 2>&1

----------
*command* > file 2>&1 

If you want to silence the error, do:

*command* 2> /dev/null
----------


Command:

foo >> output.txt 2>&1

appends to the output.txt file, without replacing the content.


GREP, EGREP, FGREP:

Here are some example scenarios:

    You have a file with a list of say ten Unix usernames in plain text. You want to search the group file on your machine to see if any of the ten users listed are in any special groups:

    grep -F -f user_list.txt /etc/group

    The reason the -F switch helps here is that the usernames in your pattern file are interpreted as plain text strings. Dots for example would be interpreted as dots rather than wild-cards.

    You want to search using a fancy expression. For example parenthesis () can be used to indicate groups with | used as an OR operator. You could run this search using -E:

    grep -E '^no(fork|group)' /etc/group

    ...to return lines that start with either "nofork" or "nogroup". Without the -E switch you would have to escape the special characters involved because with normal pattern matching they would just search for that exact pattern;

    grep '^no\(fork\|group\)' /etc/group



https://unix.stackexchange.com/questions/17949/what-is-the-difference-between-grep-egrep-and-fgrep
