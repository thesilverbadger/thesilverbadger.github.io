---
layout: post
title:  "SmtpClient throws InvalidOperationException when using SpecifiedPickupDirectory"
date:   2012-02-22 10:18:00
categories: SmtpClient
---

I was asked today to amend the settings for a bulk email application I've recently written - using the <mailSettings> element in the web.config, I was specifying an SMTP server, however the requirement now is to specify a pickup directory, and to allow IIS to handle the email sending process.

Pretty simple change I though - amend the deliveryMethod to <i>SpecifiedPickupDirectory</i> and provide a &lt;specifiedPickupDirectory/&gt; element with the path to the pickup directory that had already been configured:


<img src="/attachments/smtp_settings_incorrect.png" alt="smtp settings incorrect"/>

However causes an exception to be thrown.  After some head scratching and a bit of Googling, it appears that this is a bug in the .NET 4.0 framework.  When the SMTP Client object is defined in a using block

<img src="/attachments/newSmtpClient.png" alt="SmtpClient"/>

you must define the &lt;network/&gt; attribute after the &lt;specifiedPickupDirectory/&gt;, even though you donâ€™t want to use it:

<img src="/attachments/smtp_settings_correct.png" alt="smtp settings correct"/>
