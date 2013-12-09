---
layout: doc
title: Postal outside of ASP.NET
---

Postal is able to render email views when running outside of an ASP.NET context e.g. console app or windows service.

This is achieved by using a custom view engine.
Only Razor views are supported.
(A huge thank you to the RazorEngine project for doing all the heavy lifting!)

Here's a code sample:

{% highlight csharp %}
using Postal;

class Program
{
    static void Main(string[] args)
    {
        // Get the path to the directory containing views
        var viewsPath = Path.GetFullPath(@"..\..\Views");

        var engines = new ViewEngineCollection();
        engines.Add(new FileSystemRazorViewEngine(viewsPath));

        var service = new EmailService(engines);

        dynamic email = new Email("Test");
        // Will look for Test.cshtml or Test.vbhtml in Views directory.
        email.Message = "Hello, non-asp.net world!";
        service.Send(email);
    }
}
{% endhighlight %}

### Limitations

Layouts are not supported by RazorEngine yet. So you cannot use layouts for your email views.

In your email views, you MUST use Model and not ViewBag. RazorEngine only supports Model.