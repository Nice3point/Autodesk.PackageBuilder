# Autodesk.PackageBuilder

This package is intended to build Autodesk `PackageContent.xml` and `RevitAddin.addin` using C# fluent API.

[![Autodesk](https://img.shields.io/badge/Autodesk-black?logo=autodesk&logoColor=white)](../..)
[![Revit](https://img.shields.io/badge/Revit-black.svg)](../..)

[![Visual Studio 2022](https://img.shields.io/badge/Visual%20Studio-2022-blue)](../..)
[![Nuke](https://img.shields.io/badge/Nuke-Build-blue)](https://nuke.build/)
[![License MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Build](../../actions/workflows/Build.yml/badge.svg)](../../actions)
[![Release](https://img.shields.io/nuget/v/Autodesk.PackageBuilder?logo=nuget&label=release&color=blue)](https://www.nuget.org/packages/Autodesk.PackageBuilder)

## Examples

### Create PackageContents.xml

To get the `PackageContents.xml` like this:
```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationPackage SchemaVersion="1.0" ProductType="Application" AutodeskProduct="Revit" Name="RevitAddin" AppVersion="1.0.0">
  <CompanyDetails Name="Company Name" Email="email" Url="url" />
  <Components Description="Revit 2021">
    <RuntimeRequirements OS="Win64" Platform="Revit" SeriesMin="R2021" SeriesMax="R2021" />
    <ComponentEntry AppName="RevitAddin" ModuleName="./Contents/2021/RevitAddin.addin" />
  </Components>
  <Components Description="Revit 2022">
    <RuntimeRequirements OS="Win64" Platform="Revit" SeriesMin="R2022" SeriesMax="R2022" />
    <ComponentEntry AppName="RevitAddin" ModuleName="./Contents/2022/RevitAddin.addin" />
  </Components>
</ApplicationPackage>
```

Inherit your builder class from `PackageContentsBuilder` base class.
```C#
public class DemoPackageBuilder : PackageContentsBuilder
{
    public DemoPackageBuilder()
    {
        ApplicationPackage
            .Create()
            .ProductType(ProductTypes.Application)
            .AutodeskProduct(AutodeskProducts.Revit)
            .Name("RevitAddin")
            .AppVersion("1.0.0");

        CompanyDetails
            .Create("Company Name")
            .Email("email")
            .Url("url");

        Components
            .CreateEntry("Revit 2021")
            .OS("Win64")
            .Platform("Revit")
            .SeriesMin("R2021")
            .SeriesMax("R2021")
            .AppName("RevitAddin")
            .ModuleName(@"./Contents/2021/RevitAddin.addin");

        Components
            .CreateEntry("Revit 2022")
            .RevitPlatform(2022)
            .AppName("RevitAddin")
            .ModuleName(@"./Contents/2022/RevitAddin.addin");
    }
}
```

Or use `BuilderUtils.Build<PackageContentsBuilder>()`.

```C#
var builder = BuilderUtils.Build<PackageContentsBuilder>(builder =>
{
    builder.ApplicationPackage
        .Create()
        .ProductType(ProductTypes.Application)
        .AutodeskProduct(AutodeskProducts.Revit)
        .Name("RevitAddin")
        .AppVersion("1.0.0");

    builder.CompanyDetails
        .Create("Company Name")
        .Email("email")
        .Url("url");

    builder.Components
        .CreateEntry("Revit 2021")
        .OS("Win64")
        .Platform("Revit")
        .SeriesMin("R2021")
        .SeriesMax("R2021")
        .AppName("RevitAddin")
        .ModuleName(@"./Contents/2021/RevitAddin.addin");

    builder.Components
        .CreateEntry("Revit 2022")
        .RevitPlatform(2022)
        .AppName("RevitAddin")
        .ModuleName(@"./Contents/2022/RevitAddin.addin");
});
```

## Getting results

### String result

```C#
var builder = new DemoPackageBuilder();
var result = builder.ToString();
```

### .xml file

```C#
var builder = new DemoPackageBuilder();
builder.Build("PackageContents.xml");

// or

BuilderUtils.Build<DemoPackageBuilder>("PackageContents.xml");

// or

BuilderUtils.Build<DemoPackageBuilder>(builder => {...}, "PackageContents.xml");
```

### Create RevitAddin.addin

To get the `RevitAddin.addin` like this:

```xml
<?xml version="1.0" encoding="utf-8"?>
<RevitAddIns>
  <AddIn Type="Application">
    <Name>RevitAddin</Name>
    <AddInId>F6DB5994-D788-4060-9C97-16F6C1B07857</AddInId>
    <Assembly>RevitAddin.dll</Assembly>
    <FullClassName>RevitAddin.App</FullClassName>
    <VendorId>RevitAddin</VendorId>
    <VendorDescription>RevitAddin</VendorDescription>
    <AllowLoadingIntoExistingSession>true</AllowLoadingIntoExistingSession>
  </AddIn>
</RevitAddIns>
```

Inherit your builder class from `RevitAddInsBuilder` base class.

```C#
public class DemoAddinBuilder : RevitAddInsBuilder
{
    public DemoAddinBuilder()
    {
        AddIn.CreateEntry("Application")
            .Name("RevitAddin")
            .AddInId("F6DB5994-D788-4060-9C97-16F6C1B07857")
            .Assembly("RevitAddin.dll")
            .FullClassName("RevitAddin.App")
            .VendorId("RevitAddin")
            .VendorDescription("RevitAddin");
    }
}
```

Or use `BuilderUtils.Build<RevitAddInsBuilder>()`.

```C#
var builder = BuilderUtils.Build<RevitAddInsBuilder>(builder =>
{
    builder.AddIn.CreateEntry("Application")
        .Name("RevitAddin")
        .AddInId("F6DB5994-D788-4060-9C97-16F6C1B07857")
        .Assembly("RevitAddin.dll")
        .FullClassName("RevitAddin.App")
        .VendorId("RevitAddin")
        .VendorDescription("RevitAddin");
});
```

## Getting results

### String result

```C#
var builder = new DemoAddinBuilder();
var result = builder.ToString();
```

### .addin file

```C#
var builder = new DemoAddinBuilder();
builder.Build("RevitAddin.addin");

// or

BuilderUtils.Build<DemoAddinBuilder>("RevitAddin.addin");

// or

BuilderUtils.Build<DemoAddinBuilder>(builder => {...}, "RevitAddin.addin");
```

## Package Inspiration / Reference

This package was inspared by [InnoSetup.ScriptBuilder](https://github.com/ReactiveBIM/InnoSetup.ScriptBuilder) package.

## License

This package is [licensed](LICENSE) under the [MIT Licence](https://en.wikipedia.org/wiki/MIT_License).

---

Do you like this package? Please [star this project on GitHub](../../stargazers)!
