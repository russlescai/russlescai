---
layout: post
title: Use nginx as a Reverse Proxy for IIS and .NET Core
description: IIS and Kestrel used for development, and only listens to local requests. Nginx to the rescue!
imgurl: images/blog-banner.jpg
tags: [iis, nginx]
---

[IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) is usually installed with Visual Studio, and is used to develop web applications and debug locally. It is designed to be used for development purposes, and is configured to only listen to local requests. This makes it difficult to test websites from another PC.

[Kestrel](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/servers/kestrel?view=aspnetcore-2.1&tabs=aspnetcore2x) is included with .NET Core SDK, and also provides a local web hosting to allow debugging.

It is possible to update configuration files to allow IIS Express to listen for all requests, but there is a (in my opinion) much easier way.

[Nginx](https://nginx.org/en/) is a proxy service which can listen for HTTP requests and tunnel them to the IIS Express service. This is also a commonly described [approach](https://docs.asp.net/en/latest/publishing/linuxproduction.html#why-use-a-reverse-proxy-server) for developing with ASP.NET Core (using Kestrel).  It has a lot of configuration and features, including SSL termination and many others. The instructions below are basic and targeted for IIS Express.

To setup nginx, first download and install [nginx for Windows](https://nginx.org/en/download.html) (I used the stable version). Then, open `conf\nginx.conf` in a text editor. There are default settings provided, however paste the following in to save time.

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
