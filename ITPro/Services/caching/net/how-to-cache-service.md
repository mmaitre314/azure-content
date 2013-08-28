<properties linkid="manage-services-net-cache-how-to-cache-service" urlDisplayName="Caching" pageTitle="How to use the Cache Service - Windows Azure .NET feature guide" metaKeywords="Windows Azure cache, Windows Azure caching, Azure cache, Azure caching, Azure store session state, Azure cache .NET, Azure cache C#" metaDescription="Learn how to use Windows Azure Cache Service. The samples are written in C# code and use the .NET API." metaCanonical="" disqusComments="1" umbracoNaviHide="0" />

# How to Use Windows Azure Cache Service (Preview)

This guide shows you how to get started using 
**Windows Azure Cache Service (Preview)**. The samples are written in C\# code and
use the .NET API. The scenarios covered include **creating and configuring a cache**, **configuring cache clients**, **adding and removing
objects from the cache, storing ASP.NET session state in the cache**,
and **enabling ASP.NET page output caching using the cache**. For more
information on using Windows Azure Cache, refer to the [Next Steps][] section.

## Table of Contents

-   [What is Windows Azure Cache?][]
-	[Getting Started with Cache Service (Preview)]
	-	[Create the cache][]
	-	[Configure the cache][]
	-	[Configure the cache clients][]
-	[Working with caches][]
	-	[How To: Create a DataCache Object][]
	-   [How To: Add and Retrieve an Object from the Cache][]
	-   [How To: Specify the Expiration of an Object in the Cache][]
	-   [How To: Store ASP.NET Session State in the Cache][]
	-   [How To: Store ASP.NET Page Output Caching in the Cache][]
-   [Next Steps][]

<h2><a name="what-is"></a><span class="short-header">What is Windows Azure Cache?</span>What is Windows Azure Cache?</h2>

Windows Azure Cache Service (Preview) is a distributed, in-memory, scalable solution that enables you to build highly scalable and responsive applications by providing super-fast access to data.

Windows Azure Cache Service (Preview) includes the following
features:

-   Pre-built ASP.NET providers for session state and page output
    caching, enabling acceleration of web applications without having to
    modify application code.
-   Caches any serializable managed object - for example: CLR objects, rows, XML,
    binary data.
-   Consistent development model across both Windows Azure and Windows
    Server AppFabric.

Cache Service (Preview) gives you access to a secure, dedicated cache that is managed by Microsoft. A cache created using the Cache Service (Preview) is accessible from applications within Windows Azure running on Azure Web Sites, Web & Worker Roles and Virtual Machines.

Cache Service (Preview) is available in three tiers:

-	Basic - Shared cache in sizes from 128MB to 1GB
-	Standard - Dedicated cache in sizes from 1GB to 10GB
-	Premium - Dedicated cache in sizes from 5GB to 150GB

Each tier differs in terms of features and pricing. The features are covered later in this guide, and for more information on pricing, see [Cache Pricing Details][].

This guide provides an overview of getting started with Cache Service (Preview). For more detailed information on these features that are beyond the scope of this getting started guide, see [Overview of Windows Azure Cache Service (Preview)][].

<h2><a name="getting-started-cache-service"></a><span class="short-header">Getting Started with Cache Service (Preview)</span>Getting Started with Cache Service (Preview)</h2>

Getting started with Cache Service (Preview) is easy. To get started, you provision and configure a cache. Next, you configure the cache clients so they can access the cache. Once the cache clients are configured, you can begin working with them.

-	[Create the cache][]
-	[Configure the cache][]
-	[Configure the cache clients][]

<h2><a name="create-cache"></a><span class="short-header">Create the cache</span>Create a cache</h2>

To create a cache, first sign in to the [Management Portal][].

>If this is your first time working with Cache Service (Preview) then you need to request access to the Cache Service preview program. To sign up for the preview program, click **New**, **Data Services**, **Cache Preview**, **Preview Program**. Follow the prompts to request access to the Cache Service preview program, and when access is granted, proceed to the next steps.

