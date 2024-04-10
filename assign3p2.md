# Firewall

# Enabling the firewall on the server
- Update the system with the latest packages.
`sudo pacman -Syu`
    - if prompted, follow instructions to update the system (e.g. type “Y” and press enter)


- Download the package UFW (Uncomplicated Firewall)
    - UFW is a fire wall program that manages nftables or iptables.
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
sudo ufw allow 22
```
- You can limit the rate of ssh connections to prevent brute force attacks with the following command (6 attempts in 30 seconds will deny an incoming address):
`sudo ufw limit SSH`

- Allow http connections since we will be using a web server with either of the following commands (they are equivalent since SSH's default port is 22):
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