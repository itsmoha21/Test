docker run -d \
  --name ubuntu22 \
  --memory=2g \
  --cpus=2 \
  -p 2222:22 \
  -e ROOT_PASSWORD='ChangeThisPassword123!' \
  ubuntu:22.04 \
  bash -c '
    apt-get update &&
    DEBIAN_FRONTEND=noninteractive apt-get install -y openssh-server &&
    mkdir -p /run/sshd &&
    echo "root:$ROOT_PASSWORD" | chpasswd &&
    sed -i "s/^#\?PermitRootLogin.*/PermitRootLogin yes/" /etc/ssh/sshd_config &&
    sed -i "s/^#\?PasswordAuthentication.*/PasswordAuthentication yes/" /etc/ssh/sshd_config &&
    /usr/sbin/sshd -D
  '
