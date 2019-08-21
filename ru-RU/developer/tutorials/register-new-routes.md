---
title: How do I register new routes?
author: AndreiMaz
uid: developer/tutorials/register-new-routes
---

# Register new routes

ASP.NET Core routing is responsible for mapping incoming browser requests to particular MVC controller actions. You can find more information about routing here. NopCommerce has an IRouteProvider interface which is used for route registration during application startup. All core routes are registered in the RouteProvider class located in the `Nop.Web` project.

```csharp
public partial class RouteProvider : IRouteProvider
{
    public void RegisterRoutes(IRouteBuilder routeBuilder)
    {
        //home page
        routeBuilder.MapLocalizedRoute("HomePage", "", new { controller = "Home", action = "Index" });
    }
}
```

You can create as many RouteProvider classes as you need. For example, if your plugin has some custom routes which you want to register, then create a new class implementing the IRouteProvider interface and register the routes specific to your new plugin.