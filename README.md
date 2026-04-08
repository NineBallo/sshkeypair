# For Just Getting SSH Login Going:
cat ~/.ssh/id_ed25519.pub | ssh user@123.45.56.78 "mkdir ~/.ssh; cat >> ~/.ssh/authorized_keys"

# For Getting SSH Auth Going (after running the previous):
## RUN THIS FROM LINUX/WSL Powershell will fuck your shit up
ssh -t your_user@your_server_ip "sudo apt-get update && \
sudo apt-get install -y libpam-ssh-agent-auth && \
echo 'Defaults env_keep += \"SSH_AUTH_SOCK\"' | sudo tee /etc/sudoers.d/pam_ssh_agent > /dev/null && \
sudo chmod 0440 /etc/sudoers.d/pam_ssh_agent && \
sudo cp ~/.ssh/authorized_keys /etc/ssh/sudo_authorized_keys && \
sudo chown root:root /etc/ssh/sudo_authorized_keys && \
sudo chmod 644 /etc/ssh/sudo_authorized_keys && \
grep -q 'pam_ssh_agent_auth.so' /etc/pam.d/sudo || sudo sed -i '/@include common-auth/i auth sufficient pam_ssh_agent_auth.so file=/etc/ssh/sudo_authorized_keys' /etc/pam.d/sudo"
