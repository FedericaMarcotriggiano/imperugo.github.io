---
layout: post
title: Update MVC4 from Beta/RC
categories:
- aspnetmvc
- WebDev
tags:
- aspnetmvc
- migration
- nuget
status: publish
type: post
published: true
comments: true
---
As I wrote in my first post <a title="How to override tostring.it" href="http://tostring.it/2012/05/20/how-to-override-tostring/">here</a>, I’m a web addicted and I’m absolutely in love with <a title="ASP.NET MVC" href="http://tostring.it/tag/aspnetmvc/" target="_blank">ASP.NET MVC</a>. I started to use it from the first beta and I’m using the latest release in my projects.

What I absolutely love in ASP.NET MVC 4 is the distribution mode. Right now it is deployed by <a title="NuGet official site" href="http://www.nuget.org" target="_blank">NuGet</a> and, with few clicks, you can easily update it from the Beta to the RC.

Unluckily, in my case, the update process missed to update one package <strong>Microsoft.Web.Optimization</strong>

To be honest this sentence is not really true, in fact an update for that package is not available, so Nuget correctly updated all packages except that.

With the RC release the package changes its name from <strong>Microsoft.Web.Optimization</strong>  to <strong>Microsoft.AspNet.Web.Optimization </strong>and,<strong> </strong>if you create a new project after you have installed the RC, the package.config looks like that:
<pre class="brush: xml; gutter: true">&lt;?xml version=&quot;1.0&quot; encoding=&quot;utf-8&quot;?&gt;
&lt;packages&gt;
  &lt;package id=&quot;EntityFramework&quot; version=&quot;5.0.0-rc&quot; targetFramework=&quot;net40&quot; /&gt;
  &lt;package id=&quot;jQuery&quot; version=&quot;1.6.2&quot; targetFramework=&quot;net40&quot; /&gt;
  &lt;package id=&quot;jQuery.UI.Combined&quot; version=&quot;1.8.11&quot; targetFramework=&quot;net40&quot; /&gt;
  &lt;package id=&quot;jQuery.Validation&quot; version=&quot;1.8.1&quot; targetFramework=&quot;net40&quot; /&gt;
  &lt;package id=&quot;knockoutjs&quot; version=&quot;2.0.0&quot; targetFramework=&quot;net40&quot; /&gt;
  &lt;package id=&quot;Microsoft.AspNet.Mvc&quot; version=&quot;4.0.20505.0&quot; targetFramework=&quot;net40&quot; /&gt;
  &lt;package id=&quot;Microsoft.AspNet.Providers.Core&quot; version=&quot;1.0&quot; targetFramework=&quot;net40&quot; /&gt;
  &lt;package id=&quot;Microsoft.AspNet.Providers.LocalDB&quot; version=&quot;1.0&quot; targetFramework=&quot;net40&quot; /&gt;
  &lt;package id=&quot;Microsoft.AspNet.Razor&quot; version=&quot;2.0.20505.0&quot; targetFramework=&quot;net40&quot; /&gt;
  &lt;package id=&quot;Microsoft.AspNet.Web.Optimization&quot; version=&quot;1.0.0-beta2&quot; targetFramework=&quot;net40&quot; /&gt;
  &lt;package id=&quot;Microsoft.AspNet.WebApi&quot; version=&quot;4.0.20505.0&quot; targetFramework=&quot;net40&quot; /&gt;
  &lt;package id=&quot;Microsoft.AspNet.WebApi.Client&quot; version=&quot;4.0.20505.0&quot; targetFramework=&quot;net40&quot; /&gt;
  &lt;package id=&quot;Microsoft.AspNet.WebApi.Core&quot; version=&quot;4.0.20505.0&quot; targetFramework=&quot;net40&quot; /&gt;
  &lt;package id=&quot;Microsoft.AspNet.WebApi.WebHost&quot; version=&quot;4.0.20505.0&quot; targetFramework=&quot;net40&quot; /&gt;
  &lt;package id=&quot;Microsoft.AspNet.WebPages&quot; version=&quot;2.0.20505.0&quot; targetFramework=&quot;net40&quot; /&gt;
  &lt;package id=&quot;Microsoft.jQuery.Unobtrusive.Ajax&quot; version=&quot;2.0.20505.0&quot; targetFramework=&quot;net40&quot; /&gt;
  &lt;package id=&quot;Microsoft.jQuery.Unobtrusive.Validation&quot; version=&quot;2.0.20505.0&quot; targetFramework=&quot;net40&quot; /&gt;
  &lt;package id=&quot;Microsoft.Net.Http&quot; version=&quot;2.0.20505.0&quot; targetFramework=&quot;net40&quot; /&gt;
  &lt;package id=&quot;Microsoft.Web.Infrastructure&quot; version=&quot;1.0.0.0&quot; targetFramework=&quot;net40&quot; /&gt;
  &lt;package id=&quot;Modernizr&quot; version=&quot;2.0.6&quot; targetFramework=&quot;net40&quot; /&gt;
  &lt;package id=&quot;Newtonsoft.Json&quot; version=&quot;4.5.1&quot; targetFramework=&quot;net40&quot; /&gt;
  &lt;package id=&quot;WebGrease&quot; version=&quot;1.0.0&quot; targetFramework=&quot;net40&quot; /&gt;
&lt;/packages&gt;</pre>
<strong>But why the package name is changed?</strong> <strong>And, more important, what’s changed?</strong>
Sincerely I don’t have an answer for the first question and I think is not important (nothing changes if we know the reason) but the second point is really really interesting.

The packages include a library called <strong>System.Web.Optimization</strong> that offers an incredible cool feature for combining and minifying our resources (css and js files).

