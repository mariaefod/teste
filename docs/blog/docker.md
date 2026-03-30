sudo apt update

sudo apt install docker.io

sudo systemctl enable --now docker

sudo usermod -aG docker $USER

sudo chmod 666 /var/run/docker.sock

make container
