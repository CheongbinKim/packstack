setenforce 0

vi /etc/selinux/config

SELINUX=disabled

systemctl disable NetworkManager

systemctl disable firewalld

systemctl enable network

hostnamectl set-hostname kstack --static

yum update

reboot

...

yum install centos-release-openstack-train

yum install openstack-packstack

git clone https://github.com/cheongbinkim/packstack

cd packstack

sed -i 's/10.254.1.199/your_ip/' answers.txt

packstack --answer-file answers.txt | tee output.txt

packstack --answer-file answers.txt --debug | tee output.txt

# add external network

neutron net-create external_network --provider:network_type flat --provider:physical_network extnet  --router:external

neutron subnet-create --name public_subnet --enable_dhcp=False --allocation-pool=start=10.254.1.160,end=10.254.1.170 --gateway=10.254.1.254 external_network 10.254.1.0/24

neutron router-create router1

neutron router-gateway-set router1 external_network

neutron net-create private_network

neutron subnet-create --name private_subnet private_network 192.168.100.0/24

neutron router-interface-add router1 private_subnet

# create image

wget cloud.centos.org/centos/7/images/CentOS-7-x86_64-GenericCloud-1907.qcow2

glance image-create --container-format=bare --disk-format=qcow2 --name=CentOS7-1907 < CentOS-7-x86_64-GenericCloud-1907.qcow2

