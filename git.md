sudo pacman -S openssh

sudo systemctl start sshd
sudo systemctl enable sshd

ssh-keygen -t rsa -b 4096 -C "your_email@example.com"

cat ~/.ssh/id_rsa.pub

Enter the output in SSH keys in Github and then add a new SSH key with any title and the cat output into the "key" box
