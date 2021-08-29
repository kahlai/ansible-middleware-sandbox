
FROM centos:8

ENV container docker

RUN (cd /lib/systemd/system/sysinit.target.wants/; for i in *; do [ $i == systemd-tmpfiles-setup.service ] || rm -f $i; done); \
rm -f /lib/systemd/system/multi-user.target.wants/*;\
rm -f /etc/systemd/system/*.wants/*;\
rm -f /lib/systemd/system/local-fs.target.wants/*; \
rm -f /lib/systemd/system/sockets.target.wants/*udev*; \
rm -f /lib/systemd/system/sockets.target.wants/*initctl*; \
rm -f /lib/systemd/system/basic.target.wants/*;\
rm -f /lib/systemd/system/anaconda.target.wants/*;

VOLUME [ "/sys/fs/cgroup" ]

RUN yum -y install openssh-server passwd; yum clean all; systemctl enable sshd.service

RUN chmod u+s /usr/bin/ping && \
   echo "root:root" | chpasswd

RUN ssh-keygen -t rsa -f /etc/ssh/ssh_host_rsa_key -N '' 

EXPOSE 22

CMD ["/usr/sbin/init"]
