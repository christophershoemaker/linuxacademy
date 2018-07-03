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

Having *.deb file you can install it using apt-get install package_name. But first move your .deb file to /var/cache/apt/archives/ directory. After executing this command, it will automatically download its dependencies.

dpkg - for installing *.deb files

dpkg -i *.deb // install package - error dependencies could occur
dpkg-repack gparted // create deb package from already installed package
rpmrebuild <package_name>

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
yum whatprovides <package_name>

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
touch /var/log/temp.log && echo "DEBUG tescik obiad" >> /var/log/temp.log

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

CP, MV: // https://www.cyberciti.biz/faq/explain-brace-expansion-in-cp-mv-bash-shell-commands/
Explain: {,} in cp or mv
echo foo{1,2,3}.txt // Output: foo1.txt foo2.txt foo3.txt

Examples:
echo file.txt{,.bak}
echo file-{a..d}.txt
echo mkdir -p /apache-jail/{usr,bin,lib64,dev}
echo cp httpd.conf{,.backup}
echo mv delta.{txt,doc}

cp -v file1.txt{,.bak} // cp -v file1.txt file1.txt.bak

FIND:
find . -name '*.h'
find . -name '*.h' -o -name '*.cpp' -exec grep "CP_Image" {} \; -print

grep -r --include=\*.txt 'searchterm' ./ // case sensitive varsion
grep -r -i --include=\*.txt 'searchterm' ./ // case-insensitive version

EXEC:

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

Unzip:

unzip -Z1 file.zip // List files what's in
unzip /tmp/jpgc-casutg-2.1.zip -d /opt/apache-jmeter-3.3 // unzip to specific directory
unzip -o /tmp/jpgc-graphs-basic-2.0.zip -d /opt/apache-jmeter-3.3 // unzip with override options

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

Pidof:
pidof <program> -- find the process ID of a running program. 

Lsof:
lsof - list open files
-p s
    This option excludes or selects the listing of files for the processes whose optional process IDentification (PID) numbers are in the comma-separated set s - e.g., ''123'' or ''123,^456''. (There should be no spaces in the set.)
    
Tar:

tar -zxvf *.tar.gz // unpack

tar -cvf myarchive.tar mydirectory/ //create simple tar archive
tar -cvzf file.tar.gz inputfile1 inputfile2 // create compressed archive
tar -cvf nginx-jumphost-prd.tar /etc/nginx

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

tcpdump -nl -s 0 -A -i eth0 -c 500 'port 443' | tee output.file //output to file and stdout

sudo tcpdump -vv -i eth0 'port 443' | tee output.log / to redirect logs on consile and file

-s 1500 // snarf snaplen bytes of data from each packet rather than the default of 68.
-x // when parsing and printing, in addition to printing the headers of each packet, print the data of each packet.
-X // when parsing and printing, in addition to printing the headers of each packet, print the data of each packet (minus its link level header) in hex and ASCII. This is very handy for analysing new protocols.

TEE:
echo "hello" | sudo tee f.txt  # add -a for append (>>)

Docker:
docker images -a // list all images
docker rmi Image // remove image
docker images -f dangling=true // list danling images
docker rmi $(docker images -f dangling=true -q) // remove danling images
docker rmi $(docker images -a -q) // remove all images
sudo docker rmi -f $(sudo docker images -a -q) // force to remove images

docker ps -a // list containers // https://stackoverflow.com/questions/41122446/get-the-names-of-containers-in-docker
docker ps -a --format "{{.ID}}: {{.Name}}"
caee09882462: peaceful_saha
docker ps -a --format "table {{.ID}}\t{{.Names}}"
CONTAINER ID        NAMES
caee09882462        peaceful_saha


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
sudo docker exec -it -u root $( sudo docker ps --latest --format "{{.ID}}" ) bash

docker exec -it -u root <<container_id_z_docker_ps>> bash

docker logs --follow CONTAINER_ID


Build docker image:
docker build . // when you have specified docker image

Docker cp file:
docker cp <containerId>:/file/path/within/container /host/path/target

HISTORY:
history -c //clean history for current shell
rm ~/.bash_history // clear all history

WAIT:
wait // command which pauses until execution of a background process has ended.

SET:

Change the value of a shell option and set the positional parameters, or display the names and values of shell variables.

SHELL/BASH scripts:
================================================================================
Variables: // https://superuser.com/questions/247127/what-is-and-in-linux/247131
--------------------------------------------------------------------------------------------
$? - The return value is stored in. 0 indicates success, others indicates error.
$@ - print all arguments submitted

$#    Stores the number of command-line arguments that 
      were passed to the shell program.
$?    Stores the exit value of the last command that was 
      executed.
$0    Stores the first word of the entered command (the 
      name of the shell program).
$*    Stores all the arguments that were entered on the
      command line ($1 $2 ...).
"$@"  Stores all the arguments that were entered
      on the command line, individually quoted ("$1" "$2" ...).

So basically, $# is a number of arguments given when your script was executed. $* is a string containing all arguments. For example, $1 is the first argument and so on. This is useful, if you want to access a specific argument in your script.

As Brian commented, here is a simple example. If you run following command:

./command -yes -no /home/username

    $# = 3
    $* = -yes -no /home/username
    $@ = array: {"-yes", "-no", "/home/username"}
    $0 = ./command, $1 = -yes etc.

$@:
    Expands to the positional parameters, starting from one. When the expansion occurs within double-quotes, and where field splitting (see Field Splitting) is performed, each positional parameter shall expand as a separate field, with the provision that the expansion of the first parameter shall still be joined with the beginning part of the original word (assuming that the expanded parameter was embedded within a word), and the expansion of the last parameter shall still be joined with the last part of the original word. If there are no positional parameters, the expansion of '@' shall generate zero fields, even when '@' is double-quoted.
