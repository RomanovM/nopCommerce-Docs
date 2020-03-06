---
title: nopCommerce Documentation
uid: ru/new-index
author: git.AndreiMaz
contributors: git.AndreiMaz
---

# nopCommerce Documentation

nopCommerce is an open source ecommerce software that contains both a catalog frontend and an administration tool backend. nopCommerce is a fully customizable shopping cart, stable, secure and extendable. From downloads to documentation, [nopCommerce.com](https://www.nopCommerce.com) offers a comprehensive base of information, resources, and support to the nopCommerce community.

# How to write a plugin for nopCommerce 3.90 (and previous versions)

> In computing, a plug-in (or plugin) is a set of software components that add specific abilities to a larger software application (Wikipedia).

Plugins are used to extend the functionality of nopCommerce. nopCommerce has several types of plugins. For example, payment methods (such as PayPal), tax providers, shipping method computation methods (such as UPS, USP, FedEx), widgets (such as 'live chat' block), and many others. nopCommerce is already distributed with many different plugins. You can also search various plugins on the [nopCommerce official site](https://www.nopcommerce.com/marketplace) to see if someone has already created a plugin that suits your needs. If not, this article will guide you through the process of creating your own plugin.

## The plugin structure, required files, and locations

1. You should also created a web.config file and ensure that it's copied to output. Just copy it from any existing plugin.

    > [!IMPORTANT]
    > 
    > ВПЕРЕД  are set to "False" (do not copy)

> [!IMPORTANT]
> 
> After each project build, clean the solution before making changes. Some resources will be cached and can lead to developer insanity.

## Handling requests. Controllers, models and views

- **Create the model**. Add a Models folder in the new plugin, and then add a new model class which fits your need.
- **Create the view**. Add a Views folder in the new plugin, then add a {Name} folder (where {Name} is your plugin name), and finally add a cshtml file named `Configure.cshtml`. Important note: for versions 2.00-3.30 the view should be marked as an embedded resource. And starting version 3.40 views, ensure that the "Build Action" property of the view file is set to "Content", and the "Copy to Output Directory" property is set to "Copy if newer".
- **Create the controller**. Add a Controllers folder in the new plugin, and then add a new controller class. A good practice is to name plugin controllers `{Group}{Name}Controller.cs`. For example, PaymentAuthorizeNetController. Of course it's not a requirement to name controllers this way (but just a recommendation). Then create an appropriate action method for configuration page (in admin area). Let's name it "Configure". Prepare a model class and pass it to the following view. For nopCommerce versions 2.00-3.30 you should pass embedded view path - "Nop.Plugin.{Group}.{Name}.Views. {Group}{Name}.Configure". And starting nopCommerce version 3.40 you should pass physical view path - `~/Plugins/{PluginOutputDirectory}/Views/{ControllerName}/Configure.cshtml`. For example, open Authorize.NET payment plugin and look at its implementation of PaymentAuthorizeNetController.

    > [!TIP]
    > 
    > - The easiest way to complete the steps described above is opening any other plugin and copying these files into your plugin project. Then just rename appropriate classes and directories.
    > - If you want to limit access to a certain action method of the controller to administrators (store owners), then just mark it with [AdminAuthorize] attribute.

    For example, the project structure of Authorize.NET plugin looks like the image below

    ![p3](_static/how-to-write-plugin-3.90/write_plugin_3.90_3.jpg)

## Routes

## Handling "Install" and "Uninstall" methods

This step is optional. Some plugins can require additional logic during plugin installation. For example, a plugin can insert new locale resources. So open your IPlugin implementation (in most case it'll be derived from BasePlugin class) and override the following methods:

- Install. This method will be invoked during plugin installation. You can initialize any settings here, insert new locale resources, or create some new database tables (if required).
- Uninstall. This method will be invoked during plugin uninstallation.

> [!IMPORTANT]
> 
> If you override one of these methods, do not hide its base implementation.

For example, the project structure of Authorize.NET plugin looks like the image below

```csharp
public override void Install()
{
    var settings = new AuthorizeNetPaymentSettings()
    {
        UseSandbox = true,
        TransactMode = TransactMode.Authorize,
        TransactionKey = "123",
        LoginId = "456"
    };
    _settingService.SaveSetting(settings);
    base.Install();
}
```

> [!TIP]
> 
> The list of installed plugins is located in `\App_Data\InstalledPlugins.txt`. The list is created during installation.

## Upgrading nopCommerce may break plugins

Some plugins may become outdated and no longer work with the newer version of nopCommerce. If you have issues after upgrading to the newer version, delete the plugin and visit the official nopCommerce website to see if a newer version is available. Many plugin authors will upgrade their plugins to accommodate the newer version, however, some will not and their plugin will become obsolete with the improvements in nopCommerce. But in most cases, you can simply open an appropriate `plugin.json` file and update **SupportedVersions** field.

## Conclusion

Hopefully this will get you started with nopCommerce and prepare you to build more elaborate plugins.

## Table of content
* [User guide](xref:en/user-guide/index)
* [Developers guide](xref:en/developer/index)