Click **New**, **Data Services**, **Cache Preview**, **Quick Create**.

![NewCacheMenu][]

![QuickCreate][]

In **Endpoint**, enter a subdomain name to use for the cache endpoint. The endpoint must be a string between six and twenty characters, contain only lowercase numbers and letters, and must start with a letter.

In **Region**, select a region for the cache. For the best performance, create the cache in the same region as the cache client application.

In **Subscription**, select the Windows Azure subscription that you want to use for the cache.

>If your account has only one subscription, it will be automatically selected and the Subscription drop-down will not be displayed.

**Cache Offering** and **Cache Memory** work together to determine the size of the cache. Cache Service (Preview) is available in the three following tiers.

-	Basic - Shared cache in sizes from 128MB to 1GB in 128MB increments, with one default named cache
-	Standard - Dedicated cache in sizes from 1GB to 10GB in 1GB increments, with support for notifications and up to ten named caches

-	Premium - Dedicated cache in sizes from 5GB to 150GB in 5GB increments, with support for notifications, high availability, and up to ten named caches


Choose the **Cache Offering** and **Cache Memory** that meets the needs of your application. Note that some cache features, such as notifications and high availability, are only available with certain cache offerings. For more information on choosing the cache offering and size that's best for your application, see [Cache offerings][] and [Capacity planning][].

Once the new cache options are configured, click **Create a New Cache**. It can take a few minutes for the cache to be created. To check the status, you can monitor the notifications at the bottom of the portal. After the cache has been created, your new cache has a Running status and is ready for use with default settings. To customize the configuration of your cache, see the following [Configure the cache][] section.


<h2><a name="enable-caching"></a><span class="short-header">Configure the cache</span>Configure the cache</h2>

The **Configure** tab for Cache in the Management Portal is where you configure the options for your cache. Each cache has a **default** named cache, and the Standard and Premium cache offerings support up to nine additional named caches, for a total of ten. Each named cache has its own set of options which allow you to configure your cache in a highly flexible manner.

![NamedCaches][]

To create a named cache, type the name of the new cache into the **Name** box, specify the desired options, click **Save**, and click **Yes** to confirm. To cancel any changes, click **Discard**.

## Expiry Policy and Time (min) ##

The **Expiry Policy** works in conjunction with the **Time (min)** setting to determine when cached items expire. There are three types of expiration policies: **Absolute**, **Sliding**, and **Never**. 

When **Absolute** is specified, the expiration interval specified by **Time (min)** begins when an item is added to the cache. After the interval specified by **Time (min)** elapses, the item expires. 

When **Sliding** is specified, the expiration interval specified by **Time (min)** is reset each time an item is accessed in the cache. The item does not expire until the interval specified by **Time (min)** elapses after the last access to the item.

When **Never** is specified, **Time (min)** must be set to **0**, and items do not expire.

**Absolute** is the default expiration policy and 10 minutes is the default setting for **Time (min)**. The expiration policy is fixed for each item in a named cache, but the **Time (min)** can be customized for each item by using **Add** and **Put** overloads that take a timeout parameter.

For more information about eviction and expiration policies, see [Expiration and Eviction][].

## Notifications ##

Cache notifications that allow your applications to receive asynchronous notifications when a variety of cache operations occur on the cache cluster. Cache notifications also provide automatic invalidation of locally cached objects. For more information, see [Notifications][].

>Notifications are only available in the Standard and Premium cache offerings, and are not available in the Basic cache offering. For more information, see [Cache offerings][].

## High Availability ##

When high availability is enabled, a backup copy is made of each item added to the cache. If an unexpected failure occurs to the primary copy of the item, the backup copy is still available.

By definition, the use of high availability multiplies the amount of required memory for each cached item by two. Consider this memory impact during capacity planning tasks. For more information, see [High Availability][].

