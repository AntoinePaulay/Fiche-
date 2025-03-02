
1.apt install openssh-server
* systemctl status ssh
* adduser wilder
* nano /etc/ssh/sshd_config
* Include /etc/ssh/sshd_config.d/*.conf
    Port 22
    AllowUsers wilder
    PermitRootLogin no
* systemctl restart sshd
2. client
* openssh-client
* ssh <user>@<adresseIPServeurSSH>
* ssh-keygen
* ssh-copy-id -p 22 -i
