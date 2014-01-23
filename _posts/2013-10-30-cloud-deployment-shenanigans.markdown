---
layout: post
title:  "Cloud deployment shenanigans"
date:   2013-10-30 10:18:00
categories: azure
---

More cloud fun today! I was attempting to deploy a new .NET 4.5.1 cloud service this morning, but an error stopped me in my tracks:
 
<strong>The feature named NetFx451 that is required by the uploaded package is not available in the OS * chosen for the deployment.</strong>
 
I figured its to do with the default OS version that Azure is trying to create for my project (I'm guessing 2012), but how on earth do you change the version?
 
Turns out, you can't seem to access it from Visual Studio's GUI, nor can you change it on the cloud service container in Azure (you can change the OS version <i>after</i> you deploy, but this isn't much good if you can't deploy in the firstplace).
 
A bit of Googling lead me to a related post, and it seems that the only way to do it is to open your Cloud Service Configuration File (.cscfg), and modify the &lt;ServiceConfiguration/&gt; to use the new <i>OS Family 4</i> setting (either modify it, or add the attribute):

{% highlight ruby %}
osFamily="4"
{% endhighlight %} 

When you deploy (successfully now, I hope) you'll be allocated a Windows Server 2012 R2 VM image supporting .NET 4.5.1