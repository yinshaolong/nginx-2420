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


# Creating a new service file to run the backend
- Make a service file in the `/etc/systemd/system` directory (name can be anything, but we will default to - hello-server.service).
`sudo vim /etc/systemd/system/hello-server.service`

- Hit “i” (insert mode) on the keyboard and add the following to the service file
    - ExecStart is the path to the binary file (in our case it is the backend binary file `hello-server`).
    - WantedBy is the most frequently used way to state how a unit should be enabled.

:
```bash
[Unit] 
Description=Hello Server Service

[Service]
ExecStart=/web/html/nginx-2420/bin/hello-server 

[Install]
WantedBy=multi-user.target
```

- Save the file by pressing `esc` (to leave insert mode) then `:wq` (to write and quit) and hit `enter`.
`esc :wq`

- Reload the systemd daemon to read the new service file.
`sudo systemctl daemon-reload`

- Start the service.
`sudo systemctl start hello-server.service`

- Enable the service to start on boot.
    - (If it was successful should say `Created symlink /etc/systemd/system/multi-user.target.wants/hello-server.service → /etc/systemd/system/hello-server.service.`)
`sudo systemctl enable hello-server.service`

- Check the status of the service to ensure that it is running.
`sudo systemctl status hello-server.service`
    - If it was successful, it should say `Active: active (running) since <date>` 
    e.g. `Active: active (running) since Wed 2024-04-10 07:03:58 UTC; 33s ago`


# Editing the Nginx configuration file
- Edit the Nginx sites-available configuration file.
`sudo vim /etc/nginx/sites-available/nginx-2420.conf`

- hit "i" (to enter insert mode) and replace the contents of the file with the following:
     - the proxy_pass is passing the request to the /hey and /echo locations to the backend server running on the droplet:
```
server {
    listen 80;
    root /web/html/nginx-2420;

    location /hey {
        proxy_pass http://127.0.0.1:8080; 
    }

    location /echo {
        proxy_pass http://127.0.01:8080; 
    }
}
```

- Save the file by pressing `esc` (to leave insert mode) then `:wq` (to write and quit) and hit `enter`.
`esc :wq`

- Start the Nginx service if it is not already running:
`sudo systemctl start nginx`
    - Else, restart the Nginx service:
    `sudo systemctl restart nginx`

- Check the status of the Nginx service to make sure it is active
    - it should say `Active: active (running) since <date>` if it was successful.
`sudo systemctl status nginx`
e.g. `Active: active (running) since Wed 2024-04-10 17:25:22 UTC; 9min ago`


# Testing the backend with curl
- On the host machine, use powershell to run the following command to test the backend server:
    - If successful, it should show `Hey there` in the terminal (put your droplet's IP address in place of `<ip-address>`).
`curl http://<ip-address>/hey`
e.g. `curl http://64.23.250.17/hey`

- On the host machine, use powershell to run the following command to test the backend server:
    - If successful, it should show `{"message": "Hello from your server"}` in the terminal.
    - replace the `<ip-address>` with your droplet's IP address.
```bash
curl -X POST -H "Content-Type: application/json" \
  -d '{"message": "Hello from your server"}' \
  http://<ip-address>/echo
```
  
e.g.
```bash
curl -X POST -H "Content-Type: application/json" \
  -d '{"message": "Hello from your server"}' \
  http://64.23.250.17/echo
``` 