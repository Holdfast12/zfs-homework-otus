воспользовался инструкцией установки на RHEL с сайта https://zfsonlinux.org/
dnf -y install https://zfsonlinux.org/epel/zfs-release-2-2$(rpm --eval "%{dist}").noarch.rpm
kABI-tracking kmod
dnf config-manager --disable zfs
dnf config-manager --enable zfs-kmod
dnf -y install zfs wget
modprobe zfs
echo zfs >/etc/modules-load.d/zfs.conf

1. ПЕРВОЕ ЗАДАНИЕ############################################################################

zpool create poolzadanie1 sd{b..g}
zfs create poolzadanie1/fs-gzip
zfs create poolzadanie1/fs-gzip-n
zfs create poolzadanie1/fs-zle
zfs create poolzadanie1/fs-lzjb
zfs create poolzadanie1/fs-lz4

[root@server ~]# zfs list
NAME                     USED  AVAIL     REFER  MOUNTPOINT
poolzadanie1             331K  5.45G       28K  /poolzadanie1
poolzadanie1/fs-gzip      24K  5.45G       24K  /poolzadanie1/fs-gzip
poolzadanie1/fs-gzip-n    24K  5.45G       24K  /poolzadanie1/fs-gzip-n
poolzadanie1/fs-lz4       24K  5.45G       24K  /poolzadanie1/fs-lz4
poolzadanie1/fs-lzjb      24K  5.45G       24K  /poolzadanie1/fs-lzjb
poolzadanie1/fs-zle       24K  5.45G       24K  /poolzadanie1/fs-zle

zfs set compression=gzip poolzadanie1/fs-gzip
zfs set compression=gzip-9 poolzadanie1/fs-gzip-n
zfs set compression=lz4 poolzadanie1/fs-lz4
zfs set compression=lzjb poolzadanie1/fs-lzjb
zfs set compression=zle poolzadanie1/fs-zle

wget -O War_and_Peace.txt http://www.gutenberg.org/ebooks/2600.txt.utf-8
cp ./War_and_Peace.txt /poolzadanie1/fs-gzip/
cp ./War_and_Peace.txt /poolzadanie1/fs-gzip-n/
cp ./War_and_Peace.txt /poolzadanie1/fs-lz4/
cp ./War_and_Peace.txt /poolzadanie1/fs-lzjb/
cp ./War_and_Peace.txt /poolzadanie1/fs-zle/

[root@server ~]# zfs get compression
NAME                    PROPERTY     VALUE           SOURCE
poolzadanie1            compression  off             default
poolzadanie1/fs-gzip    compression  gzip            local
poolzadanie1/fs-gzip-n  compression  gzip-9          local
poolzadanie1/fs-lz4     compression  lz4             local
poolzadanie1/fs-lzjb    compression  lzjb            local
poolzadanie1/fs-zle     compression  zle             local

[root@server ~]# df
Filesystem             1K-blocks    Used Available Use% Mounted on
devtmpfs                  474616       0    474616   0% /dev
tmpfs                     493976       0    493976   0% /dev/shm
tmpfs                     493976    6656    487320   2% /run
tmpfs                     493976       0    493976   0% /sys/fs/cgroup
/dev/sda2               18420736 1783996  16636740  10% /
tmpfs                      98792       0     98792   0% /run/user/1000
poolzadanie1             5703296     128   5703168   1% /poolzadanie1
poolzadanie1/fs-gzip     5704448    1280   5703168   1% /poolzadanie1/fs-gzip
poolzadanie1/fs-gzip-n   5704448    1280   5703168   1% /poolzadanie1/fs-gzip-n
poolzadanie1/fs-zle      5706496    3328   5703168   1% /poolzadanie1/fs-zle
poolzadanie1/fs-lzjb     5705728    2560   5703168   1% /poolzadanie1/fs-lzjb
poolzadanie1/fs-lz4      5705344    2176   5703168   1% /poolzadanie1/fs-lz4

gzip* алгоритмы лучше