$*:
    Expands to the positional parameters, starting from one. When the expansion occurs within a double-quoted string (see Double-Quotes), it shall expand to a single field with the value of each parameter separated by the first character of the IFS variable, or by a if IFS is unset. If IFS is set to a null string, this is not equivalent to unsetting it; its first character does not exist, so the parameter values are concatenated.
$#:
    Expands to the decimal number of positional parameters. The command name (parameter 0) shall not be counted in the number given by '#' because it is a special parameter, not a positional parameter.
$?:
    Expands to the decimal exit status of the most recent pipeline (see Pipelines).
$-:
    (Hyphen.) Expands to the current option flags (the single-letter option names concatenated into a string) as specified on invocation, by the set special built-in command, or implicitly by the shell.
$$:
    Expands to the decimal process ID of the invoked shell. In a subshell (see Shell Execution Environment ), '$' shall expand to the same value as that of the current shell.
$!:
    Expands to the decimal process ID of the most recent background command (see Lists) executed from the current shell. (For example, background commands executed from subshells do not affect the value of "$!" in the current shell environment.) For a pipeline, the process ID is that of the last command in the pipeline.
$0:
    (Zero.) Expands to the name of the shell or shell script. See sh for a detailed description of how this name is derived.

$_:
    most recent parameter (or the abs path of the command to start the current shell immediately after startup).
--------------------------------------------------------------------------------------------

# Declare Array
ARRAY[INDEXNR]=value // The INDEXNR is treated as an arithmetic expression that must evaluate to a positive number
declare -a ARRAYNAME // Explicit declaration of an array is done using the declare built-in
ARRAY=(value1 value2 ... valueN) // Array variables may also be created using compound assignments
ARRAYNAME[indexnumber]=value // Adding missing or extra members in an array is done using the syntax

# Dereferencing the variables in an array
ARRAY=(one two three)
echo ${ARRAY[*]}
one two three
echo $ARRAY[*]
one[*]
echo ${ARRAY[2]}
three
ARRAY[3]=four
echo ${ARRAY[*]}
one two three four

# Deleting array variable
unset ARRAY[1]
echo ${ARRAY[*]}
one three four
unset ARRAY
echo ${ARRAY[*]}
<--no output-->

