---
layout: post
template: post
date: 2013-04-15
tags:
- ASP.NET MVC
title: "ASP.NET MVC - Default Model Binder with HTML Validation"
slug: /post/48031562990/aspnet-mvc-default-model-binder-with-html
description: "ASP.NET MVC - Default Model Binder with HTML Validation"
---
<p><strong>This post demonstrates how we can handle gracefully the HTTP request validation which exists in ASP.NET, when using ASP.NET MVC.</strong> Request validation is a feature in ASP.NET that examines an HTTP request and determines whether it contains potentially dangerous content. For example, if a user tries to input some malicious code in a field using the script element, the ASP.NET throws a "potentially dangerous value was detected" error and stops page processing. Without this validation, the site is vulnerable to this exploit typically referred as a cross-site scripting (XSS) attack.</p>
<p>Consider this form</p>
<p>&nbsp;</p>
<p><figure class="tmblr-full" data-orig-height="470" data-orig-width="500"><img alt="image" src="https://66.media.tumblr.com/65aeb86afead403581aee09b8c95103d/a530e3b9dda3aaca-2b/s540x810/7813a045ef5e112ad59ac7c20cb9d1c53662fdc9.png" data-orig-height="470" data-orig-width="500"></figure></p>
<p></p>
<p>Here is the error when the user tries to submit a field with HTML elements, pressing the Save button</p>
<p>&nbsp;</p>
<p><figure class="tmblr-full" data-orig-height="354" data-orig-width="500"><img alt="image" src="https://66.media.tumblr.com/1ae9f8f8c0d150c6dc42b7a55b1304f6/a530e3b9dda3aaca-bb/s540x810/2f2a66c9a1e8805bfbeba51a9efa0d93dd1cd557.png" data-orig-height="354" data-orig-width="500"></figure></p>
<p></p>
<p></p>
<p>We can disable this validation at a field, action, controller, or application level. But we have to think about the consequences of disabling it and being vulnerable to an XSS attack. <strong>Instead of disabling it, I want to continue validating the potential dangerous requests, but rather than throw the error and stop processing, I want to add a validation message for the field that is violating the validation.</strong></p>
<p>&nbsp;</p>
<p><figure class="tmblr-full" data-orig-height="421" data-orig-width="500"><img alt="image" src="https://66.media.tumblr.com/f4fdeb5ab7319eff0e7de8e8cebdd83b/a530e3b9dda3aaca-e1/s540x810/f8931121223d166489a86b914efb9301d111e35b.png" data-orig-height="421" data-orig-width="500"></figure></p>
<p></p>
<p></p>
<p>In ASP.NET MVC, we can do this using a custom default model binder. <strong>The model binding is the mechanism that automatically populates the controller action parameters, taking care of the details of property mappings and type conversions</strong>. The default model binder in ASP.NET MVC is the class <a href="http://msdn.microsoft.com/en-us/library/system.web.mvc.defaultmodelbinder(v=vs.98).aspx" title="Default Model Binder" target="_blank">DefaultModelBinder</a> . The good news is that we can have a custom default model binder. In our case, we want to inherit the DefaultModelBinder, overriding the main method BindModel, and handle the exception HttpRequestValidationException which is thrown when a potential dangerous request is detected</p>

```cs
public class DefaultModelBinderWithHtmlValidation : DefaultModelBinder
{
    public override object BindModel(ControllerContext controllerContext, ModelBindingContext bindingContext)
    {
        try
        {
            return base.BindModel(controllerContext, bindingContext);
        }
        catch (HttpRequestValidationException)
        {
            Trace.TraceWarning("Ilegal characters were found in field {0}", bindingContext.ModelMetadata.DisplayName ?? bindingContext.ModelName);
            bindingContext.ModelState.AddModelError(bindingContext.ModelName, string.Format("Ilegal characters were found in field {0}", bindingContext.ModelMetadata.DisplayName ?? bindingContext.ModelName));
        }

        //Cast the value provider to an IUnvalidatedValueProvider, which allows to skip validation
        IUnvalidatedValueProvider provider = bindingContext.ValueProvider as IUnvalidatedValueProvider;
        if (provider == null) return null;

        //Get the attempted value, skiping the validation
        var result = provider.GetValue(bindingContext.ModelName, skipValidation: true);
        Debug.Assert(result != null, "result is null");

        return result.AttemptedValue;
    }
}
```

<p>So, what we do is quite simple: we catch the HttpRequestValidationException exception and add a validation error message to the ModelState specifying the field name that is violating the request. Next we read the attempted value, skipping the validation and return. <strong>In our action controller we can't forget to check if the ModelState is valid</strong>.</p>

```cs
[HttpPost]
public ActionResult Create(MyModel model, string fieldArgument)
{

    if (ModelState.IsValid)
    {
        //The model is OK. We can do whatever we want to do with the model
        model.MyMessage = "Model Ok Updated @ " + DateTime.Now;
    }

    ViewBag.FieldArgument = fieldArgument;
    return View(model);
}
```

<p>Next we have to register our binder as the default binder. We do this in the application start</p>

```cs
//Register our binder as the default model binder
ModelBinders.Binders.DefaultBinder = new DefaultModelBinderWithHtmlValidation();
```

<p>You can check the full source code <a href="https://github.com/bfcamara/DefaultModelBinderWithHtmlValidation-AspNetMVc" title="Sample Source Code" target="_blank">here</a> on my github.</p>