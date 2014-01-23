---
layout: post
title:  "An error occurred while receiving the HTTP response"
date:   2012-03-06 10:18:00
categories: WCF
---

I was having a strange issue when sending a larger-than-normal payload to a WCF service hosted over HTTP:

<i>System.ServiceModel.CommunicationException: An error occurred while receiving the HTTP response to http://localhost:1487/Services/EmailService.svc. This could be due to the service endpoint binding not using the HTTP protocol. This could also be due to an HTTP request context being aborted by the server (possibly due to the service shutting down).</i>

The payload was a collection of email objects, complete with serialised attachments. Figuring it was due to the size of the message, I tried increased size of <i>maxReceivedMessageSize</i> to "1048576000" (100MB!), however I was still unable to send more than a couple of messages at a time.

After a bit of head scratching the penny dropped - I'm hosting the service in IIS (the development environment is IIS Express), and by default ASP.NET imposes a 4MB request limit.

This is pretty simple to change, although there are security considerations to bear in mind. However, our service will only be accessible internally, so I'm pretty safe increasing <i>maxRequestLength</i> to a rather large 500MB in the web.config.

A simple fix to a not-very-intuitive error message.