FROM centos:7
ENV container docker
COPY files/jdk-8u291-linux-x64.rpm files/fmw_12.2.1.4.0_wls_lite_generic.jar files/wls_response files/createCentralInventory.sh /tmp/
RUN (cd /lib/systemd/system/sysinit.target.wants/; for i in *; do [ $i == \
systemd-tmpfiles-setup.service ] || rm -f $i; done); \
rm -f /lib/systemd/system/multi-user.target.wants/*;\
rm -f /etc/systemd/system/*.wants/*;\
rm -f /lib/systemd/system/local-fs.target.wants/*; \
rm -f /lib/systemd/system/sockets.target.wants/*udev*; \
rm -f /lib/systemd/system/sockets.target.wants/*initctl*; \
rm -f /lib/systemd/system/basic.target.wants/*;\
rm -f /lib/systemd/system/anaconda.target.wants/*; \
yum install -y unzip /tmp/jdk-8u291-linux-x64.rpm; \
groupadd oinstall; \
useradd -g oinstall oracle; \
mkdir /opt/oracle; \
chown oracle:oinstall /opt/oracle; \
chmod 0755 /opt/oracle; \
/tmp/createCentralInventory.sh /etc oinstall; \
su -l oracle -c '/usr/java/jdk1.8.0_291-amd64/bin/java -jar /tmp/fmw_12.2.1.4.0_wls_lite_generic.jar -silent -responseFile /tmp/wls_response'; \
rm -rf /tmp/*;
VOLUME [ "/sys/fs/cgroup" ]
CMD ["/usr/sbin/init"]