args=("$@")  // store arguments in a special array
ELEMENTS=${#args[@]} // get number of elements

if [ ${#errors[@]} -eq 0 ]; then // Check if array is empty in Bash - https://serverfault.com/questions/477503/check-if-array-is-empty-in-bash
    echo "No errors, hooray"
else
    echo "Oops, something went wrong..."
fi

# For loop // echo each element in array
for (( i=0;i<$ELEMENTS;i++)); do 
    echo ${args[${i}]} 
done

for i in 1 2 3 4 5
do
   echo "Welcome $i times"
done

for i in {1..5}
do
   echo "Welcome $i times"
done

echo "Bash version ${BASH_VERSION}..."
for i in {0..10..2}
  do 
     echo "Welcome $i times"
 done

for i in ${farm_hosts[@]}; do
  su $login -c "scp $httpd_conf_new ${i}:${httpd_conf_path}"
  su $login -c "ssh $i sudo /usr/local/apache/bin/apachectl graceful"
done

# While loop
while (( "$#" )); do 
  echo $1 
  shift //shift 1
done

# If statement:

if [ $? != 0 ]; then
  echo "Error"
else
  echo "Succesfull"
fi

SHIFT:
--------------------------------------------------------------------------------------------
function join_by { local IFS="$1"; shift; echo "$*"; } // https://stackoverflow.com/questions/1527049/join-elements-of-an-array

It shifts the positional parameters (such as arguments passed to a bash script) to the left, putting each parameter in a lower position. // https://www.computerhope.com/unix/bash/shift.htm
--------------------------------------------------------------------------------------------

# Returning value from function - https://stackoverflow.com/questions/8742783/returning-value-from-called-function-in-a-shell-script

--------------------------------------------------------------------------------------------

A Bash function can't return a string directly like you want it to. You can do three things:

  Echo a string
  Return an exit status, which is a number, not a string
  Share a variable

This is also true for some other shells.

Here's how to do each of those options:

1. Echo strings

lockdir="somedir"
testlock(){
    retval=""
    if mkdir "$lockdir"
    then # Directory did not exist, but it was created successfully
         echo >&2 "successfully acquired lock: $lockdir"
         retval="true"
    else
         echo >&2 "cannot acquire lock, giving up on $lockdir"
         retval="false"
    fi
    echo "$retval"
}

retval=$( testlock )
if [ "$retval" == "true" ]
then
     echo "directory not created"
else
     echo "directory already created"
fi

2. Return exit status

lockdir="somedir"
testlock(){
    if mkdir "$lockdir"
    then # Directory did not exist, but was created successfully
         echo >&2 "successfully acquired lock: $lockdir"
         retval=0
    else
         echo >&2 "cannot acquire lock, giving up on $lockdir"
         retval=1
    fi
    return "$retval"
}

testlock
retval=$?
if [ "$retval" == 0 ]
then
     echo "directory not created"
else
     echo "directory already created"
fi

3. Share variable

lockdir="somedir"
retval=-1
testlock(){
    if mkdir "$lockdir"
    then # Directory did not exist, but it was created successfully
         echo >&2 "successfully acquired lock: $lockdir"
         retval=0
    else
         echo >&2 "cannot acquire lock, giving up on $lockdir"
         retval=1
    fi
}

testlock
if [ "$retval" == 0 ]
then
     echo "directory not created"
else
     echo "directory already created"
fi
--------------------------------------------------------------------------------------------

# Check if list contain a value // https://stackoverflow.com/questions/8063228/how-do-i-check-if-a-variable-exists-in-a-list-in-bash
--------------------------------------------------------------------------------------------
[[ $list =~ (^|[[:space:]])$x($|[[:space:]]) ]] && echo 'yes' || echo 'no'

or create a function:

contains() {
    [[ $1 =~ (^|[[:space:]])$2($|[[:space:]]) ]] && exit(0) || exit(1)
}

to use it:

contains aList anItem
echo $? # 0： match, 1: failed
--------------------------------------------------------------------------------------------

# 0 is a true, false is other than that // https://stackoverflow.com/questions/2933843/why-0-is-true-but-false-is-1-in-the-shell

Conventionally, a status of zero signifies success and non-zero failure. e.g. in c++, c, java languages.



There are two related issues here.

First, the OP's question, Why 0 is true but false is 1 in the shell? and the second, why do applications return 0 for success and non-zero for failure?

To answer the OP's question we need to understand the second question. The numerous answers to this post have described that this is a convention and have listed some of the niceties this convention affords. Some of these niceties are summarized below.

Why do applications return 0 for success and non-zero for failure?

Code that invokes an operation needs to know two things about the exit status of the operation. Did the operation exit successfully? [*1] And if the operation does not exit successfully why did the operation exit unsuccessfully? Any value could be used to denote success. But 0 is more convenient than any other number because it is portable between platforms. Summarizing xibo's answer to this question on 16 Aug 2011:

    Zero is encoding-independent.

    If we wanted to store one(1) in a 32-bit integer word, the first question would be "big-endian word or little-endian word?", followed by "how long are the bytes composing a little-endian word?", while zero will always look the same.

    Also it needs to be expected that some people cast errno to char or short at some point, or even to float. (int)((char)ENOLCK) is not ENOLCK when char is not at least 8-bit long (7-bit ASCII char machines are supported by UNIX), while (int)((char)0) is 0 independent of the architectural details of char.

Once it is determined that 0 will be the return value for success, then it makes sense to use any non-zero value for failure. This allows many exit codes to answer the question why the operation failed.

Why 0 is true but false is 1 in the shell?

One of the fundamental usages of shells is to automate processes by scripting them. Usually this means invoking an operation and then doing something else conditionally based on the exit status of the operation. Philippe A. explained nicely in his answer to this post that

    In bash and in unix shells in general, return values are not boolean. They are integer exit codes.

It's necessary then to interpret the exit status of these operations as a boolean value. It makes sense to map a successful (0) exit status to true and any non-zero/failure exit status to false. Doing this allows conditional execution of chained shell commands.

Here is an example mkdir deleteme && cd $_ && pwd. Because the shell interprets 0 as true this command conveniently works as expected. If the shell were to interpret 0 as false then you'd have to invert the interpreted exit status for each operation.

In short, it would be nonsensical for the shell to interpret 0 as false given the convention that applications return 0 for a successful exit status.

[*1]: Yes, many times operations need to return more than just a simple success message but that is beyond the scope of this thread.

See also Appendix E in the Advanced Bash-Scripting Guide
--------------------------------------------------------------------------------------------

# Add something to string // https://stackoverflow.com/questions/2250131/how-do-you-append-to-an-already-existing-string

In classic sh, you have to do something like:

s=test1
s="${s}test2"

(there are lots of variations on that theme, like s="$s""test2")

In bash, you can use +=:

s=test1
s+=test2
--------------------------------------------------------------------------------------------
 
# Assign the output of a Bash command to a variable

pwd=`pwd`
pwd=$(pwd)
--------------------------------------------------------------------------------------------
================================================================================

TEST: // https://ryanstutorials.net/bash-scripting-tutorial/bash-if-statements.php
================================================================================
The square brackets ( [ ] ) in the if statement above are actually a reference to the command test. This means that all of the operators that test allows may be used here as well. Look up the man page for test to see all of the possible operators (there are quite a few) but some of the more common ones are listed below.

Arguments:

The following arguments are used to construct this parameter:

-e FileName - FileName exists

All remaining arguments return true if the object (file or string) exists, and the condition specified is true.

-b Filename - Returns a True exit value if the specified FileName exists and is a block special file
-c FileName - FileName is a character special file
-d FileName - FileName is a directory

-f FileName - FileName is a regular file
-g FileName - FileName's Set Group ID bit is set
-h FileName - FileName is a symbolic link
-k FileName - FileName's sticky bit is set
-L FileName - FileName is a symbolic link
-p FileName - FileName is a named pipe (FIFO)
-r FileName - FileName is readable by the current process
-s FileName - FileName has a size greater than 0
-t FileDescriptor - FileDescriptor is open and associated with a terminal
-u FileName - FileName's Set User ID bit is set

-w FileName - FileName's write flag is on. However, the FileName will not be writable on a read-only file system even if test indicates true

-x FileName - FileName's execute flag is on
If the specified file exists and is a directory, the True exit value indicates that the current process has permission to change cd into the directory.

Non standard Korn Shell extensions:

file1 -nt file2 - file1 is newer than file2
file1 -ot file2 - file1 is older than file2
file1 -ef file2 - file1 is another name for file2 - (symbolic link or hard link)

String arguments:

In Perl, these sections are reversed: eq is a string operator and == is a numerical operator, and so on for the others.

-n String1 - the length of the String1 variable is nonzero
-z String1 - the length of the String1 variable is 0 (zero)
String1 = String2 - String1 and String2 variables are identical
String1 != String2 - String1 and String2 variables are not identical
String1 - true if String1 variable is not a null string

Number arguments:

Integer1 -eq Integer2 - Integer1 and Integer2 variables are algebraically equal
-ne - not equal
-gt - greater than
-ge - greater or equal 
-lt - less than
-le - less or equal

Operators:

test arguments can be combined with the following operators:

! - Unary negation operator
-a - Binary AND operator
-o - Binary OR operator (the -a operator has higher precedence than the -o operator)
\(Expression\) - Parentheses for grouping must be escaped with a backslash \

The -a and -o operators, along with parentheses for grouping, are XSI extensions[2] and are therefore not portable. In portable shell scripts, the same effect may be achieved by connecting multiple invocations of test together with the && and || operators and parentheses.
================================================================================

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
ansible-playbook -i inventories/mgmt/ec2.py ./playbooks/site.yml --check --diff --limit build.surecert.internal --tags "logstash"


ansible-playbook -i inventories/mgmt/ec2.py ./playbooks/site.yml --private-key=/tmp/surecert
ansible-playbook -i inventories/mgmt/ec2.py ./playbooks/jenkins_restore.yml --private-key=/tmp/surecert --limit build.surecert.internal

ansible-playbook -i inventories/prod/prod_mgmt ./playbooks/site.yml --limit proxy_tmp --check --diff

ansible-playbook -i inventories/prod/prod_mgmt ./playbooks/site.yml --limit proxy_tmp --check --diff --tags "proxy_tmp"
ansible-playbook -i inventories/dev/dev_mgmt ./playbooks/site.yml --limit proxy_temporary --check --diff --tags "proxytemporary"

Lufthansa:
ansible-playbook -i inventories/dev/dev_mgmt ./playbooks/site.yml --limit build --tags "jenkins" --check --diff
ansible-playbook -i inventories/prod/prod_mgmt ./playbooks/site.yml --limit jump --check --diff

echo 'test' | ansible-vault encrypt_string --stdin-name 'variables_name'
ansible-vault encrypt_string 'foobar' --name 'the_secret'

Kibana and elasticsearch:
https://kibana.testapps.com/_cat/indices?v // show indices
curl -XDELETE 'localhost:9200/twitter?pretty' // The above example deletes an index called twitter

https://search-elasticsearch-jmsjh54wvyfgiohplxd36jpkke.eu-central-1.es.amazonaws.com


https://kibana.testapps.com/_snapshot/snapshot-repository-DS // check settings of snapshot-repository-DS repository
curl -u kibana -XPOST 'https://kibana.testapps.com/_snapshot/snapshot-repository-DS/_verify' // verify the repository
curl -u kibana -XPUT 'https://kibana.testapps.com/_snapshot/snapshot-repository-DS/es_14_05_2018_full?wait_for_completion=false' // create snapshot named es_14_05_2018_full and wait until snapshot is compleated
curl -u kibana -XGET 'https://kibana.testapps.com/_snapshot/snapshot-repository-DS/es_14_05_2018_full' // create snapshot named es_14_05_2018_full
curl -u kibana -XGET 'https://kibana.testapps.com/_snapshot/snapshot-repository-DS/_current' // currently running snapshot
curl -XGET 'https://search-elasticsearch6-sjlzpveakwbwubookdp7hih724.eu-central-1.es.amazonaws.com/_cat/repositories?v' // list repositories
curl -XPOST 'https://search-elasticsearch6-sjlzpveakwbwubookdp7hih724.eu-central-1.es.amazonaws.com/_snapshot/snapshot-repository-DS/es_14_05_2018_full/_restore' // restore snapshot
curl -XGET 'https://search-elasticsearch6-sjlzpveakwbwubookdp7hih724.eu-central-1.es.amazonaws.com/_cat/snapshots/snapshot-repository-DS?v&s=id' // list snapshots

curl -XGET 'https://search-elasticsearch6-sjlzpveakwbwubookdp7hih724.eu-central-1.es.amazonaws.com/_snapshot/snapshot-repository-DS/es_14_05_2018_full/_status' // see restore status

curl -XGET 'https://search-elasticsearch6-sjlzpveakwbwubookdp7hih724.eu-central-1.es.amazonaws.com/_cat/recovery' | grep -v "done"

ssh -L 6543:search-elasticsearch-jmsjh54wvyfgiohplxd36jpkke.eu-central-1.es.amazonaws.com:443 jumphost-dev

ssh -L 6543:search-elasticsearch6-sjlzpveakwbwubookdp7hih724.eu-central-1.es.amazonaws.com:443 jumphost-dev
https://localhost:6543/_plugin/kibana/app/kibana






curl -k -XPUT -H "Content-Type: application/json" https://localhost:6543/.kibana/_settings -d '{
  "index.blocks.write": true
}'



curl -k -XPUT -H "Content-Type: application/json" https://localhost:6543/.kibana-6 -d '{
  "settings" : {
    "number_of_shards" : 1,
    "index.mapper.dynamic": false
  },
  "mappings" : {
    "doc": {
      "properties": {
        "type": {
          "type": "keyword"
        },
        "updated_at": {
          "type": "date"
        },
        "config": {
          "properties": {
            "buildNum": {
              "type": "keyword"
            }
          }
        },
        "index-pattern": {
          "properties": {
            "fieldFormatMap": {
              "type": "text"
            },
            "fields": {
              "type": "text"
            },
            "intervalName": {
              "type": "keyword"
            },
            "notExpandable": {
              "type": "boolean"
            },
            "sourceFilters": {
              "type": "text"
            },
            "timeFieldName": {
              "type": "keyword"
            },
            "title": {
              "type": "text"
            }
          }
        },
        "visualization": {
          "properties": {
            "description": {
              "type": "text"
            },
            "kibanaSavedObjectMeta": {
              "properties": {
                "searchSourceJSON": {
                  "type": "text"
                }
              }
            },
            "savedSearchId": {
              "type": "keyword"
            },
            "title": {
              "type": "text"
            },
            "uiStateJSON": {
              "type": "text"
            },
            "version": {
              "type": "integer"
            },
            "visState": {
              "type": "text"
            }
          }
        },
        "search": {
          "properties": {
            "columns": {
              "type": "keyword"
            },
            "description": {
              "type": "text"
            },
            "hits": {
              "type": "integer"
            },
            "kibanaSavedObjectMeta": {
              "properties": {
                "searchSourceJSON": {
                  "type": "text"
                }
              }
            },
            "sort": {
              "type": "keyword"
            },
            "title": {
              "type": "text"
            },
            "version": {
              "type": "integer"
            }
          }
        },
        "dashboard": {
          "properties": {
            "description": {
              "type": "text"
            },
            "hits": {
              "type": "integer"
            },
            "kibanaSavedObjectMeta": {
              "properties": {
                "searchSourceJSON": {
                  "type": "text"
                }
              }
            },
            "optionsJSON": {
              "type": "text"
            },
            "panelsJSON": {
              "type": "text"
            },
            "refreshInterval": {
              "properties": {
                "display": {
                  "type": "keyword"
                },
                "pause": {
                  "type": "boolean"
                },
                "section": {
                  "type": "integer"
                },
                "value": {
                  "type": "integer"
                }
              }
            },
            "timeFrom": {
              "type": "keyword"
            },
            "timeRestore": {
              "type": "boolean"
            },
            "timeTo": {
              "type": "keyword"
            },
            "title": {
              "type": "text"
            },
            "uiStateJSON": {
              "type": "text"
            },
            "version": {
              "type": "integer"
            }
          }
        },
        "url": {
          "properties": {
            "accessCount": {
              "type": "long"
            },
            "accessDate": {
              "type": "date"
            },
            "createDate": {
              "type": "date"
            },
            "url": {
              "type": "text",
              "fields": {
                "keyword": {
                  "type": "keyword",
                  "ignore_above": 2048
                }
              }
            }
          }
        },
        "server": {
          "properties": {
            "uuid": {
              "type": "keyword"
            }
          }
        },
        "timelion-sheet": {
          "properties": {
            "description": {
              "type": "text"
            },
            "hits": {
              "type": "integer"
            },
            "kibanaSavedObjectMeta": {
              "properties": {
                "searchSourceJSON": {
                  "type": "text"
                }
              }
            },
            "timelion_chart_height": {
              "type": "integer"
            },
            "timelion_columns": {
              "type": "integer"
            },
            "timelion_interval": {
              "type": "keyword"
            },
            "timelion_other_interval": {
              "type": "keyword"
            },
            "timelion_rows": {
              "type": "integer"
            },
            "timelion_sheet": {
              "type": "text"
            },
            "title": {
              "type": "text"
            },
            "version": {
              "type": "integer"
            }
          }
        },
        "graph-workspace": {
          "properties": {
            "description": {
              "type": "text"
            },
            "kibanaSavedObjectMeta": {
              "properties": {
                "searchSourceJSON": {
                  "type": "text"
                }
              }
            },
            "numLinks": {
              "type": "integer"
            },
            "numVertices": {
              "type": "integer"
            },
            "title": {
              "type": "text"
            },
            "version": {
              "type": "integer"
            },
            "wsState": {
              "type": "text"
            }
          }
        }
      }
    }
  }
}'


curl -k -XPOST -H "Content-Type: application/json" https://localhost:6543/_reindex -d '{
  "source": {
    "index": ".kibana"
  },
  "dest": {
    "index": ".kibana-6"
  },
  "script": {
    "inline": "ctx._source = [ ctx._type : ctx._source ]; ctx._source.type = ctx._type; ctx._id = ctx._type + \":\" + ctx._id; ctx._type = \"doc\"; ",
    "lang": "painless"
  }
}'

