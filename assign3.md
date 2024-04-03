
# nginx-2420 readMe

- download nginx: 

`sudo pacman -S nginx`

- if asked “Proceed with installation? [Y/n]”, type “Y” and press enter 

- download vim:

`sudo pacman -S vim`

- if asked “Proceed with installation? [Y/n]”, type “Y” and press enter 

- make the project root directory (name of directories must be as follows based on instructions):
`sudo mkdir -p /web/html/nginx-2420` 

- create a new index.html file: 
`sudo vim /web/html/nginx-2420/index.html`

- hit “i” on the keyboard and add the following to the index.html file:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>2420</title>
    <style>
        * {
            color: #db4b4b;
            background: #16161e;
        }
        body {
            display: flex;
            align-items: center;
            justify-content: center;
            height: 100vh;
            margin: 0;
        }
        h1 {
            text-align: center;
            font-family: sans-serif;
        }
    </style>
</head>
<body>
    <h1>All your base are belong to us</h1>
</body>
</html>
```

- exit vim after saving: `esc :wq`
- make a sites-available directory in nginx (mandatory naming):
`sudo mkdir /etc/nginx/sites-available`
- make a sites-enabled directory in nginx (mandatory naming):
`sudo mkdir /etc/nginx/sites-enabled`
-  make a new configuration file in the sites-available directory (name of file must be based on instructions: nginx-2420):
`sudo vim /etc/nginx/sites-available/nginx-2420.conf`
-  hit “i” on the keyboard and add the following to the
configuration file:
```bash 
server { 
    listen 80; 
    root /web/html/nginx-2420;
 }
 ```
- save the file: `esc :wq`
- edit the nginx configuration file:
`sudo vim /etc/nginx/nginx.conf`
-  add the following to the end of the http block in the
configuration file:
`include /etc/nginx/sites-enabled/*;`
- save the file: `esc :wq`
- create a symbolic link to the sites-enabled directory:
`sudo ln -s /etc/nginx/sites-available/nginx-2420.conf /etc/nginx/sites-enabled`
-  start the nginx service:
`sudo systemctl start nginx`
- enable the nginx service to run at boot:
`sudo systemctl enable nginx`
-  get the ip address of the droplet (the number following right
after inet, excluding the /20 under eth0):
`ip addr show`
-  open a web browser and type in the ip address of the droplet (ex:
http://64.23.250.17)