---
title: Meet MightyMentor .NET Console app template
date: 2024-04-13 13:00:00 +0400
categories: [software-development]
tags:
  [dotnet, csharp, visual-studio, vs-code, project-templates, nuget, dotnet-new]
image:
  path: assets/img/posts/20240413/proj_temp_thumbnail.webp
  lqip: assets/img/posts/20240413/proj_temp_lqip.png
---

Welcome to this blog where I would like to share a custom .NET project template I implemented and published recently. Hopefully, you will find it useful.

While conducting dozens of hours of presentations, workshops, and live coding sessions for beginner ASP.NET developers during the last several months, I constantly found myself recreating the same demo project over and over again. It was a simple console application with Dependency Injection and application settings support. It proved itself to be very simple, but capable. I like it because it allowed me to demo different C#/.Net concepts without forcing the listeners into learning more advanced .NET/ASP.NET workloads, like WebAPI and others right away and instead start with something more basic.

After manually recreating this project from scratch more times than I feel comfortable admitting, I finally decided to put it into a template. While I was at it, I figured why not share it with other fellow .NET developers. So, here is how MightyMentor template was born. You can find it on [Nuget.org](https://www.nuget.org/packages/MightyMentorConsole.Template) and [GitHub](https://github.com/equenum/dotnet_project_templates/tree/main/MightyMentorConsole.Template).

I also published a dedicated blog entry where I composed a list of useful materials and examples I found as part of my research. You can find it here.

## MightyMentorConsole.Template

A simple .Net Console App template tailored for better workshops, live coding sessions, and knowledge-sharing experiences. Can be useful to mentors/trainers for showcasing not only basic C# syntax and features but also more advanced concepts, like Dependency Injection, application settings, asynchronous and parallel execution, and so on. Offers a set of useful built-in utilities, making working with the console easier and more visually precise. The utilities and project structure are kept simple so that they are easy to understand for beginner developers.

### Utilities

#### ColorConsole

Offers a set of methods that allow for writing to console in color for better visibility (wraps around System.Console and implements all the most often used methods).

```csharp
ColorConsole.WriteLine(); // line terminator

ColorConsole.WriteSuccess("Success !"); // writes in green
ColorConsole.WriteError("Error =("); // writes in red
ColorConsole.WriteWarning("Warning !"); // writes in dark yellow

ColorConsole.WriteLine(
    text: "Write text in a specific color",
    color: ConsoleColor.DarkYellow);

// writes '*****Header*****'
ColorConsole.WriteSeparationLine(
    header: "Header",
    paddingSize: 5,
    paddingChar: '*',
    color: ConsoleColor.DarkGreen);
```

#### ConsoleHelper

Offers a set of methods that allow for simpler and cleaner console input parsing.

```csharp
// reads user input form the console.
// if invalid, prompts to repeat; else, converts to the specified type.
var number = ConsoleHelper.GetInput<int>(
    prompt: $"Please enter your number: ",
    errorMessage: "Invalid input! Please try again: ");
```

### Parameters

#### Visual Studio:

- **IncludeAppsettings:** Includes `appsettings.json` and `appsettings.Development.json` (defaults to `true`).
- **IncludeUtils:** Includes template specific utility classes (defaults to `true`).

#### .Net CLI:

```shell
dotnet new mentor-console -h
```

### Configurations

#### Dependency Injection

```csharp
private static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureServices((context, services) =>
        {
            // configure DI services here
            services.AddTransient<AppRunner>();
        });
```

#### Console configurations and access to `appsettings.json` via `IConfiguration`

```csharp
private static void Configure(IHost host)
{
    // integrate appsettings.json configs, environment variables, etc.
    var configuration = host.Services.GetService<IConfiguration>();

    // configure the console
    Console.Title = "MightyConsole.Template";
    Console.BackgroundColor = ConsoleColor.Black;
    Console.ForegroundColor = ConsoleColor.Gray;
    Console.SetWindowSize(120, 30);
}
```

### Install

The template is available in the Nuget.org package source.

Install via .Net CLI:

```shell
dotnet new install MightyMentorConsole.Template
```

### Usage

#### .Net CLI

```shell
dotnet new mentor-console
```

#### Visual Studio

- Go to 'Create a new project'.
- Search for `MightyMentor Console App`.

### Uninstall

```shell
dotnet new uninstall MightyMentorConsole.Template
```

### Notes

- The template targets `.Net8` and was tested with `Visual Studio 2022` on Windows and `VS Code + C# Dev Kit extensions` on Linux. It might not work or require adjustments for other configurations.
- When using the template with `Visual Studio 2022` consider ticking the 'Place solution and project in the same directory' checkbox, if you prefer reducing folder nesting. To do the same when using `.Net CLI` specify the output folder parameter as `-o .`.
- Please reach out or open an issue in case of feature suggestions, errors found, or any other form of feedback! Thanks!

## Useful URLs

- <https://www.nuget.org/packages/MightyMentorConsole.Template>
- <https://github.com/equenum/dotnet_project_templates/tree/main/MightyMentorConsole.Template>