Every web developers know how important is reduce the number of calls from the client to the server obtaining less traffic and more performance (the files cleaned from whitespaces and comments).

Unfortunately this technique makes it difficult to debug and understand the Javascripts and Stylesheets. For this reason the latest version (the version that I didn’t received with the update) includes a cool class that help us to use the classic approach (not combined files) when we are debugging, and the optimized way when the site is in production.

With ASP.NET MVC Beta we were using the bundles in this way:
<pre class="brush: xhtml; gutter: true">&lt;link href=&quot;@System.Web.Optimization.BundleTable.Bundles.ResolveBundleUrl(&quot;~/Content/Styles/css&quot;)&quot; rel=&quot;stylesheet&quot; type=&quot;text/css&quot; /&gt;
&lt;script src=&quot;@System.Web.Optimization.BundleTable.Bundles.ResolveBundleUrl(&quot;~/Content/themes/base/css&quot;)&quot;&gt;&lt;/script&gt;</pre>
And the result will be something like that:
<pre class="brush: xhtml; gutter: true">&lt;link href=&quot;/Content/Styles/css?v=jt8aeS0wrB24h6ADtIW3yucGRpbJ2UViuR3OjTHGcuQ1&quot; rel=&quot;stylesheet&quot; type=&quot;text/css&quot; /&gt;
&lt;script src=&quot;/Scripts/myBundle/js?v=R9XQ5talr3vJ0OlpwOs_MJgF38vpqToKF_ryeXfaBLY1&quot;&gt;&lt;/script&gt;</pre>
How is it possible to debug this output?
With the Beta is really difficult, in fact there are few post in internet that describe how to create and extension method that renders all resources when you are debugging (<a title="Disabling Bundling and Minification in ASP.NET 4.5/MVC 4" href="http://blog.kurtschindler.net/post/disabling-bundling-and-minification-in-aspnet-45mvc-4" target="_blank">this </a>is one of that posts)

With the new packages, so with the new version, the ASP.NET Team introduces the same approach and now we have this code:
<pre class="brush: xhtml; gutter: true">@Styles.Render(&quot;~/Content/themes/base/css&quot;, &quot;~/Content/css&quot;)
@Scripts.Render(&quot;~/bundles/modernizr&quot;)</pre>
Obtaining this result:
<pre class="brush: xhtml; gutter: true">&lt;link href=&quot;/Content/css?v=1KFY-7bppjbhzN_j-uZ7aDrhiojPPiupccFdk_rQVig1&quot; rel=&quot;stylesheet&quot; type=&quot;text/css&quot; /&gt;
&lt;script src=&quot;/bundles/modernizr?v=EuTZa4MRY0ZqCYpBXj_MhJfFJU2QBDf0xGrV_p1fHME1&quot; type=&quot;text/javascript&quot;&gt;&lt;/script&gt;</pre>
Probably you are asking to yourself that the result is the same and it’s just changed a helper but there isn’t.
The helper has an interesting behavior binded to a property in the web.config. Inside system.web/compilation there is an attribute debug; if you set it to false the result is like the code shown above, otherwise it looks like that:
<pre class="brush: xhtml; gutter: true">&lt;link href=&quot;/Content/themes/base/jquery.ui.resizable.css&quot; rel=&quot;stylesheet&quot; type=&quot;text/css&quot; /&gt;
&lt;link href=&quot;/Content/themes/base/jquery.ui.selectable.css&quot; rel=&quot;stylesheet&quot; type=&quot;text/css&quot; /&gt;
&lt;link href=&quot;/Content/themes/base/jquery.ui.accordion.css&quot; rel=&quot;stylesheet&quot; type=&quot;text/css&quot; /&gt;
&lt;link href=&quot;/Content/themes/base/jquery.ui.autocomplete.css&quot; rel=&quot;stylesheet&quot; type=&quot;text/css&quot; /&gt;
&lt;link href=&quot;/Content/themes/base/jquery.ui.button.css&quot; rel=&quot;stylesheet&quot; type=&quot;text/css&quot; /&gt;
&lt;link href=&quot;/Content/themes/base/jquery.ui.dialog.css&quot; rel=&quot;stylesheet&quot; type=&quot;text/css&quot; /&gt;
&lt;link href=&quot;/Content/themes/base/jquery.ui.slider.css&quot; rel=&quot;stylesheet&quot; type=&quot;text/css&quot; /&gt;
&lt;link href=&quot;/Content/themes/base/jquery.ui.tabs.css&quot; rel=&quot;stylesheet&quot; type=&quot;text/css&quot; /&gt;
&lt;link href=&quot;/Content/themes/base/jquery.ui.datepicker.css&quot; rel=&quot;stylesheet&quot; type=&quot;text/css&quot; /&gt;
&lt;link href=&quot;/Content/themes/base/jquery.ui.progressbar.css&quot; rel=&quot;stylesheet&quot; type=&quot;text/css&quot; /&gt;
&lt;link href=&quot;/Content/themes/base/jquery.ui.theme.css&quot; rel=&quot;stylesheet&quot; type=&quot;text/css&quot; /&gt;
&lt;link href=&quot;/Content/site.css&quot; rel=&quot;stylesheet&quot; type=&quot;text/css&quot; /&gt;
&lt;script src=&quot;/Scripts/modernizr-2.0.6-development-only.js&quot; type=&quot;text/javascript&quot;&gt;&lt;/script&gt;</pre>
Now it’s really simple to debug our styles and Javascript functions.
If you had update your MVC Beta application with NuGet and you have the same problem, the solution is easy.

Remove <strong>Microsoft.Web.Optimization and install Microsoft.AspNet.Web.Optimization!</strong>
