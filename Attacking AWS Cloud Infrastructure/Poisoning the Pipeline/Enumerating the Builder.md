# Enumerating the Builder

OS and Kernel information
>``` shell
>jenkins@fcd3cc360d9e:~/agent/workspace/image-transform$ uname -a
>
># ========== Expected Result ==========
>uname -a
>Linux fcd3cc360d9e 4.14.309-231.529.amzn2.x86_64 #1 SMP Tue Mar 14 23:44:59 UTC 2023 x86_64 GNU/Linux
># =====================================
>
>jenkins@fcd3cc360d9e:~/agent/workspace/image-transform$ cat /etc/os-release
>
># ========== Expected Result ==========
>cat /etc/os-release
>PRETTY_NAME="Debian GNU/Linux 11 (bullseye)"
>NAME="Debian GNU/Linux"
>VERSION_ID="11"
>VERSION="11 (bullseye)"
>VERSION_CODENAME=bullseye
>ID=debian
>HOME_URL="https://www.debian.org/"
>SUPPORT_URL="https://www.debian.org/support"
>BUG_REPORT_URL="https://bugs.debian.org/"
># =====================================
>```

Listing Working Directory
>``` shell
>jenkins@fcd3cc360d9e:~/agent/workspace/image-transform$ ls
>
># ========== Expected Result ==========
>ls
>Jenkinsfile
>README.md
>image-processor-template.yml
># =====================================
>```

Listing Home Directory
>``` shell
>jenkins@fcd3cc360d9e:~/agent/workspace/image-transform$ cd ~
>
>jenkins@fcd3cc360d9e:~$ ls -a
>
># ========== Expected Result ==========
>ls -a
>.
>..
>.bash_logout
>.bashrc
>.cache
>.config
>.profile
>.ssh
>agent
># =====================================
>```

Checking for Private Keys and Authorized Keys
>``` shell
>jenkins@fcd3cc360d9e:~$ ls -a .ssh
>
># ========== Expected Result ==========
>ls -a
>.
>..
>authorized_keys
># =====================================
>
>jenkins@fcd3cc360d9e:~$ cat .ssh/authorized_keys
>
># ========== Expected Result ==========
>cat .ssh/authorized_keys
>ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDP+HH9VS2Oe1djuSNJWhbYaswUC544I0QCp8sSdyTs/yQiytovhTAP/Z1eA2n0OZB2/4/oJn5wpdui8TTnkQGb6KdiLMfO1hZep7QVAY1QAwxLaKz6iEAFUuNxRrctwebVNCVokZr1yQmvlW0qKdQ5RaqU5xu35oDsYhk5vcQj+o8FAhkI5zkA4Mq6UPdLgakxEHaxJT4vWL7rYYvMW8Wz2/ngZS4LlcYmTVRiSRxFs1LdwTwC5DDlL05sqqFGED+Gs6Jy6VFhCZE0oFGZ0EoIMXkjasifVUvf7jPJ/qFKRP47AwJ6zMUUGlwf8t5HFwzK6ZmDoKUiUHg6ZdOEHxHYJRXqQ1IILpgp9g+1+NhYpIwpnvkuurCLFpKby4rRKkECueRUjSMsArKuTdPBZZ1cpC12z/czcGzTib1AjIUaNwobsU5dwVbgPLnDJ6vYVQGTNq5/PLRBeHCluzpaiHFtrP80PL9XomVhCI+lGTKxD9QxYq+mSYyESiEeu7idqw8= jenkins@jenkins
># =====================================
>```

Checking Network Configuration
>``` shell
>jenkins@fcd3cc360d9e:~$ ifconfig
>
># ========== Expected Result ==========
>ifconfig
>
>bash: ifconfig: command not found
># =====================================
>
>jenkins@fcd3cc360d9e:~$ ip a
>
># ========== Expected Result ==========
>ip a
>bash: ip: command not found
># =====================================
>```

