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
