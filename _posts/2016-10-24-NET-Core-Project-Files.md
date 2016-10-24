---
layout: post
title: Using Project Files in .NET Core Projects
description: Project files have evolved in .NET Core 1.0.  See how they have changed and what is next.  
imgurl: images/blog-banner.jpg
tags: [dotnet, visualstudio]
---

Before .NET core, project files are made up of complex XML which is automatically generated using Visual Studio. Individual files were listed, including build steps for each file. This was arduous and required Visual Studio to keep files up to date. MS Build natively support these files, but other build engines and services didn't always provide support.

While .NET Core 1.0 runtime has been [released](https://blogs.msdn.microsoft.com/dotnet/2016/06/27/announcing-net-core-1-0/) for general availability, it is important to understand that the tooling is still in preview mode. That is, while the platform is production ready, the tools to build the software are still in preview.

The project file is technically under tooling, as it is used to maintain a project and direct build tools.

Web application projects and database projects are two examples of projects which contain a large number of files, and managing them individually becomes difficult to maintain. The files still need to be part of the project (e.g. include in source control or package up).

To solve some of these issues, the preview of .NET Core (and is still supported) included a [project.json](https://docs.microsoft.com/en-us/dotnet/articles/core/tools/project-json) file. NPM (Node Package Manager) uses project.json format, which I believe is the basis for .NET core project files.

Project.json files are very easy to maintain by hand, allowing wildcard file inclusions and support JavaScript framework and build tools (e.g. gulp and bower) very well.  However they were a complete step away from existing project files for .NET projects.

Here is an example of a project.json file:

```
{
  "version": "0.1-alpha-*",
  "compilationOptions": {
    "warningsAsErrors": true
  },
  "code": ["**/*.cs", "/Shared/*.cs"],
  "frameworks": {
    "dnx451": {
      "frameworkAssemblies": {
        "System.Runtime": "",
        "System.Collections": ""
      }
    },
    "dnxcore50": {
      "dependencies": {
        "System.Collections": "4.0.0.0",
        "System.Collections.Concurrent": "4.0.0.0",
        "System.ComponentModel": "4.0.0.0",
        "System.Console": "4.0.0.0"
      }
    }
  }
}
```

Microsoft have [announced](https://blogs.msdn.microsoft.com/dotnet/2016/05/23/changes-to-project-json/) they are going to meet back in the middle. Project files were going to revert to XML files (supporting MSBuild), but be simplified to allow easy maintenance, and allow wildcard file inclusions.

> NOTE: While Microsoft have announced they are planning to move back to MSBuild project files, they recommend continuing the project.json file format in the meantime, with the proviso of a migration path when the new tooling is released.

Microsoft have given this a lot of thought, and I believe they have made the right decision to maintain consistent support with MSBuild, while simplifying the structure to make integration much easier. A simpler file structure will solve 80% of issues, and make integration with other tooling easier.

[Apparently](https://www.infoq.com/news/2016/07/NETCore-roadmap) we will see the project file changes released in the fourth quarter of 2016, or first quarter of 2017. I would say it would be around the time of Visual Studio "15" release.