curl -k -XPOST -H "Content-Type: application/json" https://localhost:6543/_aliases -d '{
  "actions" : [
    { "add":  { "index": ".kibana-6", "alias": ".kibana" } },
    { "remove_index": { "index": ".kibana" } }
  ]
}'


Logging linux:
https://unix.stackexchange.com/questions/115839/change-sshd-logging-file-location-on-centos



Please post your sshd_config something else would seem to be up. A stock CentOS system always logs to /var/log/secure.
Example

$ sudo tail -f /var/log/secure
Feb 18 23:23:34 greeneggs sshd[3545]: pam_succeed_if(sshd:auth): requirement "uid >= 1000" not met by user "root"
Feb 18 23:23:36 greeneggs sshd[3545]: Failed password for root from ::1 port 46401 ssh2
Feb 18 23:23:42 greeneggs unix_chkpwd[3555]: password check failed for user (root)
Feb 18 23:23:42 greeneggs sshd[3545]: pam_succeed_if(sshd:auth): requirement "uid >= 1000" not met by user "root"
Feb 18 23:23:43 greeneggs sshd[3545]: Failed password for root from ::1 port 46401 ssh2
Feb 18 23:23:48 greeneggs sshd[3545]: Accepted password for root from ::1 port 46401 ssh2
Feb 18 23:23:48 greeneggs sshd[3545]: pam_unix(sshd:session): session opened for user root by (uid=0)
Feb 18 23:24:05 greeneggs sshd[3545]: Received disconnect from ::1: 11: disconnected by user
Feb 18 23:24:05 greeneggs sshd[3545]: pam_unix(sshd:session): session closed for user root
Feb 18 23:27:15 greeneggs sudo:     saml : TTY=pts/3 ; PWD=/home/saml ; USER=root ; COMMAND=/bin/tail /var/log/secure