>High availability is only available in the Premium cache offering, and is not available in the Basic or Standard cache offerings. For more information, see [Cache offerings][].

## Eviction ##

To maintain the memory capacity available within a cache, least recently used (LRU) eviction is supported. When memory consumption exceeds the memory threshold, objects are evicted from memory, regardless of whether they have expired or not, until the memory pressure is relieved.
Eviction is enabled by default. If eviction is disabled, items will not be evicted from the cache when the capacity is reached, and instead Put and Add operations will fail.

For more information about eviction and expiration policies, see [Expiration and Eviction][].

Once the cache is configured, you can configure the cache clients to allow access to the cache.

<h2><a name="NuGet"></a><span class="short-header">Configure the cache clients</span>Configure the cache clients</h2>

A cache created using the Cache Service (Preview) is accessible from Windows Azure applications running on Azure Web Sites, Web & Worker Roles and Virtual Machines. A NuGet package is provided that simplifies the configuration of cache client applications. 

To configure a client application using the Cache NuGet package, right-click the project in **Solution Explorer** and choose **Manage NuGet Packages**. 

![NuGetPackageMenu][]

Select **Windows Azure Caching**, click **Install**, and then click I Accept.

>If **Windows Azure Caching** does not appear in the list type **WindowsAzure.Caching** into the **Search Online** text box and select it from the results.

![NuGetPackage][]

The NuGet package does several things: it adds the required configuration to the config file of the application, and it adds the required assembly references. For Cloud Services projects, it also adds a cache client diagnostic level setting to the ServiceConfiguration.cscfg file of the Cloud Service.

>For ASP.NET web projects, the Cache NuGet package also adds two commented out sections to web.config. The first section enables session state to be stored in the cache, and the second section enables ASP.NET page output caching. For more information, see [How To: Store ASP.NET Session State in the Cache] and [How To: Store ASP.NET Page Output Caching in the Cache][].

The NuGet package adds the following configuration elements into your application's web.config or app.config. A **dataCacheClients** section and a **cacheDiagnostics** section are added under the **configSections** element. If there is no **configSections** element present, one is created as a child of the **configuration** element.

    <configSections>
      <!-- Existing sections omitted for clarity. -->
      <section name="dataCacheClients" 
        type="Microsoft.ApplicationServer.Caching.DataCacheClientsSection,
              Microsoft.ApplicationServer.Caching.Core" 
        allowLocation="true" 
        allowDefinition="Everywhere" />
  
    <section name="cacheDiagnostics" 
        type="Microsoft.ApplicationServer.Caching.AzureCommon.DiagnosticsConfigurationSection,
              Microsoft.ApplicationServer.Caching.AzureCommon" 
        allowLocation="true" 
        allowDefinition="Everywhere" />


These new sections include references to a **dataCacheClients** element, which is also added to the **configuration** element.

    <dataCacheClients>
      <dataCacheClient name="default">
        <!--To use the in-role flavor of Windows Azure Caching, 
            set identifier to be the cache cluster role name -->
        <!--To use the Windows Azure Caching Service,
            set identifier to be the endpoint of the cache cluster -->
        <autoDiscover isEnabled="true" identifier="[Cache role name or Service Endpoint]" />
        <!--<localCache isEnabled="true" sync="TimeoutBased" objectCount="100000" ttlValue="300" />-->
        <!--Use this section to specify security settings for connecting to your cache. 
            This section is not required if your cache is hosted on a role that is a part
            of your cloud service. -->
        <!--<securityProperties mode="Message" sslEnabled="false">
          <messageSecurity authorizationInfo="[Authentication Key]" />
        </securityProperties>-->
      </dataCacheClient>
    </dataCacheClients>


After the configuration is added, replace the following two items in the newly added configuration.

1. Replace **[Cache role name or Service Endpoint]** with the endpoint, which is displayed on the Dashboard in the Management Portal.

	![Endpoint][]

