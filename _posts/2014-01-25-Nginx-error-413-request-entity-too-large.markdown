---
layout: post
title:  "Nginx Error - 413 Request Entity Too Large"
date:   2014-01-24 10:18:00
categories: Nginx, 413 Request Entity Too Large
---

If you're getting 413 Request Entity Too Large errors trying to upload files with Nginx, you need to increase the size limit in the site configuration file (normally in <i>/etc/nginx/sites-available</i>). Add <i>client\_max\_body\_size</i> inside the server section, where the value is the size (in megabytes) that you want to allow.

{% highlight ruby %}
server {
	#your other site configuration
	client_max_body_size 20M;
}
{% endhighlight %} 