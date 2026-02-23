# Best Method: Nginx Maintenance Page
## I will configure Nginx to show:
## -------------------------------------------------
“System under maintenance. Please try again later.”
## -------------------------------------------------
## Step 1 — Create maintenance page
Create a simple HTML file:
```
sudo nano /var/www/html/maintenance.html

```
## pest the html there.
```
<!DOCTYPE html>
<html>
<head>
    <title>Maintenance</title>
    <style>
        body {
            background: #0d1b2a;
            color: white;
            text-align: center;
            padding-top: 100px;
            font-family: Arial, sans-serif;
        }
        h1 { font-size: 40px; }
        p { font-size: 18px; }
    </style>
</head>
<body>
    <h1>NTTN OPS System</h1>
    <p>The system is currently under maintenance.</p>
    <p>Please try again in a few minutes.</p>
</body>
</html>

```
## Save and exit.

# Step 2 — Modify Nginx config

# Open your site config:

```
sudo nano /etc/nginx/sites-available/nttn

```
# Modify it to this:
```
server {
    listen 80;
    server_name _;

    location / {
        proxy_pass http://127.0.0.1:8000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;

        error_page 502 503 504 /maintenance.html;
    }

    location = /maintenance.html {
        root /var/www/html;
        internal;
    }
}

```
## Save it and exit.

# Step 3 — Reload nginx
```
sudo nginx -t

```
## Then
```
sudo systemctl reload nginx
```
# finally Test it
## Stop your backend:
```
sudo systemctl stop Application

```
## Now open:
http://192.168.xx.xx