2. Uncomment the securityProperties section, and replace **[Authentication Key]** with the authentication key, which can be found in the Management Portal by clicking **Manage Keys** from the cache dashboard.

	![AccessKeys][]

>These settings must be configured properly or clients will not be able to access the cache.

For Cloud Services projects, the NuGet package also adds a **ClientDiagnosticLevel** setting to the **ConfigurationSettings** of the cache client role in ServiceConfiguration.cscfg. The following example is the **WebRole1** section from a ServiceConfiguration.cscfg file with a **ClientDiagnosticLevel** of 1, which is the default **ClientDiagnosticLevel**.

    <Role name="WebRole1">
      <Instances count="1" />
      <ConfigurationSettings>
        <!-- Existing settings omitted for clarity. -->
        <Setting name="Microsoft.WindowsAzure.Plugins.Caching.ClientDiagnosticLevel" 
                 value="1" />
      </ConfigurationSettings>
    </Role>

>The client diagnostic level configures the level of caching diagnostic information collected for cache clients. For more information, see [Troubleshooting and Diagnostics][]

The NuGet package also adds references to the following assemblies:

-   Microsoft.ApplicationServer.Caching.Client.dll
-   Microsoft.ApplicationServer.Caching.Core.dll
-   Microsoft.WindowsFabric.Common.dll
-   Microsoft.WindowsFabric.Data.Common.dll
-   Microsoft.ApplicationServer.Caching.AzureCommon.dll
-   Microsoft.ApplicationServer.Caching.AzureClientHelper.dll

If your project is a web project, the following assembly reference is also added:

-	Microsoft.Web.DistributedCache.dll.

>These assemblies are located in the C:\\Program Files\\Microsoft SDKs\\Windows Azure\\.NET SDK\\[sdk version]\\ref\\Caching\\ folder.

Once your client project is configured for caching, you can use the techniques described in the following sections for working with your cache.

<h2><a name="working-with-caches"></a><span class="short-header">Working with Caches</span>Working with Caches</h2>

The steps in this section describe how to perform common tasks with Cache.

-	[How To: Create a DataCache Object][]
-   [How To: Add and Retrieve an Object from the Cache][]
-   [How To: Specify the Expiration of an Object in the Cache][]
-   [How To: Store ASP.NET Session State in the Cache][]
-   [How To: Store ASP.NET Page Output Caching in the Cache][]

<h2><a name="create-cache-object"></a><span class="short-header">Create a DataCache Object</span>How To: Create a DataCache Object</h2>

In order to programatically work with a cache, you need a reference to the cache. Add the following to the top of any file from which you want to use
Windows Azure Cache:

    using Microsoft.ApplicationServer.Caching;

>If Visual Studio doesn't recognize the types in the using
statement even after installing the Cache NuGet package, which adds the necessary references, ensure that the target
profile for the project is .NET Framework 4 or higher, and be sure to select one of the profiles that does not specify **Client Profile**. For instructions on configuring cache clients, see [Configure the cache clients][].

There are two ways to create a **DataCache** object. The first way is to simply create a **DataCache**, passing in the name of the desired cache.

    DataCache cache = new DataCache("default");

Once the **DataCache** is instantiated, you can use it to interact with the cache, as described in the following sections.

To use the second way, create a new **DataCacheFactory** object in your application using the default constructor. This causes the cache client to use the settings in the configuration file. Call either the **GetDefaultCache** method of the new **DataCacheFactory** instance which returns a **DataCache** object, or the **GetCache** method and pass in the name of your cache. These methods return a **DataCache** object that can then be used to programmatically access the cache.

    // Cache client configured by settings in application configuration file.
    DataCacheFactory cacheFactory = new DataCacheFactory();
    DataCache cache = cacheFactory.GetDefaultCache();
    // Or DataCache cache = cacheFactory.GetCache("MyCache");
    // cache can now be used to add and retrieve items.	

<h2><a name="add-object"></a><span class="short-header">Add and Retrieve an Object from the Cache</span>How To: Add and Retrieve an Object from the Cache</h2>

