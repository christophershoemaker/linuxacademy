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
yum repolist // list all repositories
yum --disablerepo="*" --enablerepo="google" list available // list available packages in google repo

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

https://unix.stackexchange.com/questions/103114/what-do-the-fields-in-ls-al-output-mean

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

SSH:
ssh-copy-id // install your public key in a remote machine's authorized_keys - please test it how it works


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

ansible-playbook -i inventories/prod/prod_mgmt ./playbooks/site.yml --limit proxy_tmp --check --diff

ansible-playbook -i inventories/prod/prod_mgmt ./playbooks/site.yml --limit proxy_tmp --check --diff --tags "proxy_tmp"
ansible-playbook -i inventories/dev/dev_mgmt ./playbooks/site.yml --limit proxy_temporary --check --diff --tags "proxytemporary"

Lufthansa:
ansible-playbook -i inventories/dev/dev_mgmt ./playbooks/site.yml --limit build --tags "jenkins" --check --diff

Kibana and elasticsearch:
https://kibana.mroapps.com/_cat/indices?v // show indices
curl -XDELETE 'localhost:9200/twitter?pretty' // The above example deletes an index called twitter



OpemSSL:

https://serverfault.com/questions/9708/what-is-a-pem-file-and-how-does-it-differ-from-other-openssl-generated-key-file // type of certs

openssl genrsa -out ./skeyos.com.key 4096 // create rsa key
openssl genrsa -des3 -out server.key 1024 // create rsa key with des3 encryption (passphrase)
openssl rsa -in server.key.org -out server.key // remove encryption (passphrase)
openssl rsa -in ./skeyos.com.key -pubout > ./skeyos.com.key.pub
openssl req -new -sha256 -key ./skeyos.com.key -out ./skeyos.csom.key.csr // create sha256 CSR
openssl req -new -key server.key -out server.csr // create CSR
openssl x509 -req -days 365 -in server.csr -signkey server.key -out server.crt // generate self-signed certificate
openssl req -in ./skeyos.com.key.csr -noout -text // see the content of the certificate

openssl x509 -in skeyos.com.chained.crt -text // print certificate 
openssl x509 -in acs.qacafe.com.pem -text // print certificate in pem format
openssl x509 -in MYCERT.der -inform der -text // print certificate in der format

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

SYSTEMCTL: 
systemctl list-units //This will show any unit
systemctl list-units --all --state=inactive
systemctl list-unit-files
systemctl list-dependencies sshd.service
systemctl show sshd.service
systemctl edit nginx.service