Checking Mounts
>``` shell
>jenkins@fcd3cc360d9e:~$ cat /proc/mounts
>
># ========== Expected Result ==========
>cat /proc/mounts
>overlay / overlay rw,relatime,lowerdir=/var/lib/docker/overlay2/l/ZWMYT5LL7SJG7W2C2AQDU3DNZU:/var/lib/docker/overlay2/l/NWVNHZEQTXKQV7TK6L5PBW2LY6:/var/lib/docker/overlay2/l/XQAFTST24ZNNZODESKXRXG2DT3:/var/lib/docker/overlay2/l/XQEBX4RY52MDAKX5AHOFQ33C3J:/var/lib/docker/overlay2/l/RL6A3EXVAAKLS2H3DCFGHT6G4I:/var/lib/docker/overlay2/l/RK5MUYP5EXDS66AROAZDUW4VJZ:/var/lib/docker/overlay2/l/GITV6R24OXBRFWILXTIPQJWAUO:/var/lib/docker/overlay2/l/IJIDXIBWIZUYBIWUF5YWXCOG4L:/var/lib/docker/overlay2/l/6MLZE4Z6A4O4GGDABKH4SEB2ML:/var/lib/docker/overlay2/l/DWFB6EYO3HEPBCCAWYQ4256GNS:/var/lib/docker/overlay2/l/I7JY2SWCL2IPGXKRREITBKE3XF:/var/lib/docker/overlay2/l/U3ULKCXTN7B3QA7WZBNB67UESW,upperdir=/var/lib/docker/overlay2/b01b1c72bc2d688d01493d2aeda69d6a4ec1f6dbb3934b8c1ba00aed3040de4a/diff,workdir=/var/lib/docker/overlay2/b01b1c72bc2d688d01493d2aeda69d6a4ec1f6dbb3934b8c1ba00aed3040de4a/work 0 0
>proc /proc proc rw,nosuid,nodev,noexec,relatime 0 0
>tmpfs /dev tmpfs rw,nosuid,size=65536k,mode=755 0 0
>devpts /dev/pts devpts rw,nosuid,noexec,relatime,gid=5,mode=620,ptmxmode=666 0 0
>sysfs /sys sysfs rw,nosuid,nodev,noexec,relatime 0 0
>tmpfs /sys/fs/cgroup tmpfs rw,nosuid,nodev,noexec,relatime,mode=755 0 0
>cgroup /sys/fs/cgroup/systemd cgroup rw,nosuid,nodev,noexec,relatime,xattr,release_agent=/usr/lib/systemd/systemd-cgroups-agent,name=systemd 0 0
>cgroup /sys/fs/cgroup/pids cgroup rw,nosuid,nodev,noexec,relatime,pids 0 0
>cgroup /sys/fs/cgroup/devices cgroup rw,nosuid,nodev,noexec,relatime,devices 0 0
>cgroup /sys/fs/cgroup/freezer cgroup rw,nosuid,nodev,noexec,relatime,freezer 0 0
>cgroup /sys/fs/cgroup/cpuset cgroup rw,nosuid,nodev,noexec,relatime,cpuset 0 0
>cgroup /sys/fs/cgroup/blkio cgroup rw,nosuid,nodev,noexec,relatime,blkio 0 0
>cgroup /sys/fs/cgroup/perf_event cgroup rw,nosuid,nodev,noexec,relatime,perf_event 0 0
>cgroup /sys/fs/cgroup/hugetlb cgroup rw,nosuid,nodev,noexec,relatime,hugetlb 0 0
>cgroup /sys/fs/cgroup/cpu,cpuacct cgroup rw,nosuid,nodev,noexec,relatime,cpu,cpuacct 0 0
>cgroup /sys/fs/cgroup/net_cls,net_prio cgroup rw,nosuid,nodev,noexec,relatime,net_cls,net_prio 0 0
>cgroup /sys/fs/cgroup/memory cgroup rw,nosuid,nodev,noexec,relatime,memory 0 0
>mqueue /dev/mqueue mqueue rw,nosuid,nodev,noexec,relatime 0 0
>/dev/xvda1 /run xfs rw,noatime,attr2,inode64,noquota 0 0
>/dev/xvda1 /tmp xfs rw,noatime,attr2,inode64,noquota 0 0
>/dev/xvda1 /home/jenkins xfs rw,noatime,attr2,inode64,noquota 0 0
>/dev/xvda1 /run xfs rw,noatime,attr2,inode64,noquota 0 0
>/dev/xvda1 /etc/resolv.conf xfs rw,noatime,attr2,inode64,noquota 0 0
>/dev/xvda1 /etc/hostname xfs rw,noatime,attr2,inode64,noquota 0 0
>/dev/xvda1 /etc/hosts xfs rw,noatime,attr2,inode64,noquota 0 0
>shm /dev/shm tmpfs rw,nosuid,nodev,noexec,relatime,size=65536k 0 0
># =====================================
>```

Checking Capability for container
>``` shell
>jenkins@fcd3cc360d9e:~$ cat /proc/1/status | grep Cap
>
># ========== Expected Result ==========
>cat /proc/1/status | grep Cap
>CapInh: 0000000000000000
>CapPrm: 0000003fffffffff
>CapEff: 0000003fffffffff
>CapBnd: 0000003fffffffff
>CapAmb: 0000000000000000
># =====================================
>```

Decoding the capabilities
>``` shell
>kali@kali:~$ capsh --decode=0000003fffffffff
>
># ========== Expected Result ==========
>0x0000003fffffffff=cap_chown,cap_dac_override,cap_dac_read_search,cap_fowner,cap_fsetid,cap_kill,cap_setgid,cap_setuid,cap_setpcap,cap_linux_immutable,cap_net_bind_service,cap_net_broadcast,cap_net_admin,cap_net_raw,cap_ipc_lock,cap_ipc_owner,cap_sys_module,cap_sys_rawio,cap_sys_chroot,cap_sys_ptrace,cap_sys_pacct,cap_sys_admin,cap_sys_boot,cap_sys_nice,cap_sys_resource,cap_sys_time,cap_sys_tty_config,cap_mknod,cap_lease,cap_audit_write,cap_audit_control,cap_setfcap,cap_mac_override,cap_mac_admin,cap_syslog,cap_wake_alarm,cap_block_suspend,cap_audit_read
># =====================================
>```

Discovering AWS Keys
>``` shell
>jenkins@fcd3cc360d9e:~$ env | grep AWS
>
># ========== Expected Result ==========
>env | grep AWS
>AWS_DEFAULT_REGION=us-east-1
>AWS_REGION=us-east-1
>AWS_SECRET_ACCESS_KEY=W4gtNvsaeVgx5278oy5AXqA9XbWdkRWfKNamjKXo
>AWS_ACCESS_KEY_ID=AKIAUBHUBEGIMU2Y5GY7
># =====================================
>```

Lab 1 - Read the /etc/os-release file. What is the name of the operating system in use?
>``` shell
>
>```
>

Lab 2 - Discover the flag in the "secret" file.
>``` shell
>
>```
>

Lab 3 - Discover the environment variable with a "flag".
>``` shell
>
>```
>