This is controlled through /etc/ssh/sshd_config:

# Logging
# obsoletes QuietMode and FascistLogging
#SyslogFacility AUTH
SyslogFacility AUTHPRIV
#LogLevel INFO

As well as the contents of /etc/rsyslog.conf:

# Log anything (except mail) of level info or higher.
# Don't log private authentication messages!
*.info;mail.none;authpriv.none;cron.none                /var/log/messages

# The authpriv file has restricted access.
authpriv.*                                              /var/log/secure

Your issue

In one of your comments you mentioned that your rsyslogd config file was named /etc/rsyslog.config. That isn't the correct name for this file, and is likely the reason your logging is screwed up. Change the name of this file to /etc/rsyslog.conf and then restart the logging service.

sudo service rsyslog restart



What are the differences between “su”, “sudo -s”, “sudo -i”, “sudo su”?
//https://askubuntu.com/questions/70534/what-are-the-differences-between-su-sudo-s-sudo-i-sudo-su

OpemSSL:

https://serverfault.com/questions/9708/what-is-a-pem-file-and-how-does-it-differ-from-other-openssl-generated-key-file // type of certs
https://stackoverflow.com/questions/33494750/self-signed-certificate-for-public-and-private-ip-tomcat-7

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

openssl genrsa -out mrologs.key 4096
openssl req -new -out mrologs.csr -key mrologs.key -config openssl.cnf
openssl x509 -req -days 3650 -in mrologs.csr -signkey mrologs.key -out mrologs.crt -extensions v3_req -extfile openssl.cnf


