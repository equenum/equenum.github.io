---
title: Create and publish your custom .NET project templates
date: 2024-04-18 13:00:00 +0400
categories: [software-development]
tags:
  [dotnet, csharp, visual-studio, vs-code, project-templates, nuget, dotnet-new]
image:
  path: assets/img/posts/20240418/project-template-thumbnail.webp
  lqip: assets/img/posts/20240418/project-template-lqip.webp
---

In this blog I composed a set of useful tips, samples and materials on how to create, test, use and publish ([Nuget.org](https://www.nuget.org/)) your own custom .NET project templates. If you are looking for a recent example, feel free to check out one of my own templates on [Github](https://github.com/equenum/dotnet_project_templates/tree/main/MightyMentorConsole.Template). You can find more details about the template in this [dedicated blog entry]({% post_url 2024-04-13-mighty-mentor-console-app-template %}).

## Work with templates

### .NET Cli

- [dotnet new](https://learn.microsoft.com/en-us/dotnet/core/tools/dotnet-new): creates a new project, configuration file, or solution based on the specified template.
- [dotnet new search](https://learn.microsoft.com/en-us/dotnet/core/tools/dotnet-new-search): searches for the templates supported by `dotnet new` on [NuGet.org](https://www.nuget.org/).
- [dotnet new list](https://learn.microsoft.com/en-us/dotnet/core/tools/dotnet-new-list): lists available templates to be run using `dotnet new`.
- [.NET default templates for dotnet new](https://learn.microsoft.com/en-us/dotnet/core/tools/dotnet-new-sdk-templates).
- [Available templates for dotnet new](https://github.com/dotnet/templating/wiki/Available-templates-for-dotnet-new).

### Visual Studio

- [Create a new project in Visual Studio](https://learn.microsoft.com/en-us/visualstudio/ide/create-new-project?view=vs-2022).
- [.NET CLI Templates in Visual Studio](https://devblogs.microsoft.com/dotnet/net-cli-templates-in-visual-studio/).

## Default templates

- [Common project (classlib, console) and item templates](https://github.com/dotnet/templating).
- [ASP.NET and Blazor templates](https://github.com/dotnet/aspnetcore).
- [WPF templates](https://github.com/dotnet/wpf).
- [Other](https://github.com/dotnet/templates).

## Custom templates

### Articles and documentation

- [.Net templating Wiki on GitHub](https://github.com/dotnet/templating/wiki).
- [How to create your own templates for dotnet new](https://devblogs.microsoft.com/dotnet/how-to-create-your-own-templates-for-dotnet-new/) by Sayed Ibrahim Hashimi.
- [Custom templates for dotnet new](https://learn.microsoft.com/en-us/dotnet/core/tools/custom-templates) on Microsoft Learn.
- [Create custom project and item templates](https://learn.microsoft.com/en-us/visualstudio/extensibility/creating-custom-project-and-item-templates?view=vs-2022) on Microsoft Learn.
- [Create project templates](https://learn.microsoft.com/en-us/visualstudio/ide/how-to-create-project-templates?view=vs-2022) on Microsoft Learn.
- [Tutorial: Create a template package](https://learn.microsoft.com/en-us/dotnet/core/tutorials/cli-templates-create-template-package) on Microsoft Learn.

### Videos

- [How to create your own project templates in .NET](https://www.youtube.com/watch?v=rdWZo5PD9Ek&t=5s) by Nick Chapsas.
- Microsoft series with Sayed Ibrahim Hashimi:
  - [Creating your own .NET Project Templates](https://www.youtube.com/watch?v=H_pqfeRgTYw).
  - [Create .NET Core Projects with the Command Line](https://www.youtube.com/watch?v=rZFIbbxsGmc).
  - [Use an Existing .NET Core Project Template](https://www.youtube.com/watch?v=XRrBFXN02w8).
  - [Create a .NET Core Project Template](https://www.youtube.com/watch?v=GDNcxU0_OuE).
  - [Create a .NET Core Project Template for Visual Studio](https://www.youtube.com/watch?v=2hpNFrY_faI).
  - [Add a Parameter to a .NET Core Project Template](https://www.youtube.com/watch?v=o4lE_wcdmFo).
  - [Troubleshooting .NET Core Project Templates](https://www.youtube.com/watch?v=Sbk87OO73jQ).

### Code examples

- [Template samples](https://github.com/sayedihashimi/template-sample/blob/main/README.md) by Sayed Ibrahim Hashimi.
- [.NET Boxed templates](https://github.com/Dotnet-Boxed).
- [.NET Template Samples](https://github.com/dotnet/templating/tree/main/dotnet-template-samples).
- Refer to the [Default templates section](#default-templates) for official examples.
- Find community templates: Go to [Nuget.org](https://www.nuget.org/) => Search (set Package type to 'Template').

### Implementation

1. Write code, implement template project.
2. [Define template configuration file (`template.json`)](https://github.com/equenum/dotnet_project_templates/blob/main/MightyMentorConsole.Template/content/.template.config/template.json). Find documentation [here](https://github.com/dotnet/templating/wiki/Reference-for-template.json).
3. Define [additional files](https://github.com/dotnet/templating/wiki/Reserved-Aliases) to override `template.json` or define additional presentation configurations for .NET CLI ([`dotnetcli.host.json`](https://github.com/equenum/dotnet_project_templates/blob/main/MightyMentorConsole.Template/content/.template.config/dotnetcli.host.json)) and Visual Studio ([`ide.host.json`](https://github.com/equenum/dotnet_project_templates/blob/main/MightyMentorConsole.Template/content/.template.config/ide.host.json)) hosts.
4. Use `template.json` parameters [in classes](https://github.com/equenum/dotnet_project_templates/blob/main/MightyMentorConsole.Template/content/MightyMentorConsole.Template/AppRunner.cs).
5. Use `template.json` parameters [in `.csproj` files.](https://github.com/equenum/dotnet_project_templates/blob/main/MightyMentorConsole.Template/content/MightyMentorConsole.Template/MightyMentorConsole.Template.csproj)

## Build package

- [dotnet pack](https://learn.microsoft.com/en-us/dotnet/core/tools/dotnet-pack): packs the code into a NuGet package.
- [pack command (NuGet CLI)](https://learn.microsoft.com/en-us/nuget/reference/cli-reference/cli-ref-pack): creates a NuGet package based on the specified `.nuspec` or `.csproj` file.
- [Adding nuget pack as a msbuild target](https://github.com/NuGet/Home/wiki/Adding-nuget-pack-as-a-msbuild-target).
- [NuGet pack and restore as MSBuild targets](https://learn.microsoft.com/en-us/nuget/reference/msbuild-targets).
- [Create a NuGet package with the dotnet CLI](https://learn.microsoft.com/en-us/nuget/create-packages/creating-a-package-dotnet-cli).
- [Create a NuGet package using MSBuild](https://learn.microsoft.com/en-us/nuget/create-packages/creating-a-package-msbuild).
- [`.nuspec` vs `.csproj` for making nuget packages](https://github.com/NuGet/docs.microsoft.com-nuget/issues/1328).

Find an example here: [`MightyMentorConsole.Template.csproj`](https://github.com/equenum/dotnet_project_templates/blob/main/MightyMentorConsole.Template/MightyMentorConsole.Template.csproj).

## Install and test locally

- [Manage .NET project and item templates](https://learn.microsoft.com/en-us/dotnet/core/install/templates?pivots=os-windows).
- [How to test template changes locally by Sayed Ibrahim Hashimi](https://github.com/sayedihashimi/template-sample/blob/main/README.md#how-to-test-template-changes-locally).
- [Install a template package from a file system directory](https://learn.microsoft.com/en-us/dotnet/core/tools/custom-templates#to-install-a-template-package-from-a-file-system-directory).

## Publish

- [Publish NuGet packages](https://learn.microsoft.com/en-us/nuget/nuget-org/publish-a-package).
- [Creating and Publishing a NuGet Package](https://www.youtube.com/watch?v=E0rPteTWxYQ).
