###nginx-2420
1. download nginx:
```sudo pacman -S nginx```
2. if asked "Proceed with installation? [Y/n]", type "Y" and press enter
3. make the project root directory:
```sudo mkdir -p /web/html/nginx-2420```
4. create a new index.html file:
```sudo vim /web/html/nginx-2420/index.html```
5. hit "i" on the keyboard and add the following to the index.html file:
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
6. exit vim after saving:
```esc :wq```
<!-- 7.make conf.d directory in nginx
```sudo mkdir /etc/nginx/conf.d``` -->

7. make a sites-available directory in nginx:
```sudo mkdir /etc/nginx/sites-available```
8. make a sites-enabled directory in nginx:
```sudo mkdir /etc/nginx/sites-enabled```
9. make a new configuration file in the sites-available directory:
```sudo vim /etc/nginx/sites-available/nginx-2420.conf```
10. hit "i" on the keyboard and add the following to the configuration file:
```bash
server {
        listen 80;
        root /web/html/nginx-2420;
}```
11. save the file:
```esc :wq```
12. edit the nginx configuration file:
```sudo vim /etc/nginx/nginx.conf```
13. add the following to the end of the http block in the configuration file:
```include /etc/nginx/sites-enabled/*;```
14. save the file:
```esc :wq```
15. create a symbolic link to the sites-enabled directory:
```sudo ln -s /etc/nginx/sites-available/nginx-2420.conf /etc/nginx/sites-enabled```

13. start the nginx service:
```sudo systemctl start nginx```
15. enable the nginx service to run at boot:
```sudo systemctl enable nginx```
16. get the ip address of the droplet (the number following right after inet, excluding the /20 under eth0):
```ip addr show```
17. open a web browser and type in the ip address of the droplet (ex: http://64.23.250.17)

