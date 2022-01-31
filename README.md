# Portable OpenSSH Honeypot version by enohka

This modification of OpenSSH will log all password login attempts in a json like format at:

/var/log/hp

A sample log file at this location looks like this:

hp_1643514716.log
```
{ "ipaddress": "84.171.**.**", "username": "root", "password": "wrongpassword", "time": 1643514716 }
```

Due to the nature of unix timestamps it is possible that multible login attempts occour within the same milisecond thus it will log more then one attempt into the same file.

Besides the additional logging, all functionality of OpenSSH stays the same, It will still be possible to login via SSH.


### Building from git

If building from git, you'll need [autoconf](https://www.gnu.org/software/autoconf/) installed to build the ``configure`` script. The following commands will check out and build portable OpenSSH from git:

```
git clone https://github.com/enohka/openssh-portable 
cd openssh-portable
autoreconf
./configure
make
```

### Installation

I have installed my honeypot on a plain debian 11.2 (bullseye) installation with ssh server option 

After building run these commands

```
# The portable is looking at /usr/local/etc for sshd_conf and the like so we are creating a symlink
sn -l /etc/ssh /usr/local/etc

systemctl stop sshd
# Copy the custom built sshd to the original location
cp <path to your sshd binary> /usr/sbin/sshd
systemctl start sshd
```