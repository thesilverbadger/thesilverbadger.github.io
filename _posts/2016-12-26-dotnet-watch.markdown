---
layout: post
title:  "Troubleshooting dotnet watch"
date:   2016-12-26 13:00:00
categories: DOTNETCore
---

I've been following .NET Core since its inception, and I love the tooling support on macOS via VS Code.

Over the last couple of days, I've been following a short tutorial on .NET Core and Angular 2 (which you can find here: http://angularfirst.com/your-first-angular-2-asp-net-core-project-in-visual-studio-code-part-1/), and 
I've learnt a few things along the way.

However, I ran into a problem getting the dotnet watch command to work - I've had it working in the past, but on this tutorial project I kept getting the error 

> No executable found matching command "dotnet-watch"

Turns out I'd put the `tools` section in the wrong place, and rather than adding it to the root at the same level as `frameworks`, I'd added it inside `frameworks` and below the dependencies section within that.

Moving the section to the same level and the command appears.

```javascript
{
  "version": "1.0.0-*",
  "buildOptions": {
    "debugType": "portable",
    "emitEntryPoint": true
  },
  "dependencies": {
    "Microsoft.NETCore.App": {
      "type": "platform",
      "version": "1.1.0"
    },
    "Microsoft.AspNetCore.Hosting": "1.1.0",
    "Microsoft.AspNetCore.Server.Kestrel": "1.1.0",
    "Microsoft.AspNetCore.StaticFiles": "1.1.0"
  },
  "frameworks": {
    "netcoreapp1.1": {
      "dependencies": {
      },
      "imports": "dnxcore50"
    }
  },
  "tools": {
    "Microsoft.DotNet.Watcher.Tools": "1.1.0-preview4-final"
  }
}

```
