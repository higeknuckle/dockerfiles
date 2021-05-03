FROM centos:7 AS builder
ENV container docker
COPY files/jdk-8u291-linux-x64.rpm files/fmw_12.2.1.4.0_wls_lite_generic.jar files/wls_response files/createCentralInventory.sh /tmp/
RUN yum install -y /tmp/jdk-8u291-linux-x64.rpm; \
groupadd oinstall; \
useradd -g oinstall oracle; \
mkdir /opt/oracle; \
chown oracle:oinstall /opt/oracle; \
chmod 0755 /opt/oracle; \
/tmp/createCentralInventory.sh /etc oinstall; \
su -l oracle -c '/usr/java/jdk1.8.0_291-amd64/bin/java -jar /tmp/fmw_12.2.1.4.0_wls_lite_generic.jar -silent -responseFile /tmp/wls_response';
VOLUME [ "/sys/fs/cgroup" ]
CMD ["/usr/sbin/init"]

FROM centos:7
ENV container docker
COPY --from=builder /opt/oracle /opt/oracle
COPY --from=builder /usr/java /usr/java
COPY --from=builder /etc/oraInst.loc /etc/oraInst.loc
RUN (cd /lib/systemd/system/sysinit.target.wants/; for i in *; do [ $i == \
systemd-tmpfiles-setup.service ] || rm -f $i; done); \
rm -f /lib/systemd/system/multi-user.target.wants/*; \
rm -f /etc/systemd/system/*.wants/*; \
rm -f /lib/systemd/system/local-fs.target.wants/*; \
rm -f /lib/systemd/system/sockets.target.wants/*udev*; \
rm -f /lib/systemd/system/sockets.target.wants/*initctl*; \
rm -f /lib/systemd/system/basic.target.wants/*; \
rm -f /lib/systemd/system/anaconda.target.wants/*; \
groupadd oinstall; \
useradd -g oinstall oracle; \
chown oracle:oinstall /opt/oracle; \
chmod 0755 /opt/oracle;
VOLUME [ "/sys/fs/cgroup" ]
CMD ["/usr/sbin/init"]