2. ВТОРОЕ ЗАДАНИЕ#############################################################

scp -P 2222 -r ./zpoolexport/ vagrant@127.1:/home/vagrant
mv /home/vagrant/zpoolexport/ /root/

zpool import -d /root/zpoolexport/
zpool import -d /root/zpoolexport/ otus

размер хранилища
[root@server ~]# zfs list
NAME                     USED  AVAIL     REFER  MOUNTPOINT
otus                    2.04M   350M       24K  /otus
otus/hometask2          1.88M   350M     1.88M  /otus/hometask2
poolzadanie1            10.4M  5.44G     29.5K  /poolzadanie1
poolzadanie1/fs-gzip    1.24M  5.44G     1.24M  /poolzadanie1/fs-gzip
poolzadanie1/fs-gzip-n  1.23M  5.44G     1.23M  /poolzadanie1/fs-gzip-n
poolzadanie1/fs-lz4     2.02M  5.44G     2.02M  /poolzadanie1/fs-lz4
poolzadanie1/fs-lzjb    2.41M  5.44G     2.41M  /poolzadanie1/fs-lzjb
poolzadanie1/fs-zle     3.23M  5.44G     3.23M  /poolzadanie1/fs-zle

тип pool - mirror
[root@server ~]# zpool status otus
  pool: otus
 state: ONLINE
status: Some supported and requested features are not enabled on the pool.
	The pool can still be used, but some features are unavailable.
action: Enable all features using 'zpool upgrade'. Once this is done,
	the pool may no longer be accessible by software that does not support
	the features. See zpool-features(7) for details.
config:

	NAME                         STATE     READ WRITE CKSUM
	otus                         ONLINE       0     0     0
	  mirror-0                   ONLINE       0     0     0
	    /root/zpoolexport/filea  ONLINE       0     0     0
	    /root/zpoolexport/fileb  ONLINE       0     0     0

errors: No known data errors

значение recordsize
[root@server ~]# zfs get recordsize | grep otus
otus                    recordsize  128K     local
otus/hometask2          recordsize  128K     inherited from otus

какое сжатие используется
[root@server ~]# zfs get compression | grep otus
otus                    compression  zle             local
otus/hometask2          compression  zle             inherited from otus

какая контрольная сумма используется
[root@server ~]# zfs get checksum | grep otus
otus                    checksum  sha256     local
otus/hometask2          checksum  sha256     inherited from otus

3. ТРЕТЬЕ ЗАДАНИЕ#######################################################################

scp -P 2222 -r ./otus_task2.file vagrant@127.1:/home/vagrant
mv /home/vagrant/otus_task2.file /root/

zfs receive otus/storage < otus_task2.file

[root@server ~]# zfs list
NAME                     USED  AVAIL     REFER  MOUNTPOINT
otus                    4.94M   347M       25K  /otus
otus/hometask2          1.88M   347M     1.88M  /otus/hometask2
otus/hometask3          2.83M   347M     2.83M  /otus/hometask3
poolzadanie1            10.4M  5.44G     29.5K  /poolzadanie1
poolzadanie1/fs-gzip    1.24M  5.44G     1.24M  /poolzadanie1/fs-gzip
poolzadanie1/fs-gzip-n  1.23M  5.44G     1.23M  /poolzadanie1/fs-gzip-n
poolzadanie1/fs-lz4     2.02M  5.44G     2.02M  /poolzadanie1/fs-lz4
poolzadanie1/fs-lzjb    2.41M  5.44G     2.41M  /poolzadanie1/fs-lzjb
poolzadanie1/fs-zle     3.23M  5.44G     3.23M  /poolzadanie1/fs-zle

[root@server hometask3]# ls
10M.file   Moby_Dick.txt      cinderella.tar    homework4.txt  world.sql
Limbo.txt  War_and_Peace.txt  for_examaple.txt  task1

tar -xvf cinderella.tar

cd /otus/storage/task1/file_mess

[root@server file_mess]# cat secret_message 
https://github.com/sindresorhus/awesome