PEM format - If they begin with -----BEGIN and you can read them in a text editor (they use base64, which is readable in ASCII, not binary format), they are in PEM format.

sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout ./logstash.key -out ./logstash.crt

openssl s_client -connect {HOSTNAME}:{PORT} -showcerts // save a remote server SSL certificate locally as a file
wget https:/server.edu:443/somepage --ca-certificate=mycertfile.pem // save a remote server SSL certificate locally as a file


Self signed certificate:

When certificate is self-signed, then issuer and subject field contains the same value. Also, there will be only this one certificate in the certificate path

openssl x509 -in ./ops-aws/ansible/playbooks/roles/filebeat-proxy/files/mrologs.crt -noout -subject -issuer
Issuer: C=PL, ST=Pomeranian, L=Gdansk, O=Kainos, OU=Enterprise, CN=mrolog
Subject: C=PL, ST=Pomeranian, L=Gdansk, O=Kainos, OU=Enterprise, CN=mrolog

Rsync:
sudo rsync -az /vagrant/ /surecert/

AWSCLI:
aws iam get-instance-profile --instance-profile-name eb_inst_profile_demo
aws iam delete-instance-profile --instance-profile-name eb_inst_profile_demo

aws iam list-instance-profiles > list_profiles.json // list the profiles and attached roles


aws iam list-server-certificates
aws iam get-server-certificate --server-certificate-name
aws iam delete-server-certificate --server-certificate-name
aws iam upload-server-certificate --server-certificate-name KSTestServerCertificate --certificate-body file://skeyos.com.crt --private-key file://app.skeyos.com.key


aws lambda get-policy --function-name daily_rds_snapshot


aws rds modify-db-cluster --db-cluster-identifier <value> --new-db-cluster-identifier <value>] --apply-immediately
aws rds create-db-cluster-snapshot --db-cluster-snapshot-identifier <value> --db-cluster-identifier <value>

aws rds create-db-cluster-snapshot --db-cluster-identifier backup-surecert-cluster-test-cluster  --db-cluster-snapshot-identifier backup-surecert-cluster-test
aws rds create-db-cluster-snapshot --db-cluster-identifier backup-surecert-cluster-develop-cluster --db-cluster-snapshot-identifier backup-surecert-cluster-develop
aws rds create-db-cluster-snapshot --db-cluster-identifier backup-surecert-cluster-accept-cluster --db-cluster-snapshot-identifier backup-surecert-cluster-accept

aws s3 cp s3://mybucket/test.txt test2.txt
aws s3 cp s3://mybucket . --recursive

DIFF:

diff -bur folder1/ folder2/

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



BOOTLOADER MISSING : 
https://askubuntu.com/questions/397485/what-to-do-when-i-get-an-attempt-to-read-or-write-outside-of-disk-hd0-error
Locate the partition in which linux is present with the help of following technique

grub rescue > ls
(hd0) (hd0, msdos9)
grub rescue > ls (hd0,msdos9)/
grub rescue > ls (hd0,msdos8)/
grub rescue > ls (hd0,msdos5)/ # suppose this is root and bootloader of linux
grub rescue > ls (hd0,msdos5)/
grub rescue > set root=(hd0,msdos5)
grub rescue > set prefix=(hd0,msdos5)/boot/grub
grub rescue > insmod normal
grub rescue > normal

Now, system's boot menu appears. Boot into linux.

sudo update-grub
sudo grub-install  /dev/sda # If the drive is hd0 the equivalent is sda, if it's hd1 then use sdb





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

SED:
sed -n 2280,2400p /var/log/wfeservices.log // print exacly these lines from file - https://unix.stackexchange.com/questions/288521/with-the-linux-cat-command-how-do-i-show-only-certain-lines-by-number/288525

GREP, EGREP, FGREP:

Here are some example scenarios:

    You have a file with a list of say ten Unix usernames in plain text. You want to search the group file on your machine to see if any of the ten users listed are in any special groups:

    grep -F -f user_list.txt /etc/group

    The reason the -F switch helps here is that the usernames in your pattern file are interpreted as plain text strings. Dots for example would be interpreted as dots rather than wild-cards.

    You want to search using a fancy expression. For example parenthesis () can be used to indicate groups with | used as an OR operator. You could run this search using -E:

    grep -E '^no(fork|group)' /etc/group

    ...to return lines that start with either "nofork" or "nogroup". Without the -E switch you would have to escape the special characters involved because with normal pattern matching they would just search for that exact pattern;

    grep '^no\(fork\|group\)' /etc/group

grep "^[^#;]" smb.conf // grep lines which does not begin with "#" or ";". The first ^ refers to the beginning of the line, so lines with comments starting after the first character will not be excluded. [^#;] means any character which is not # or ;



https://unix.stackexchange.com/questions/17949/what-is-the-difference-between-grep-egrep-and-fgrep

command inside dollar sign and parentheses: $(command):

CWD="$(cd "$(dirname $0)"; pwd)"
DIR="$( cd "$( dirname "${BASH_SOURCE[0]}" )" && pwd )"

Usage of the $ like $(echo foo) means run whatever is inside the parentheses in a subshell and return that as the value. In my example, you would get foo since echo will write foo to standard out

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


SSH Tunnels: - https://unix.stackexchange.com/questions/115897/whats-ssh-port-forwarding-and-whats-the-difference-between-ssh-local-and-remot:
ssh -L 2000:localhost:8080 10.0.1.66

