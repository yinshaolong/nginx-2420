# Firewall

# Enabling the firewall on the server
- Update the system with the latest packages.
`sudo pacman -Syu`
    - if prompted, follow instructions to update the system (e.g. type “Y” and press enter)


- Download the package UFW (Uncomplicated Firewall)
    - UFW is a fire wall program that manages nftables or iptables.
    - if prompted, follow instructions to update the system (e.g. type “Y” and press enter)
`sudo pacman -S ufw`

- Enable the UFW service
    - The following command will start the UFW service (--now) and enable it to start on boot.
`sudo systemctl enable --now ufw.service`

- Check the status of UFW 
    - The following command will show the status of UFW.
    - it should return inactive if UFW was just installed.
`sudo ufw status verbose`

- Confirm that ufw is configured by default to deny all incoming traffic and to allow all outgoing traffic (allowing outgoing traffic is fine since your own systems files shouldn't be a threat).
```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
```

- Allow SSH connections on port 22 with either of the following commands (they are equivalent since SSH's default port is 22):
```bash
sudo ufw allow SSH
```
or
```bash
sudo ufw allow 22
```
- You can limit the rate of ssh connections to prevent brute force attacks with the following command (6 attempts in 30 seconds will deny an incoming address):
`sudo ufw limit SSH`

- Allow http connections since we will be using a web server with either of the following commands (they are equivalent since HTTP's default port is 80):
```bash
sudo ufw allow http
```
or
```bash
sudo ufw allow 80
```

- Turn on firewall now that some of the rules are set:
`sudo ufw enable`

- Check the status of the firewall to ensure that everything is working:
`sudo ufw status verbose`


# Placing backend binary on the droplet
- On the host machine, download the `hello-server` file from the following link: `https://gitlab.com/cit2420/2420_notes_w24/-/blob/main/attachments/hello-server?ref_type=heads`

- On the host machine, after the `hello-server` file has been downloaded, we use SFTP (Secure File Transfer Protocol) to transfer the `hello-server` file from our host-machine to the droplet.
    - first use the following command to connect to your remote droplet using the sftp utility (replace `user` with your username and `ip-address` with your droplet's IP address).
        - If it was successful, it should say ` Connected to <ip-address>`
`sftp -i ~/.ssh/do-key user@ip-address` 
e.g. `sftp -i ~/.ssh/do-key shaol@64.23.250.17`

- change directories (cd command) to the directory where `hello-server` is located.
    - assuming the `hello-server` file hasn't been moved from `~/Downloads` directory the command would be:
`cd ~/Downloads`

- After connecting to the droplet, use the following command to transfer the `hello-server` file to the droplet (default is the droplet's home directory).
    - If successful, it should say `Uploading hello-server to /home/<user>/hello-server
hello-server ` in the terminal.
`put hello-server`

- After the file has been transferred, exit the sftp utility by typing `exit` in the terminal.

- Make a binary directory in the nginx-2420 directory from part 1.
`sudo mkdir -p /web/html/nginx-2420/bin` 


- move (mv) `hello-server` to the `/web/html/nginx-2420/bin` directory of the arch linux server.
`sudo mv hello-server /web/html/nginx-2420/bin`  