To add an item to the cache, the **Add** method or the **Put** method
can be used. The **Add** method adds the specified object to the cache,
keyed by the value of the key parameter.

    // Add the string "value" to the cache, keyed by "item"
    cache.Add("item", "value");

If an object with the same key is already in the cache, a
**DataCacheException** will be thrown with the following message:

> ErrorCode:SubStatus: An attempt is being made to create an object with
> a Key that already exists in the cache. Caching will only accept
> unique Key values for objects.

To retrieve an object with a specific key, the **Get** method can be used. If the object exists, it is
returned, and if it does not, null is returned.

    // Add the string "value" to the cache, keyed by "key"
    object result = cache.Get("Item");
    if (result == null)
    {
        // "Item" not in cache. Obtain it from specified data source
        // and add it.
        string value = GetItemValue(...);
        cache.Add("item", value);
    }
    else
    {
        // "Item" is in cache, cast result to correct type.
    }

The **Put** method adds the object with the specified key to the cache
if it does not exist, or replaces the object if it does exist.

    // Add the string "value" to the cache, keyed by "item". If it exists,
    // replace it.
    cache.Put("item", "value");

<h2><a name="specify-expiration"></a><span class="short-header">Specify the Expiration of an Object in the Cache</span>How To: Specify the Expiration of an Object in the Cache</h2>

By default items in the cache expire 10 minutes after they are placed in the cache. This can be configured in the **Time (min)** setting on the Configure tab for Cache in the Management Portal.

![NamedCaches][]

There are three types of **Expiry Policy**: **Never**, **Absolute**, and **Sliding**. These configure how **Time (min)** is used to determine expiration. The default **Expiration Type** is **Absolute**, which means that the countdown timer for an item's expiration begins when the item is placed into the cache. Once the specified amount of time has elapsed for an item, the item expires. If **Sliding** is specified, then the expiration countdown for an item is reset each time the item is accessed in the cache, and the item will not expire until the specified amount of time has elapsed since its last access. If **Never** is specified, then **Time (min)** must be set to **0**, and items will not expire, and will remain valid as long as they are in the cache.

If a longer or shorter timeout interval than what is configured in the cache properties is desired, a specific duration can be specified when an item is added or updated in the cache by using the
overload of **Add** and **Put** that take a **TimeSpan** parameter. In
the following example, the string **value** is added to cache, keyed by
**item**, with a timeout of 30 minutes.

    // Add the string "value" to the cache, keyed by "item"
    cache.Add("item", "value", TimeSpan.FromMinutes(30));

To view the remaining timeout interval of an item in the cache, the
**GetCacheItem** method can be used to retrieve a **DataCacheItem**
object that contains information about the item in the cache, including
the remaining timeout interval.

    // Get a DataCacheItem object that contains information about
    // "item" in the cache. If there is no object keyed by "item" null
    // is returned. 
    DataCacheItem item = cache.GetCacheItem("item");
    TimeSpan timeRemaining = item.Timeout;

<h2><a name="store-session"></a><span class="short-header">Store ASP.NET Session State in the Cache</span>How To: Store ASP.NET Session State in the Cache</h2>

The Session State Provider for Windows Azure Cache is an
out-of-process storage mechanism for ASP.NET applications. This provider
enables you to store your session state in a Windows Azure cache rather
than in-memory or in a SQL Server database. To use the caching session
state provider, first configure your cache, and then configure your ASP.NET application for Cache using the Cache NuGet package as described in [Getting Started with Cache Service (Preview)][]. When the Cache NuGet package is installed, it adds a commented out section in web.config that contains the required configuration for your ASP.NET application to use the Session State Provider for Windows Azure Cache.

    <!--Uncomment this section to use Windows Azure Caching for session state caching
    <system.web>
      <sessionState mode="Custom" customProvider="AFCacheSessionStateProvider">
        <providers>
          <add name="AFCacheSessionStateProvider" 
            type="Microsoft.Web.DistributedCache.DistributedCacheSessionStateStoreProvider,
                  Microsoft.Web.DistributedCache" 
            cacheName="default" 
            dataCacheClientName="default"/>
        </providers>
      </sessionState>
    </system.web>-->