Run remote command by ssh:
"ssh -o StrictHostKeyChecking=no -i ${MP_PROD_SSH_KEY} ec2-user@10.66.70.47 \'if [ -e /etc/nginx/sites-enabled/wfeservices.conf ]; then sudo rm /etc/nginx/sites-enabled/wfeservices.conf ; fi\'"

LAST:
last // show last logged users

GIT:
git log -p --author="your_email@domain.com" --since="2018-01-11" --until="2018-01-29" // list commits

git config --list //will show credential.helper = manager (this is on a windows machine)

git config credential.helper "" //To disable this cached username/password for your current local git folder, simply enter
git config credential.helper store // To store credentials
git config --global credential.helper store

git config --global user.email "krzysztofsz@kainos.com"
git config --global user.name "Krzysztof Szewczyk"

git reset --soft HEAD~1

git rebase -i HEAD~4 [commit_id] //squash 

Add executable permission for sh scripts: //https://stackoverflow.com/questions/21691202/how-to-create-file-execute-mode-permissions-in-git-on-windows
--------------------------------------------------------------------------------------------
git add foo.sh
git ls-files --stage
100644 e69de29bb2d1d6434b8b29ae775ad8c2e48c5391 0       foo.sh // permissions are 644
git update-index --chmod=+x foo.sh // mark as executable before commiting
git ls-files --stage
100755 e69de29bb2d1d6434b8b29ae775ad8c2e48c5391 0       foo.sh // permissions are 755

or:

git add --chmod=+x -- afile
git commit -m "Executable!"
--------------------------------------------------------------------------------------------

SET: //https://stackoverflow.com/questions/2853803/in-a-shell-script-echo-shell-commands-as-they-are-executed

set -x or set -o xtrace expands variables and prints a little + sign before the line.

set -v or set -o verbose does not expand the variables before printing.

Use set +x and set +v to turn off the above settings.

Switch users:
su --login --shell type_of_shell user
su -m, -p, --preserve-environment // Preserves the whole environment, ie does not set HOME, SHELL, USER nor LOGNAME.  The option is ignored if the option --login is specified.

su -c ‘command’ // To run a single command as the root user with su. This is similar to running a command with sudo, but you’ll need the root account’s password instead of your current user account’s password

sudo –i // To get a full, interactive root shell with sudo. Run the shell specified by the target user's password database entry as a login shell. This means that login-specific resource files such as .profile or .login will be read by the shell. If a command is specified, it is passed to the shell for execution via the shell's -c option. If no command is specified, an interactive shell is executed. sudo attempts to change to that user's home directory before running the shell. The command is run with an environment similar to the one a user would receive at log in. The Command environment section in the sudoers(5) manual documents how the -i option affects the environment in which a command is run when the sudoers policy is in use.

sudo -u < username > //      
-u user, --user=user
    Run the command as a user other than the default target user (usually root). The user may be either a user name or a numeric user ID (UID) prefixed with the ‘#’ character (e.g. #0 for UID 0). When running commands as a UID, many shells require that the ‘#’ be escaped with a backslash (‘\’). Some security policies may restrict UIDs to those listed in the password database. The sudoers policy allows UIDs that are not in the password database as long as the targetpw option is not set. Other security policies may not support this.

sudo su - // To get a full, interactive root shell with sudo

SUDOERS: // https://www.garron.me/en/linux/visudo-command-sudoers-file-sudo-default-editor.html
%admin – the group named "admin" (% prefix)
ALL= – on all hosts (if you distribute the same sudoers file to many computers)
(ALL) – as any target user
ALL – can run any command

A more restricted example would be:
%mailadmin   snow,rain=(root) /usr/sbin/postfix, /usr/sbin/postsuper, /usr/bin/doveadm
nobody       ALL=(root) NOPASSWD: /usr/sbin/rndc reload

