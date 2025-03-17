---
title: Injecting an EF DbContext instance into an IHostedService
date: 2025-02-07 13:00:00 +0400
categories: [software-development]
tags: [dotnet, csharp, dependency-injection, hosted-service]
image:
  path: assets/img/posts/common/ef-logo.png
  lqip: assets/img/posts/common/ef-logo-lqip.webp
---

## Problem

As we all know, there are hardly any APIs that do not use a database to persist their data. ASP.NET makes it easy to connect applications to databases via `DbContext`, part of `Entity Framework`.

While working on one of my personal projects, I have encountered the following error when trying to inject `DbContext` into a hosted background service:

```
Cannot consume scoped service 'Microsoft.EntityFrameworkCore.DbContextOptions' from singleton 'Microsoft.Extensions.Hosting.IHostedService'.
```

This makes sense since at the time I was using `AddDbContext()` to register my context, which by default registers a context as scoped. `IHostedServices` are singletons, hence the conflict.

## Solution

To fix the issue, I decided to use `AddDbContextFactory()` registration instead:

```csharp
builder.Services.AddDbContextFactory<MonitorDbContext>(options =>
{
    options.UseLazyLoadingProxies();
    options.UseNpgsql(builder.Configuration.GetConnectionString("ChangeMonitor"));
    options.UseSnakeCaseNamingConvention();
});
```

This method registers a singleton `IDbContextFactory` and a scoped `DbContext` which in turn allows us to produce instances of the second on demand via `CreateDbContext()`:

```csharp
...

private readonly IDbContextFactory<MonitorDbContext> _contextFactory;

public MonitorJobsRegistrationService(
  ...
  IDbContextFactory<MonitorDbContext> contextFactory)
{
  ...
  _contextFactory = contextFactory;
}

public async Task StartAsync(CancellationToken cancellationToken)
{
  await InitiateScheduler(cancellationToken);

  using (var context = _contextFactory.CreateDbContext())
  {
    var targetEntities = await context.Targets.ToListAsync(cancellationToken);

    if (targetEntities.Count > 0)
    {
      _logger.LogInformation("Existing targets found, count: {TargetCount}. Registering jobs...",
          targetEntities.Count);

      await _jobService.ScheduleAsync(targetEntities.Select(entity => entity.ToTarget()),
          cancellationToken);
    }
  }
}

...
```

The 2 types registered in `WebApplicationBuilder.Services`:

![Desktop View](assets/img/posts/20250207/web-app-builder-services.png){: width="893" height="486" }
_The 2 types registered in WebApplicationBuilder.Services_

The other nice thing about `AddDbContextFactory()` is that you can still inject scoped `DbContext` instances directly without using the factory:

```csharp
...

private readonly MonitorDbContext _context;
private readonly IMonitorJobService _jobService;

public ResourceService(
  MonitorDbContext context,
  IMonitorJobService jobService)
{
  _context = context;
  _jobService = jobService;
}

...
```

That’s pretty neat. It makes me wonder whether it's a good idea to just adopt it as a common practice and always use `AddDbContextFactory()` by default when first wiring up `Entity Framework` in new projects.

## Useful URLs

- <https://learn.microsoft.com/en-us/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontextfactory?view=efcore-9.0>
- <https://github.com/dotnet/efcore/blob/64512bea5f6f21661ee3a0a99fb9bfa96d44af88/src/EFCore/Extensions/EntityFrameworkServiceCollectionExtensions.cs#L737>
- <https://github.com/equenum/webpage_change_monitor/blob/d4f6e02f97f77bb0e6ba8abe5b56e0ad70b399ed/api/src/WebPageChangeMonitor.Api/Infrastructure/MonitorJobsRegistrationService.cs>
- <https://github.com/equenum/webpage_change_monitor/blob/d4f6e02f97f77bb0e6ba8abe5b56e0ad70b399ed/api/src/WebPageChangeMonitor.Api/Services/Controller/ResourceService.cs#L19>
- <https://github.com/equenum/webpage_change_monitor/blob/d4f6e02f97f77bb0e6ba8abe5b56e0ad70b399ed/api/src/WebPageChangeMonitor.Api/Program.cs#L62>