DU:
du -sh ./* / check how many space is busy per specific catalogs

CRON: https://www.pantz.org/software/cron/croninfo.html
crontab -u userName -l // list all crons for username

Location for cron files:
/etc/crontab
/etc/cron.daily/
/etc/cron.hourly/
/etc/cron.weekly/
/etc/cron.monthly/
/etc/cron.d/

SLEEP:

To sleep for 5 seconds
sleep 5

To sleep for 2 minutes
sleep 2m

To sleep for 3 hours
sleep 3h

To sleep for 5 days
sleep 5d

To sleep for 1.5 seconds:
sleep 1.5

To sleep for .5 seconds:
sleep .5

REDIRESTION OUTPUT, ERRORS etc:

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


watch

less

more

tail -f

modprobe
depmod // https://www.lifewire.com/depmod-linux-command-4095356

tun/tap - network interfaces

locate - https://www.computerhope.com/unix/ulocate.htm

LDAP:

String  X.500 AttributeType
------------------------------
CN      commonName
L       localityName
ST      stateOrProvinceName
O       organizationName
OU      organizationalUnitName
C       countryName
STREET  streetAddress
DC      domainComponent
UID     userid

DN  Distinguish Name

##################################################################################################################
MAC - CLOSED CARDS:

Cgroups:
https://en.wikipedia.org/wiki/Cgroups

Virtual interfaces capture traffic: https://unix.stackexchange.com/questions/122468/how-does-one-capture-traffic-on-virtual-interfaces

Type command: https://superuser.com/questions/256651/how-to-echo-contents-of-file-in-a-dos-windows-command-prompt

Auditd: http://security.blogoverflow.com/2013/01/a-brief-introduction-to-auditd/

Creating users groups: https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-14-04?refcode=c8c27c60df3b

HoneyPOT:
www.eioba.pl/a/1ike/honey-pot-pulapka-na-hakera
https://zeltser.com/modern-honey-network-experiments/
https://github.com/threatstream/mhn

Linux runlevel: https://en.wikipedia.org/wiki/Runlevel

DDOS: https://sekurak.pl/czym-jest-atak-ddos-cz-2-techniki-i-narzedzia/

GRE: http://searchenterprisewan.techtarget.com/definition/Generic-routing-encapsulation-GRE

Netstat: https://spece.it/windows-serwer-artykuly/netstat-sprawdzanie-portow-oraz-statystyki-interfejsow

Linux nohup tool:
https://pl.wikipedia.org/wiki/Nohup

Best AWS practises:
https://github.com/hashicorp/best-practices/blob/master/terraform/providers/aws/README.md 

Build Redundant IP Routing:
https://msdn.microsoft.com/en-us/library/ms995864.aspx

Trafic conrol and shaping and queues: 
http://www.razorsedge.org/~mike/software/traffic
https://serverfault.com/questions/350023/tc-ingress-policing-and-ifb-mirroring
http://lartc.org/howto/lartc.qdisc.filters.html
https://wiki.archlinux.org/index.php/Advanced_traffic_control
https://wiki.linuxfoundation.org/networking/iproute2_examples
https://unix.stackexchange.com/questions/96804/tc-show-output-explanation
https://serverfault.com/questions/362452/deleting-filters-in-tc
https://www.cyberciti.biz/faq/linux-traffic-shaping-using-tc-to-control-http-traffic/
https://stackoverflow.com/questions/9513981/rtnetlink-answers-no-such-file-or-directory-error
http://www.fyzix.net/index.php?title=Traffic_shaping_with_tc_and_iptables
https://morfitronik.pl/konfiguracja-interfejsow-ifb-w-linuxie/
https://serverfault.com/questions/350023/tc-ingress-policing-and-ifb-mirroring
https://www.coverfire.com/articles/queueing-in-the-linux-network-stack/
http://www.linuxjournal.com/content/queueing-linux-network-stack

Output traffic on different interfaces based on destination port:
https://unix.stackexchange.com/questions/21093/output-traffic-on-different-interfaces-based-on-destination-port

How work with iptables: 
https://n0where.net/how-does-it-work-iptables/
https://serverfault.com/questions/467756/what-is-the-mangle-table-in-iptables
https://unix.stackexchange.com/questions/83120/iptables-how-to-allow-traffic-from-redirected-port
http://www.faqs.org/docs/iptables/traversingoftables.html
http://www.iptables.info/en/iptables-targets-and-jumps.html#ACCEPTTARGET
http://www.tldp.org/HOWTO/Adv-Routing-HOWTO/lartc.netfilter.html
http://pl.docs.pld-linux.org/siec_nat.html
https://home.regit.org/netfilter-en/netfilter-connmark/
https://www.cs.montana.edu/courses/spring2004/409/topics/nat/outline.html
https://stackoverflow.com/questions/17076936/iptables-remove-packet-mark-on-certain-packets
https://unix.stackexchange.com/questions/186636/forward-packets-from-one-interface-to-another-interface-using-iptables
https://serverfault.com/questions/370962/nginx-reverse-proxy-syn-flood
http://www.webhostingtalk.com/showthread.php?t=949912

Trainigs:

Securing linux servers:
https://www.pluralsight.com/courses/securing-linux-servers

Zero downtime haproxy: 
https://inside.unbounce.com/product-dev/haproxy-reloads/
https://serverfault.com/questions/580595/haproxy-graceful-reload-with-zero-packet-loss
https://engineeringblog.yelp.com/2015/04/true-zero-downtime-haproxy-reloads.html

Meditation:
https://www.headspace.com/

Pluralsight: 
https://www.pluralsight.com/courses/code-auditing-security-hackers-developers

DMatusiewicz: https://github.com/dmatusiewicz/terraform_layout/wiki/Infrastructure-as-a-code:-Terraform-journey

StartSSL and LetsEncrypt:
https://www.digitalocean.com/community/tutorials/how-to-set-up-apache-with-a-free-signed-ssl-certificate-on-a-vps

Email Server with spamassasin:
https://www.digitalocean.com/community/tutorials/how-to-configure-a-mail-server-using-postfix-dovecot-mysql-and-spamassassin
http://security.psnc.pl/files/szkolenia/KDM_071030.pdf
https://www.linode.com/docs/email/postfix/email-with-postfix-dovecot-and-mysql
http://trener-it.pl/strona/artykuly/17
http://ccm.net/contents/116-how-email-works-mta-mda-mua
https://ubuntu101.co.za/ssl/postfix-and-dovecot-on-ubuntu-with-a-lets-encrypt-ssl-certificate/
https://www.howtoforge.com/community/threads/running-smtp-on-multiple-ports-postfix.4788/
https://www.linode.com/docs/email/postfix/email-with-postfix-dovecot-and-mariadb-on-centos-7
https://support.linuxpl.com/Knowledgebase/Article/View/86/3/porty-dla-uslug-pocztowych-pop3-imap-smtp-oraz-ssl
https://superuser.com/questions/280165/how-do-i-change-postfix-port-from-25-to-587

Root squash:
https://en.wikipedia.org/wiki/Unix_security#Root_squash

Nginx high availibility with keepalived:
https://www.nginx.com/resources/admin-guide/nginx-ha-keepalived-nodes/

Certbot:
https://certbot.eff.org/#centosrhel7-nginx

Docker containers load balancig with nginx and consul templates:
https://tech.bellycard.com/blog/load-balancing-docker-containers-with-nginx-and-consul-template/
http://blog.hypriot.com/post/docker-compose-nodejs-haproxy/

Provisioning docker containers with ansible:
https://www.ibm.com/developerworks/cloud/library/cl-provision-docker-containers-ansible/

Plays and fun with english leanguage:
http://www.strefaangielskiego.pl/angielskie-kalambury/
https://www.gamestolearnenglish.com/
http://www.strefaangielskiego.pl/zagadki-slowne-lewisa-carrolla/

100 devops tools:
http://theremotelab.com/blog/updates_about_devops_world/

OpenGrok:
https://opengrok.github.io/OpenGrok/

Jokes:
http://www.jestpozytywnie.pl/13-prostych-zartow/?utm_medium=social&utm_campaign=postplanner&utm_source=facebook.com

Vege:
https://zdrowozakrecona.blogspot.co.uk/2016/06/pizza-z-kaszy-jaglanej-z-grillowanym.html

Interview questions:
https://interviewme.pl/blog/pytania-na-rozmowie-kwalifikacyjnej-jakie-warto-zadac

Netflix engineers give ssh access to developers:
https://www.oreilly.com/learning/how-netflix-gives-all-its-engineers-ssh-access

Regex check page:
https://regex101.com/r/yN4tJ6/84

JamWiFi:
https://github.com/unixpickle/JamWiFi

Variable of variable inansible:
http://techieroop.com/variable-of-variable-in-ansible-playbook/

Running ansible on localhost:
http://ansible.pickle.io/post/86598332429/running-ansible-playbook-in-localhost

VMWare Fusion keys:
http://appnee.com/vmware-fusionpro-78-all-versions-universal-license-keys/

XSS Beef:
https://sekurak.pl/do-czego-mozna-wykorzystac-xss-czyli-czym-jest-beef/

Angieslki:
http://www.t4tw.info/angielski/gramatyka/reported-speech.html
https://ell.stackexchange.com/questions/9449/is-would-accompanied-with-present-perfect
https://www.englishforums.com/English/WeSupposedStartWork/cjngh/post.htm
http://www.5minuteenglish.com/jul14.htm
https://www.ang.pl/gramatyka/strona-bierna/czasy-perfect
https://www.e-ang.pl/130,Przedimki-a-an-the.html
https://english.stackexchange.com/questions/171942/sprung-or-unsprung-trap/171943

Linux daemons init scripts:
https://www.linux.com/learn/managing-linux-daemons-init-scripts
https://coderwall.com/p/z9z3vw/start-stop-status-cycle-for-vert-x-program-or-any-other-pid-controlled-one
http://codingfreak.blogspot.com/2012/03/daemon-izing-process-in-linux.html
https://stackoverflow.com/questions/3095566/linux-daemonize
https://askubuntu.com/questions/2075/whats-the-difference-between-service-and-etc-init-d
https://groups.google.com/forum/#!topic/vault-tool/Epyb3p-habQ
https://unix.stackexchange.com/questions/177690/passing-variable-in-init-d-script

Linux capabilities:
http://man7.org/linux/man-pages/man7/capabilities.7.html
http://blog.siphos.be/2013/05/overview-of-linux-capabilities-part-2/

Ulimit:
https://www.networkworld.com/article/2693414/operating-systems/setting-limits-with-ulimit.html

GPG keys:
https://serverfault.com/questions/471412/gpg-gen-key-hangs-at-gaining-enough-entropy-on-centos-6
https://help.github.com/articles/generating-a-new-gpg-key/
https://gpgtools.tenderapp.com/discussions/problems/18784-using-gpgsm-from-homebrew
http://www.sysmic.org/dotclear/index.php?post/2010/03/24/Convert-keys-betweens-GnuPG%2C-OpenSsh-and-OpenSSL
https://gpgtools.tenderapp.com/discussions/problems/18999-feature-request-converting-ssh-rsa-keys-to-gpg-keys
http://www.sysmic.org/dotclear/index.php?post/2010/03/24/Convert-keys-betweens-GnuPG%2C-OpenSsh-and-OpenSSL
https://pl.wikipedia.org/wiki/Pretty_Good_Privacy
https://gpgtools.tenderapp.com/discussions/problems/18999-feature-request-converting-ssh-rsa-keys-to-gpg-keys

Continuuos Integration and Delivery:
https://app.pluralsight.com/player?course=continuous-delivery-automation-aws-certified-devops-engineer&author=mike-pfeiffer&name=continuous-delivery-automation-aws-certified-devops-engineer-m4&clip=2

DNS tools:
http://www.dnsstuff.com/tools

Linux abort function:
http://man7.org/linux/man-pages/man3/abort.3.html

Linux show processes:
https://www.cyberciti.biz/faq/show-all-running-processes-in-linux/

Music;
https://www.youtube.com/watch?v=tD4HCZe-tew
https://www.youtube.com/watch?v=GTyN-DB_v5M
https://www.youtube.com/watch?v=IcioUkiOMV0
https://www.youtube.com/watch?v=BQ0mxQXmLsk
https://www.youtube.com/watch?v=zdVcD3yzFw4&pbjreload=10
https://www.youtube.com/watch?v=IcioUkiOMV0
https://www.youtube.com/watch?v=lPtNXuORLS8
https://www.youtube.com/watch?v=yzWPwQzc2n8
https://www.youtube.com/watch?v=6slHFn7VNFU
https://www.youtube.com/watch?v=nGUSNxN7X-4
https://www.youtube.com/watch?v=1iNRiy6q8HQ

Linux change password:
https://www.cyberciti.biz/faq/linux-set-change-password-how-to/

Linux exit status:
http://www.tldp.org/LDP/abs/html/exit-status.html

Netstat and port listening:
https://serverfault.com/questions/725262/what-causes-the-connection-refused-message
https://stackoverflow.com/questions/4421633/who-is-listening-on-a-given-tcp-port-on-mac-os-x

Su and sudo:
https://unix.stackexchange.com/questions/7013/why-do-we-use-su-and-not-just-su

Encryption:
https://crypto.stackexchange.com/questions/26916/is-there-a-multiple-asymmetric-encryption-algorithm-which-requires-all-private

Tar and tar the specified files only:
https://stackoverflow.com/questions/18731603/how-to-tar-certain-file-types-in-all-subdirectories
https://www.cyberciti.biz/faq/how-do-i-compress-a-whole-linux-or-unix-directory/

Declarative and imperative configuration management tools:
https://www.upguard.com/blog/articles/declarative-vs.-imperative-models-for-configuration-management

KVM:
https://www.howtogeek.com/117635/how-to-install-kvm-and-create-virtual-machines-on-ubuntu/

Linux versin check commands:
https://www.symantec.com/connect/articles/commands-check-linux-version-release-name-kernel-version

Ssh-key and see if it is encrypted:
https://serverfault.com/questions/628921/how-do-i-know-if-pem-is-password-protected-using-ssh-keygen

Rsync:
https://www.digitalocean.com/community/tutorials/how-to-use-rsync-to-sync-local-and-remote-directories-on-a-vps

Configure network interfaces:
https://www.tecmint.com/ip-command-examples/

Powershell try catch:
https://www.vexasoft.com/blogs/powershell/7255220-powershell-tutorial-try-catch-finally-and-error-handling-in-powershell

Routes in Windows:
https://www.howtogeek.com/howto/windows/adding-a-tcpip-route-to-the-windows-routing-table/

Psychology and communication:
https://www.wikihow.com/Adapt-the-Way-You-Communicate-to-Different-Situations
https://businessinsider.com.pl/rozwoj-osobisty/kariera/jak-osiagnac-sukces-w-pracy/wf42v35

Jenkins echo off:
https://stackoverflow.com/questions/26797219/echo-off-in-jenkins-console-output

NFS failover:
https://serverfault.com/questions/566885/autofs-and-nfs-failover-can-autofs-remount

DNS failover:
https://www.digitalocean.com/community/tutorials/how-to-setup-dns-slave-auto-configuration-using-virtualmin-webmin-on-ubuntu

Managing secrets with vault:
https://www.amon.cx/blog/managing-all-secrets-with-vault/