>If your web.config does not contain this commented out section after installing the Cache NuGet package, ensure that the latest NuGet Package Manager is installed from [NuGet Package Manager Installation][], and then uninstall and reinstall the package.

To enable the Session State Provider for Windows Azure Cache, uncomment the specified section. The default cache is specified in the provided snippet. To use a different cache, specify the desired cache in the **cacheName** attribute.

For more information about using the Cache service session state
provider, see [Session State Provider for Windows Azure Cache][].

<h2><a name="store-page"></a><span class="short-header">Store ASP.NET Page Output Caching in the Cache</span>How To: Store ASP.NET Page Output Caching in the Cache</h2>

The Output Cache Provider for Windows Azure Cache is an out-of-process storage mechanism for output cache data. This data is specifically for full HTTP
responses (page output caching). The provider plugs into the new output
cache provider extensibility point that was introduced in ASP.NET 4. To
use the output cache provider, first configure your cache cluster, and then configure your ASP.NET application for caching using the Cache NuGet package, as described in [Getting Started with Cache Service (Preview)][]. When the Caching NuGet package is installed, it adds the following commented out section in web.config that contains the required configuration for your ASP.NET application to use the Output Cache Provider for Windows Azure Caching.

    <!--Uncomment this section to use Windows Azure Caching for output caching
    <caching>
      <outputCache defaultProvider="AFCacheOutputCacheProvider">
        <providers>
          <add name="AFCacheOutputCacheProvider" 
            type="Microsoft.Web.DistributedCache.DistributedCacheOutputCacheProvider,
                  Microsoft.Web.DistributedCache" 
            cacheName="default" 
            dataCacheClientName="default" />
        </providers>
      </outputCache>
    </caching>-->

>If your web.config does not contain this commented out section after installing the Cache NuGet package, ensure that the latest NuGet Package Manager is installed from [NuGet Package Manager Installation][], and then uninstall and reinstall the package.

To enable the Output Cache Provider for Windows Azure Cache, uncomment the specified section. The default cache is specified in the provided snippet. To use a different cache, specify the desired cache in the **cacheName** attribute.

Add an **OutputCache** directive to each page for which you wish to cache the output.

    <%@ OutputCache Duration="60" VaryByParam="*" %>

In this example the cached page data will remain in the cache for 60 seconds, and a different version of the page will be cached for each parameter combination. For more information on the available options, see [OutputCache Directive][].

For more information about using the Output Cache Provider for Windows Azure Cache, see [Output Cache Provider for Windows Azure Cache][].

<h2><a name="next-steps"></a><span class="short-header">Next Steps</span>Next Steps</h2>

Now that you've learned the basics of Cache Service (Preview),
follow these links to learn how to do more complex caching tasks.

