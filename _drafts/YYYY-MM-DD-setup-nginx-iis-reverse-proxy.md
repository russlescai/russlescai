---
layout: post
title: Use Nginx as a Reverse proxy for IIS Express
imgurl: images/blog-banner.jpg
tags: [iis, nginx]
---

IIS Express is usually installed with Visual Studio, and is used to develop web applications and debug locally. It is designed to be used for development purposes, and is configured to only listen to local requests. This makes it difficult to test websites from another PC. It is also common for development environments to be hosted in VMs, which prevents them from being accessed from the host.

It is possible to update configuration files to allow IIS Express to listen for all requests, but there is a (in my opinion) much easier way.

Nginx is a forward proxy service which can listen for requests and tunnel them to the IIS Express service. This is also a commonly described approach for developing with ASP.NET Core.

To setup Nginx, first download and install Nginx for Windows. Then, open `conf\nginx.conf` in a text editor. There are default settings provided, however paste the following in to save time.

```
events {
  worker_connections 1024;
}

http {
  server {
    listen 50000;

    location / {
      proxy_set_header Host localhost;
      proxy_pass http://localhost:2930/;
    }
  }
}
```

Replace port `2930` with the port configured in your web application project. Optionally, change the port `50000` to a different port if you prefer.

The configuration simply passes requests to IIS Express URL, setting the expected HTTP Header Host to `localhost`.  This is important, otherwise you will receive a HTTP 400 error.

Double click on `nginx.exe` or run in a command prompt. The first time you run it, you will be asked which networks you want to access. Expose to domain and private networks.  

That's it! You should be able to access it via http://localhost:50000 and http://servername:50000 (even from another PC).

If you are having issues, you can check the `logs\access.log` or `logs\error.log` files.

Hopefully this helps, especially for testing scenarios or being more efficient with development VMs.