In this case, the group mailadmin is allowed to run mail server control tools as user root on hosts named "snow" and "rain". The user nobody is allowed to run rndc reload as root, on all hosts, without being asked for any password. (Normally sudo asks for the invoker's own password.)





gksu // Graphical Versions of Su
ssh -t myself@192.168.100.101 "su oracle"
-t
    Force pseudo-terminal allocation. This can be used to execute arbitrary screen-based programs on a remote machine, which can be very useful, e.g. when implementing menu services. Multiple -t options force tty allocation, even if ssh has no local tty



nscd: // https://unix.stackexchange.com/questions/324913/sbin-nologin-and-bin-false-are-ignored-in-etc-passwd-as-a-user-shell
Nscd is a daemon that provides a cache for the most common name service requests. The default configuration file, /etc/nscd.conf, 
nscd -i passwd
nscd -i group

USERMOD:
usermod -L <account> // block users password

PASSWD:
passwd <username> -l // lock the account

Security issue:
cat ./*access.log | awk '{print $1}' | sort -r | uniq -c | sort -nr
cat ./*error.log | grep -o 'client: [0-9.]*' | sort -r | uniq -c | sort -nr
zcat ./*.gz | awk '{print $1}' | sort -r | uniq -c | sort -nr
zcat ./*.gz | grep -o 'client: [0-9.]*' | sort -r | uniq -c | sort -nr


Powiększanie dysku online:

Powiększamy dysk
Sprawdzamy które urządzenie scsi odpowiada za dysk w systemie
# ls -lad /sys/class/scsi_device/*/device/block/*
 
Re-skanujemy odpowiednie urządzenie scsi (podmieniamy numerki 2\:0\:0\:0 na te znalezione w wyniku poprzedniego polecenia)
# echo 1 > /sys/class/scsi_device/2\:0\:0\:0/device/rescan
 
Za pomocą parted sprawdzamy czy się pojawiła nowa wielkość dysku w systemie (w fdisk tego nie widać)
# parted /dev/sda print free
 
Powiększamy odpowiednie partycje, standardowo w okta systemowy lv jest na partycji rozszerzonej wiec powiększamy najpierw partycję 3 a potem 5 (w tym wypadku partycja 3 jest extended a partycja 5 jest partycją LVM)
# parted /dev/sda resizepart 3 100%
# parted /dev/sda resizepart 5 100%
 
Powiekszamy pv (wolumen fizyczny)
# pvresize /dev/sda5
 
Powiekszamy lv (wolumen logiczny)
# lvextend -l+100%FREE /dev/vg_name/disk
 
Powiekszamy fs (system plików)
# resize2fs /dev/vg_name/disk
 
Na centos 7 jest stary parted który nie ma resizepart
Rozwiązaniem jest instalacja growpart
# yum install cloud-utils-growpart
# growpart /dev/sda 3
# growpart /dev/sda 5

# SET TIMEZONE
timedatectl list-timezones
sudo timedatectl set-timezone Europe/Warsaw
timedatectl set-ntp yes
timedatectl //Verify it

Virtualbox mount sdcard on Linux From Windows using Virtualbox:


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

Python eating memory, testing how docker ram works:
https://utcc.utoronto.ca/~cks/space/blog/python/EatingMemory
https://shuheikagawa.com/blog/2017/05/27/memory-usage/

Start and shutdown hooks:
https://askubuntu.com/questions/30383/best-way-to-make-a-shutdown-hook

Kumple:
https://github.com/mazur92/simple-azure-vpn#note-about-operating-systems

IO waiting:
https://serverfault.com/questions/87504/is-cpu-actually-occupied-during-iowait-wa-in-top-on-linux-ec2
https://serverfault.com/questions/12679/can-anyone-explain-precisely-what-iowait-is



Windows:

BCDEDIT

Boot Configuration Data jest magazynem, w którym system Windows Vista (a także późniejsze) przechowują pliki oraz ustawienia aplikacji dotyczące rozruchu. BCDEdit.exe jest edytorem z linii poleceń systemu, dzięki któremu możemy zarządzać magazynem danych konfiguracji rozruchu.

C:\windows\system32>bcdedit

Windows Boot Manager
--------------------
identifier              {bootmgr}
device                  partition=\Device\HarddiskVolume1
path                    \EFI\Microsoft\Boot\bootmgfw.efi
description             Windows Boot Manager
locale                  en-US
inherit                 {globalsettings}
integrityservices       Enable
default                 {current}
resumeobject            {8c72885b-f9b6-11e6-a6a2-28f10e0e9eb6}
displayorder            {current}
toolsdisplayorder       {memdiag}
timeout                 30

Windows Boot Loader
-------------------
identifier              {current}
device                  partition=C:
path                    \windows\system32\winload.efi
description             Windows 8.1
locale                  en-US
inherit                 {bootloadersettings}
recoverysequence        {8c72885d-f9b6-11e6-a6a2-28f10e0e9eb6}
integrityservices       Enable
recoveryenabled         Yes
isolatedcontext         Yes
allowedinmemorysettings 0x15000075
osdevice                partition=C:
systemroot              \windows
resumeobject            {8c72885b-f9b6-11e6-a6a2-28f10e0e9eb6}
nx                      OptIn
bootmenupolicy          Standard
hypervisorlaunchtype    Off


BCDEDIT /Set {current} hypervisorlaunchtype auto //Enable hyper-v at the startup





Service monitoring tools:
Icinga
Nagios

Internet proxy:
squid

Proxy/loadbalancer:
HAproxy
Nginx
https://github.com/containous/traefik
https://github.com/linkerd/linkerd

Proxy client:
luminati

Backups:
Bacula
backup - http://backup.github.io/backup/v4/
duplicity+duply
https://restic.net/
rdiff-backup
borg-backup
bareos
bacula

Static analizer:
FindBugs™ - Find Bugs in Java Program
checkstyle
PMD (software)

Network:
Clumsy makes your network condition on Windows significantly worse, but in a managed and interactive manner - https://jagt.github.io/clumsy/

Radius:


Witam. Na potrzeby swoich dzieci (a może raczej ze względu na wygodę własną) ich kompputery od kilku lat skonfigurowane są na maszyny niezniszczalne. Dzieci mają uprawnienia adminów (mogą robić co chcą, nawet zainstalować sobie milion wirusów), ale przy każdym restarcie ich systemy (obecnie Win 10 na 64GB dyskach SSD) przywracane są do stanu początkowego (Reboot Restore RX / Deep Freeze). Tak więc za każdym razem systemy są jak nowe. Nie stosuję antywirusów (trochę podśmiewam się ze środowiska antywirusowych pasjonatów ;-) przez co maszyny mają 100% swojej wydajności. Maszyny jak widać nie do zabicia. Oczywiście mają podłączone udziały sieciowe, na których mogą przechowywać dane trwałe, ale sam system jest niezniaszczalny. Oczywiście co kilka tygodni, czasem miesięcy, odfreezowuję dyski, przeprowadzam aktualizację, robię clonezillą backupy i tak dalej.

Ale dzieci podrosły, i zaczął się problem z save-ami statusów gier komputerowych. O ile w przypadku instalacji niektórych gier, wystarczy zainstalować je na udziale sieciowym (i co ciekawe, po restarcie systemu ruszają z kopyta z podmontowanego dysku - co może być dziwne, bo przecież takie rzeczy, jak rejestr, program data itp są kasowane / zapominane przy restarcie) o tyle z save-ami statusów robi się problem. Każda gra, każdy tytuł, trzyma je w tylko sobie znanym miejscu. Zazwyczaj, gdzieś na dysku systemowym.

I szczerze powiem - nie wiem jak podejść do tematu / jak uporać sobie z problemem zapisywania statusów gier komputerowych w tak spreparowanym środowisku. Z drugiej strony nie chcę rezygnować z wygodnego dla mnie rozwiązania z maszynami nie do zabicia. Rzuci ktoś jakiś pomysł, albo sprawdzone rozwiązanie?