-   See the MSDN Reference: [Cache Service (Preview)][]
-	Learn how to migrate to Cache Service (Preview): [Migrate to Cache Service (Preview)][]
-   Check out the samples: [Cache Service (Preview) Samples][]


  [Next Steps]: #next-steps
  [What is Windows Azure Cache?]: #what-is
  [Create a Windows Azure Cache]: #create-cache
  [Which type of caching is right for me?]: #choosing-cache
  [Prepare Your Visual Studio Project to Use Windows Azure Caching]: #prepare-vs
  [Configure Your Application to Use Caching]: #configure-app
  [Getting Started with Cache Service (Preview)]: #getting-started-cache-service
  [Create the cache]: #create-cache
  [Configure the cache]: #enable-caching
  [Configure the cache clients]: #NuGet
  [Working with Caches]: #working-with-caches
  [How To: Create a DataCache Object]: #create-cache-object
  [How To: Add and Retrieve an Object from the Cache]: #add-object
  [How To: Specify the Expiration of an Object in the Cache]: #specify-expiration
  [How To: Store ASP.NET Session State in the Cache]: #store-session
  [How To: Store ASP.NET Page Output Caching in the Cache]: #store-page
  [Windows Azure Management Portal]: http://windows.azure.com/
  [NewCacheMenu]: ../../Media/CacheServiceNewCacheMenu.png
  [QuickCreate]: ../../Media/CacheServiceQuickCreate.png
  [Endpoint]: ../../Media/CacheServiceEndpoint.jpg
  [AccessKeys]: ../../Media/CacheServiceManageAccessKeys.png
  [NuGetPackage]: ../../Media/CacheServiceManageNuGetPackage.png
  [NuGetPackageMenu]: ../../Media/CacheServiceManageNuGetPackagesMenu.png
  [NamedCaches]: ../../Media/CacheServiceNamedCaches.jpg
  
  [Target a Supported .NET Framework Profile]: #prepare-vs-target-net
  [How to: Configure a Cache Client Programmatically]: http://msdn.microsoft.com/en-us/library/windowsazure/gg618003.aspx
  [Session State Provider for Windows Azure Cache]: http://go.microsoft.com/fwlink/?LinkId=320835
  [Windows Azure AppFabric Cache: Caching Session State]: http://www.microsoft.com/en-us/showcase/details.aspx?uuid=87c833e9-97a9-42b2-8bb1-7601f9b5ca20
  [Output Cache Provider for Windows Azure Cache]: http://go.microsoft.com/fwlink/?LinkId=320837
  [Windows Azure Shared Caching]: http://msdn.microsoft.com/en-us/library/windowsazure/gg278356.aspx
  [Team Blog]: http://blogs.msdn.com/b/windowsazure/
  [Windows Azure Caching]: http://www.microsoft.com/en-us/showcase/Search.aspx?phrase=azure+caching
  [How to Configure Virtual Machine Sizes]: http://go.microsoft.com/fwlink/?LinkId=164387
  [Windows Azure Caching Capacity Planning Considerations]: http://go.microsoft.com/fwlink/?LinkId=320167
  [Windows Azure Caching]: http://go.microsoft.com/fwlink/?LinkId=252658
  [How to: Set the Cacheability of an ASP.NET Page Declaratively]: http://msdn.microsoft.com/en-us/library/zd1ysf1y.aspx
  [How to: Set a Page's Cacheability Programmatically]: http://msdn.microsoft.com/en-us/library/z852zf6b.aspx
  [Overview of Windows Azure Cache Service (Preview)]: http://go.microsoft.com/fwlink/?LinkId=320830
  [Cache Service (Preview)]: http://go.microsoft.com/fwlink/?LinkId=320830
  [OutputCache Directive]: http://go.microsoft.com/fwlink/?LinkId=251979
  [Troubleshooting and Diagnostics]: http://go.microsoft.com/fwlink/?LinkId=320839
  [NuGet Package Manager Installation]: http://go.microsoft.com/fwlink/?LinkId=240311
  [Cache Pricing Details]: http://www.windowsazure.com/en-us/pricing/details/cache/
  [Management Portal]: https://manage.windowsazure.com/
  [Cache offerings]: http://go.microsoft.com/fwlink/?LinkId=317277
  [Capacity planning]: http://go.microsoft.com/fwlink/?LinkId=320167
  [Expiration and Eviction]: http://go.microsoft.com/fwlink/?LinkId=317278
  [High Availability]: http://go.microsoft.com/fwlink/?LinkId=317329
  [Notifications]: http://go.microsoft.com/fwlink/?LinkId=317276
  [Migrate to Cache Service (Preview)]: http://go.microsoft.com/fwlink/?LinkId=317347
  [Cache Service (Preview) Samples]: http://go.microsoft.com/fwlink/?LinkId=320840
