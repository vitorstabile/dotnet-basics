# .NET Core — A Comprehensive Guide for Beginners

<h1 align="center">
    <img alt=".NET" title=".NET" src="https://upload.wikimedia.org/wikipedia/commons/thumb/7/7d/Microsoft_.NET_logo.svg/456px-Microsoft_.NET_logo.svg.png" width="220px" />
</h1>

<p align="center">
  <a href="#about">About</a>&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;
  <a href="#contents">Contents</a>&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;
  <a href="#section-1">Section 1 — What is .NET?</a>&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;
  <a href="#section-2">Section 2 — CLI & Project Setup</a>&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;
  <a href="#section-3">Section 3 — Project Structure</a>&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;
  <a href="#section-4">Section 4 — Configuration</a>&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;
  <a href="#section-5">Section 5 — Dependency Injection</a>&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;
  <a href="#section-6">Section 6 — Exception Handling</a>&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;
  <a href="#section-7">Section 7 — Logging</a>&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;
  <a href="#section-8">Section 8 — NuGet & Packages</a>&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;
  <a href="#section-9">Section 9 — Testing</a>&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;
  <a href="#references">References</a>
</p>

---

## :bookmark_tabs: About <a name = "about"></a>

This guide is a structured, step-by-step roadmap to understand and work with the **.NET Platform** — the runtime, tooling, project system, configuration, dependency injection, logging, and testing that underpin every .NET application.

This guide is **Part 2** of a three-part series:

| Part | Guide | Focus |
|------|-------|-------|
| 1 | README-csharp | The C# language |
| **2** | **README-dotnetcore (this)** | **The .NET platform & runtime** |
| 3 | README-aspnetcore | ASP.NET Core web development |

> **Prerequisites:** Familiarity with C# basics (Part 1). Each section builds on the previous one.

> **How to use this guide:** Read each section in order. Every topic includes a brief explanation, key concepts, and practical code examples. Each section ends with exercises to reinforce your knowledge.

---

## :books: Contents <a name = "contents"></a>

| Section | Topic |
|---------|-------|
| [Section 1](#section-1) | What is .NET? |
| [Section 2](#section-2) | CLI & Project Setup |
| [Section 3](#section-3) | Project Structure |
| [Section 4](#section-4) | Configuration System |
| [Section 5](#section-5) | Dependency Injection |
| [Section 6](#section-6) | Exception Handling |
| [Section 7](#section-7) | Logging |
| [Section 8](#section-8) | NuGet & Package Management |
| [Section 9](#section-9) | Testing |

---

## Section 1 — What is .NET? <a name = "section-1"></a>

### 1.1 Overview

**.NET** is a free, open-source, cross-platform development framework maintained by Microsoft. It provides everything needed to build and run applications written in C#, F#, or VB.NET on Windows, Linux, and macOS.

**Core components:**

| Component | Description |
|-----------|-------------|
| **CLR** (Common Language Runtime) | Executes managed code, handles garbage collection, exceptions, and threads |
| **BCL** (Base Class Library) | Built-in namespaces: `System`, `System.Collections`, `System.IO`, `System.Net`, etc. |
| **SDK** | Tools to create, build, test, and publish .NET applications (`dotnet` CLI) |
| **NuGet** | Package manager for third-party and first-party libraries |
| **Runtime** | The environment where compiled .NET code actually runs |

---

### 1.2 .NET Evolution

```
.NET Framework 1.0  (2002) — Windows only, closed-source
      ↓
.NET Framework 4.x  (2010–2019) — mature but Windows-locked
      ↓
.NET Core 1.0       (2016) — cross-platform reboot, open-source
.NET Core 2.x       (2017–2018) — performance improvements
.NET Core 3.x       (2019–2020) — desktop apps (WPF, WinForms) on .NET Core
      ↓
.NET 5              (2020) — unified: merges .NET Core + .NET Framework into one
.NET 6 (LTS)        (2021) — minimal hosting, hot reload
.NET 7              (2022) — performance & C# 11
.NET 8 (LTS)        (2023) — Native AOT, Blazor improvements
.NET 9              (2024) — latest features
```

**LTS = Long-Term Support (3 years of support)**

> Today, when someone says ".NET", they mean **.NET 5+** (the unified, cross-platform platform). ".NET Core" technically refers to versions 1.x–3.x, but the term is still widely used to mean "modern cross-platform .NET".

---

### 1.3 How .NET Executes Code

```
C# Source Code (.cs)
        ↓ Roslyn compiler (csc / dotnet build)
Intermediate Language (IL / CIL) in .dll or .exe
        ↓ CLR's JIT (Just-In-Time) compiler at runtime
Native Machine Code
        ↓
Runs on CPU
```

Key concepts:
- **IL (Intermediate Language):** Platform-neutral bytecode. All .NET languages compile to IL.
- **JIT (Just-In-Time):** The CLR compiles IL to native code on first execution.
- **AOT (Ahead-of-Time):** .NET 7+ supports Native AOT — compiles to native code at publish time for faster startup and smaller binaries.
- **GC (Garbage Collector):** Automatically frees unused memory. You don't call `free()`.

---

### 1.4 Runtimes and SDKs

```bash
# List all installed SDKs
dotnet --list-sdks
# 6.0.420 [C:\Program Files\dotnet\sdk]
# 8.0.100 [C:\Program Files\dotnet\sdk]

# List all installed runtimes
dotnet --list-runtimes
# Microsoft.AspNetCore.App 8.0.0
# Microsoft.NETCore.App 8.0.0
# Microsoft.WindowsDesktop.App 8.0.0

# Current SDK version in use
dotnet --version
```

**SDK vs Runtime:**
- **SDK** — includes the compiler, CLI, and runtime. Needed to **develop** apps.
- **Runtime** — only what's needed to **run** already-compiled apps.

---

### :pencil: Section 1 — Exercises

1. Run `dotnet --list-sdks` and `dotnet --list-runtimes`. Identify which versions are LTS.
2. Research the difference between `Microsoft.NETCore.App`, `Microsoft.AspNetCore.App`, and `Microsoft.WindowsDesktop.App` runtimes.
3. Create a simple console app and inspect the generated `.dll` file using `dotnet-ildasm` or `ILSpy` to see what IL looks like.

---

## Section 2 — CLI & Project Setup <a name = "section-2"></a>

### 2.1 The .NET CLI

The `dotnet` CLI is the primary tool for all .NET development tasks.

```bash
# ─── INFO ────────────────────────────────────────────────────
dotnet --version                     # active SDK version
dotnet --info                        # detailed environment info
dotnet --list-sdks                   # all installed SDKs
dotnet --list-runtimes               # all installed runtimes

# ─── PROJECT CREATION ────────────────────────────────────────
dotnet new list                      # list all available templates
dotnet new console    -n MyApp       # Console application
dotnet new classlib   -n MyLib       # Class library (no entry point)
dotnet new webapi     -n MyApi       # ASP.NET Core Web API
dotnet new mvc        -n MyMvc       # ASP.NET Core MVC
dotnet new blazorwasm -n MyBlazor    # Blazor WebAssembly
dotnet new worker     -n MyWorker    # Background worker service
dotnet new xunit      -n MyApp.Tests # xUnit test project
dotnet new sln        -n MySolution  # Solution file

# ─── SOLUTION MANAGEMENT ─────────────────────────────────────
dotnet sln MySolution.sln add MyApp/MyApp.csproj
dotnet sln MySolution.sln add MyApp.Tests/MyApp.Tests.csproj
dotnet sln MySolution.sln list       # list projects in solution

# ─── BUILD ───────────────────────────────────────────────────
dotnet build                         # Debug build
dotnet build -c Release              # Release build (optimized)
dotnet build --no-restore            # skip package restore
dotnet build -v detailed             # verbose output

# ─── RUN ─────────────────────────────────────────────────────
dotnet run                           # build + run current project
dotnet run --project ./MyApi         # run specific project
dotnet run -- --port 8080            # pass args to the application
dotnet watch run                     # hot-reload on file save (dev)
dotnet watch test                    # re-run tests on file save

# ─── PUBLISH ─────────────────────────────────────────────────
dotnet publish -c Release            # publish for production
dotnet publish -c Release -r linux-x64 --self-contained   # self-contained Linux
dotnet publish -c Release -r win-x64  --self-contained   # self-contained Windows
dotnet publish -c Release /p:PublishSingleFile=true       # single .exe file

# ─── PACKAGES ────────────────────────────────────────────────
dotnet add package Newtonsoft.Json                      # latest version
dotnet add package Serilog --version 3.1.1              # specific version
dotnet remove package Newtonsoft.Json
dotnet list package                                     # all packages
dotnet list package --outdated                          # show outdated
dotnet restore                                          # restore dependencies

# ─── TESTING ─────────────────────────────────────────────────
dotnet test                          # run all tests
dotnet test -v normal                # with output
dotnet test --filter "Category=Unit" # filter by trait
dotnet test --collect:"XPlat Code Coverage" # with coverage

# ─── GLOBAL TOOLS ────────────────────────────────────────────
dotnet tool install  --global dotnet-ef       # EF Core CLI
dotnet tool install  --global dotnet-outdated # check outdated packages
dotnet tool install  --global dotnet-format   # code formatter
dotnet tool list     --global                 # list installed global tools
dotnet tool update   --global dotnet-ef       # update a tool
dotnet tool uninstall --global dotnet-ef      # remove a tool
```

---

### 2.2 global.json — Pinning the SDK Version

`global.json` lets you control which SDK version is used for a project or solution.

```json
// global.json (place at the solution root)
{
  "sdk": {
    "version": "8.0.100",
    "rollForward": "latestMinor"
  }
}
```

`rollForward` options: `patch`, `minor`, `major`, `latestPatch`, `latestMinor`, `latestMajor`, `disable`.

---

### 2.3 Target Frameworks

The `<TargetFramework>` in `.csproj` specifies which .NET version to compile for:

```xml
<!-- Single target -->
<TargetFramework>net8.0</TargetFramework>

<!-- Multi-target (library targeting multiple frameworks) -->
<TargetFrameworks>net6.0;net7.0;net8.0</TargetFrameworks>
```

Common TFMs (Target Framework Monikers):

| TFM | Description |
|-----|-------------|
| `net8.0` | .NET 8 (cross-platform) |
| `net8.0-windows` | .NET 8 with Windows-specific APIs |
| `net6.0` | .NET 6 LTS |
| `net48` | .NET Framework 4.8 |
| `netstandard2.0` | Library compatible with both .NET Framework and .NET Core |

---

### :pencil: Section 2 — Exercises

1. Create a solution with two projects: a `classlib` called `MathLibrary` and a `console` called `MathApp`. Add a reference from `MathApp` to `MathLibrary` and call a method from the library.
2. Add a `global.json` to pin the SDK to your current version. Verify it works with `dotnet --version` inside that folder.
3. Publish `MathApp` as a self-contained single-file executable for your current OS. Inspect the output folder.

---

## Section 3 — Project Structure <a name = "section-3"></a>

### 3.1 The .csproj File

The `.csproj` is an XML file that describes your project: its target framework, dependencies, build settings, and more.

```xml
<!-- Console application -->
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <OutputType>Exe</OutputType>               <!-- Exe = app, Library = dll -->
    <TargetFramework>net8.0</TargetFramework>
    <Nullable>enable</Nullable>                <!-- Enable nullable reference types -->
    <ImplicitUsings>enable</ImplicitUsings>    <!-- Auto-imports common namespaces -->
    <LangVersion>latest</LangVersion>          <!-- Use the latest C# version -->
    <RootNamespace>MyApp</RootNamespace>
    <AssemblyName>MyApp</AssemblyName>
    <Version>1.0.0</Version>
    <Authors>Your Name</Authors>
  </PropertyGroup>

  <!-- NuGet package references -->
  <ItemGroup>
    <PackageReference Include="Newtonsoft.Json" Version="13.0.3" />
    <PackageReference Include="Serilog"         Version="3.1.1"  />
  </ItemGroup>

  <!-- Reference to another project in the same solution -->
  <ItemGroup>
    <ProjectReference Include="..\MyLibrary\MyLibrary.csproj" />
  </ItemGroup>

  <!-- Include extra files -->
  <ItemGroup>
    <None Include="data.json" CopyToOutputDirectory="Always" />
  </ItemGroup>

</Project>
```

---

### 3.2 Implicit Usings

With `<ImplicitUsings>enable</ImplicitUsings>`, the following namespaces are automatically imported in every `.cs` file (no `using` statement needed):

```
System
System.Collections.Generic
System.IO
System.Linq
System.Net.Http
System.Threading
System.Threading.Tasks
```

For Web projects (`Microsoft.NET.Sdk.Web`), additional namespaces are imported:
```
Microsoft.AspNetCore.Builder
Microsoft.AspNetCore.Hosting
Microsoft.AspNetCore.Http
Microsoft.AspNetCore.Routing
Microsoft.Extensions.Configuration
Microsoft.Extensions.DependencyInjection
Microsoft.Extensions.Hosting
Microsoft.Extensions.Logging
```

---

### 3.3 Recommended Folder Structure

**Console / Class Library:**

```
MyApp/
├── MyApp.sln
├── src/
│   └── MyApp/
│       ├── Models/         ← data classes
│       ├── Services/       ← business logic
│       ├── Repositories/   ← data access
│       ├── Utilities/      ← helpers, extensions
│       ├── Program.cs      ← entry point
│       └── MyApp.csproj
└── tests/
    └── MyApp.Tests/
        ├── Unit/
        ├── Integration/
        └── MyApp.Tests.csproj
```

**ASP.NET Core Web API:**

```
ProductsApi/
├── ProductsApi.sln
├── src/
│   └── ProductsApi/
│       ├── Controllers/        ← API endpoints
│       ├── Models/             ← entities / domain objects
│       ├── DTOs/               ← request/response shapes
│       ├── Services/
│       │   ├── Interfaces/
│       │   └── Implementations/
│       ├── Repositories/
│       │   ├── Interfaces/
│       │   └── Implementations/
│       ├── Data/               ← DbContext, migrations
│       ├── Middleware/         ← custom middleware
│       ├── Extensions/         ← IServiceCollection extensions
│       ├── appsettings.json
│       ├── appsettings.Development.json
│       ├── Program.cs
│       └── ProductsApi.csproj
└── tests/
    └── ProductsApi.Tests/
        ├── Unit/
        │   └── Services/
        ├── Integration/
        │   └── Controllers/
        └── ProductsApi.Tests.csproj
```

---

### 3.4 Namespaces and File-Scoped Namespaces

```csharp
// Traditional namespace (C# 1–9)
namespace MyApp.Services
{
    public class ProductService
    {
        // ...
    }
}

// File-scoped namespace (C# 10+) — preferred, reduces indentation
namespace MyApp.Services;

public class ProductService
{
    // ...
}
```

---

### :pencil: Section 3 — Exercises

1. Create a multi-project solution following the folder structure above. Add a `Models` project (classlib), an `App` project (console), and a `Tests` project (xunit). Wire them together.
2. Add a `Constants.cs` file to your project using file-scoped namespaces. Define at least 5 constants used across the app.
3. Explore `<ImplicitUsings>`: disable it and observe which `using` statements are now required. Re-enable it and notice the difference.

---

## Section 4 — Configuration System <a name = "section-4"></a>

### 4.1 How Configuration Works

.NET's configuration system reads settings from multiple **providers** in order of precedence — **later sources override earlier ones**:

```
Priority (lowest → highest):
  1. appsettings.json
  2. appsettings.{Environment}.json
  3. User Secrets (Development only)
  4. Environment Variables
  5. Command-line arguments
```

---

### 4.2 appsettings.json

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft": "Warning",
      "Microsoft.Hosting.Lifetime": "Information"
    }
  },
  "AllowedHosts": "*",
  "ConnectionStrings": {
    "DefaultConnection": "Server=localhost;Database=MyDb;User Id=sa;Password=secret;"
  },
  "AppSettings": {
    "AppName": "My Application",
    "Version": "1.0.0",
    "MaxPageSize": 50,
    "SupportEmail": "support@myapp.com",
    "FeatureFlags": {
      "EnableNewDashboard": true,
      "EnableBetaApi": false
    }
  },
  "Smtp": {
    "Host": "smtp.gmail.com",
    "Port": 587,
    "UseSsl": true,
    "Username": "noreply@myapp.com"
  }
}
```

```json
// appsettings.Development.json — overrides the base file in Development
{
  "Logging": {
    "LogLevel": {
      "Default": "Debug",
      "Microsoft.EntityFrameworkCore": "Information"
    }
  },
  "ConnectionStrings": {
    "DefaultConnection": "Data Source=dev.db"
  },
  "AppSettings": {
    "FeatureFlags": {
      "EnableBetaApi": true
    }
  }
}
```

---

### 4.3 Reading Configuration

```csharp
// In Program.cs (or any place with access to IConfiguration)
var builder = Host.CreateApplicationBuilder(args);

// ─── Direct key access (string) ────────────────────────────
string appName = builder.Configuration["AppSettings:AppName"] ?? "Unknown";
string connStr = builder.Configuration.GetConnectionString("DefaultConnection")!;

// ─── Typed value with default ──────────────────────────────
int maxPageSize = builder.Configuration.GetValue<int>("AppSettings:MaxPageSize", 25);
bool betaEnabled = builder.Configuration.GetValue<bool>(
    "AppSettings:FeatureFlags:EnableBetaApi", false);

// ─── Bind a section to a class (recommended) ───────────────
var appSettings = builder.Configuration
    .GetSection("AppSettings")
    .Get<AppSettings>()!;

// ─── IOptions<T> pattern (with DI) ─────────────────────────
builder.Services.Configure<AppSettings>(
    builder.Configuration.GetSection("AppSettings"));
builder.Services.Configure<SmtpSettings>(
    builder.Configuration.GetSection("Smtp"));
```

---

### 4.4 Strongly Typed Options

```csharp
// Options/AppSettings.cs
public class AppSettings
{
    public string  AppName      { get; set; } = string.Empty;
    public string  Version      { get; set; } = string.Empty;
    public int     MaxPageSize  { get; set; } = 25;
    public string  SupportEmail { get; set; } = string.Empty;
    public FeatureFlags FeatureFlags { get; set; } = new();
}

public class FeatureFlags
{
    public bool EnableNewDashboard { get; set; }
    public bool EnableBetaApi      { get; set; }
}

public class SmtpSettings
{
    public string Host     { get; set; } = string.Empty;
    public int    Port     { get; set; } = 587;
    public bool   UseSsl   { get; set; } = true;
    public string Username { get; set; } = string.Empty;
}

// Register in Program.cs
builder.Services.Configure<AppSettings>(
    builder.Configuration.GetSection("AppSettings"));
builder.Services.Configure<SmtpSettings>(
    builder.Configuration.GetSection("Smtp"));

// Inject in a service
public class EmailService
{
    private readonly SmtpSettings _smtp;
    private readonly AppSettings _app;

    // IOptions<T> — reads value once at startup (singleton-like)
    public EmailService(IOptions<SmtpSettings> smtpOptions,
                        IOptions<AppSettings> appOptions)
    {
        _smtp = smtpOptions.Value;
        _app  = appOptions.Value;
    }
}

// IOptionsSnapshot<T> — reloads per scope (scoped services)
// IOptionsMonitor<T>  — live reload, notifies on change (singletons)
public class FeatureService
{
    private readonly IOptionsMonitor<AppSettings> _monitor;

    public FeatureService(IOptionsMonitor<AppSettings> monitor)
    {
        _monitor = monitor;
        // Subscribe to changes
        _monitor.OnChange(settings =>
            Console.WriteLine($"Settings changed: EnableBetaApi = {settings.FeatureFlags.EnableBetaApi}"));
    }

    public bool IsBetaEnabled => _monitor.CurrentValue.FeatureFlags.EnableBetaApi;
}
```

---

### 4.5 User Secrets (Development)

User Secrets keep sensitive config out of source code during development.

```bash
# Initialize user secrets for the project
dotnet user-secrets init

# Set a secret
dotnet user-secrets set "Smtp:Password" "my-super-secret-password"
dotnet user-secrets set "ConnectionStrings:DefaultConnection" "Server=prod;..."

# List secrets
dotnet user-secrets list

# Remove a secret
dotnet user-secrets remove "Smtp:Password"

# Clear all secrets
dotnet user-secrets clear
```

Secrets are stored in `%APPDATA%\Microsoft\UserSecrets\{userSecretsId}\secrets.json` (Windows) or `~/.microsoft/usersecrets/{userSecretsId}/secrets.json` (Linux/macOS) — **outside** your project folder, so they're never committed to git.

---

### 4.6 Environment Variables

Environment variables are the standard way to configure apps in production (containers, cloud, CI/CD).

```bash
# Setting environment variables
# Linux / macOS
export ASPNETCORE_ENVIRONMENT=Production
export AppSettings__MaxPageSize=100        # __ = : in JSON path
export ConnectionStrings__DefaultConnection="Server=prod-db;..."

# Windows (PowerShell)
$env:ASPNETCORE_ENVIRONMENT = "Production"
$env:AppSettings__MaxPageSize = "100"
```

```csharp
// Reading environment variables in code
string env = Environment.GetEnvironmentVariable("ASPNETCORE_ENVIRONMENT")
             ?? "Production";

// Or via IHostEnvironment (injected by the framework)
public class MyService
{
    public MyService(IHostEnvironment env)
    {
        if (env.IsDevelopment())
            Console.WriteLine("Running in Development");
        if (env.IsProduction())
            Console.WriteLine("Running in Production");

        Console.WriteLine($"ContentRootPath: {env.ContentRootPath}");
    }
}
```

---

### :pencil: Section 4 — Exercises

1. Create a console app that reads its name, version, and a list of supported languages from `appsettings.json` using strongly typed options.
2. Add `appsettings.Development.json` that overrides the app name and enables a feature flag. Verify it takes precedence when `DOTNET_ENVIRONMENT=Development`.
3. Use `dotnet user-secrets` to store a database password. Read it in the app and confirm it never appears in `appsettings.json`.

---

## Section 5 — Dependency Injection <a name = "section-5"></a>

### 5.1 What is Dependency Injection?

**Dependency Injection (DI)** is a design pattern where objects receive their dependencies from an external source rather than creating them themselves. .NET has a built-in **IoC container** that manages the creation and lifetime of objects.

**Without DI (tightly coupled):**
```csharp
public class OrderService
{
    private readonly EmailService _emailService;

    public OrderService()
    {
        _emailService = new EmailService(); // hard dependency — hard to test
    }
}
```

**With DI (loosely coupled):**
```csharp
public class OrderService
{
    private readonly IEmailService _emailService;

    public OrderService(IEmailService emailService) // injected from outside
    {
        _emailService = emailService;
    }
}
```

Benefits: **testability** (swap with a mock), **flexibility** (swap implementations without changing consumers), **maintainability**.

---

### 5.2 Service Lifetimes

| Lifetime | Created | Destroyed | Shared? |
|----------|---------|-----------|---------|
| **Singleton** | Once at app start | App shutdown | Shared by all requests |
| **Scoped** | Once per scope (HTTP request) | End of scope | Shared within a request |
| **Transient** | Every time requested | After each use | Never shared |

```csharp
// Registering services
builder.Services.AddSingleton<IConfiguration>(builder.Configuration);
builder.Services.AddSingleton<ICacheService, MemoryCacheService>();

builder.Services.AddScoped<IProductRepository, ProductRepository>();
builder.Services.AddScoped<IOrderService, OrderService>();

builder.Services.AddTransient<IEmailService, SmtpEmailService>();
builder.Services.AddTransient<IPasswordHasher, BCryptPasswordHasher>();
```

**Rule of thumb:**
- `Singleton` → stateless services, caches, configs. **Thread-safe only.**
- `Scoped` → anything that wraps a unit-of-work (e.g., `DbContext`).
- `Transient` → lightweight, stateless services where fresh instances are cheap.

> ⚠️ **Captive dependency**: never inject a `Scoped` or `Transient` service into a `Singleton`. The shorter-lived service gets "captured" and lives as long as the singleton.

---

### 5.3 Registering Services

```csharp
// ─── Interface → Implementation ───────────────────────────
builder.Services.AddScoped<IProductService, ProductService>();

// ─── Concrete class (no interface) ────────────────────────
builder.Services.AddScoped<ProductService>();

// ─── Factory registration ──────────────────────────────────
builder.Services.AddScoped<IEmailService>(sp =>
{
    var settings = sp.GetRequiredService<IOptions<SmtpSettings>>().Value;
    return new SmtpEmailService(settings.Host, settings.Port);
});

// ─── Multiple implementations of the same interface ───────
builder.Services.AddTransient<INotificationChannel, EmailNotification>();
builder.Services.AddTransient<INotificationChannel, SmsNotification>();
builder.Services.AddTransient<INotificationChannel, PushNotification>();
// Inject IEnumerable<INotificationChannel> to get all three

// ─── Open generic registration ─────────────────────────────
builder.Services.AddScoped(typeof(IRepository<>), typeof(Repository<>));

// ─── Conditional registration ──────────────────────────────
if (builder.Environment.IsDevelopment())
    builder.Services.AddScoped<IPaymentGateway, MockPaymentGateway>();
else
    builder.Services.AddScoped<IPaymentGateway, StripePaymentGateway>();

// ─── TryAdd — only registers if not already registered ────
builder.Services.TryAddScoped<IProductService, ProductService>();
```

---

### 5.4 Injecting Services

```csharp
// Constructor injection (most common — preferred)
public class OrderService : IOrderService
{
    private readonly IOrderRepository _orderRepo;
    private readonly IEmailService    _email;
    private readonly ILogger<OrderService> _logger;

    public OrderService(
        IOrderRepository orderRepo,
        IEmailService email,
        ILogger<OrderService> logger)
    {
        _orderRepo = orderRepo;
        _email     = email;
        _logger    = logger;
    }
}

// Resolving manually from the container (service locator pattern — use sparingly)
using var scope = app.Services.CreateScope();
var service = scope.ServiceProvider.GetRequiredService<IProductService>();

// GetService vs GetRequiredService
var optional = sp.GetService<IOptionalFeature>(); // returns null if not registered
var required = sp.GetRequiredService<IProductService>(); // throws if not registered

// Injecting IEnumerable<T> to get all implementations
public class NotificationService
{
    private readonly IEnumerable<INotificationChannel> _channels;

    public NotificationService(IEnumerable<INotificationChannel> channels)
        => _channels = channels;

    public async Task NotifyAllAsync(string message)
    {
        foreach (var channel in _channels)
            await channel.SendAsync(message);
    }
}
```

---

### 5.5 Organizing Registrations with Extension Methods

As the app grows, `Program.cs` can get cluttered. Use extension methods to organize registrations:

```csharp
// Extensions/ServiceCollectionExtensions.cs
public static class ServiceCollectionExtensions
{
    public static IServiceCollection AddApplicationServices(
        this IServiceCollection services)
    {
        services.AddScoped<IProductService, ProductService>();
        services.AddScoped<IOrderService, OrderService>();
        services.AddScoped<IInventoryService, InventoryService>();
        return services;
    }

    public static IServiceCollection AddRepositories(
        this IServiceCollection services)
    {
        services.AddScoped<IProductRepository, ProductRepository>();
        services.AddScoped<IOrderRepository, OrderRepository>();
        return services;
    }

    public static IServiceCollection AddInfrastructure(
        this IServiceCollection services,
        IConfiguration config)
    {
        services.AddDbContext<AppDbContext>(options =>
            options.UseSqlite(config.GetConnectionString("DefaultConnection")));
        services.AddMemoryCache();
        return services;
    }
}

// Clean Program.cs
builder.Services
    .AddApplicationServices()
    .AddRepositories()
    .AddInfrastructure(builder.Configuration);
```

---

### :pencil: Section 5 — Exercises

1. Create an `IGreetingService` interface with two implementations: `FormalGreeting` and `CasualGreeting`. Register one and swap them without changing any consumer code.
2. Register `INotificationChannel` with three implementations (Email, SMS, Push). Inject `IEnumerable<INotificationChannel>` and send a notification through all three.
3. Demonstrate the captive dependency problem: register a `Scoped` service inside a `Singleton` and observe the `InvalidOperationException`. Fix it using `IServiceScopeFactory`.

---

## Section 6 — Exception Handling <a name = "section-6"></a>

### 6.1 Try / Catch / Finally

```csharp
try
{
    int[] numbers = { 1, 2, 3 };
    int result = numbers[10]; // IndexOutOfRangeException
}
catch (IndexOutOfRangeException ex)
{
    Console.WriteLine($"Index error: {ex.Message}");
}
catch (ArgumentNullException ex)
{
    Console.WriteLine($"Null argument: {ex.ParamName}");
}
catch (Exception ex) when (ex.Message.Contains("timeout")) // exception filter
{
    Console.WriteLine("Timeout occurred.");
}
catch (Exception ex)
{
    Console.WriteLine($"Unexpected error: {ex.Message}");
    throw; // re-throw preserving the original stack trace
}
finally
{
    Console.WriteLine("Runs always — use for cleanup/resource disposal.");
}
```

---

### 6.2 Custom Exceptions

```csharp
// Base domain exception
public class DomainException : Exception
{
    public DomainException(string message) : base(message) { }
    public DomainException(string message, Exception inner) : base(message, inner) { }
}

// Specific exceptions
public class NotFoundException : DomainException
{
    public string ResourceType { get; }
    public object ResourceId   { get; }

    public NotFoundException(string resourceType, object resourceId)
        : base($"{resourceType} with id '{resourceId}' was not found.")
    {
        ResourceType = resourceType;
        ResourceId   = resourceId;
    }
}

public class ValidationException : DomainException
{
    public IReadOnlyDictionary<string, string[]> Errors { get; }

    public ValidationException(Dictionary<string, string[]> errors)
        : base("One or more validation errors occurred.")
    {
        Errors = errors;
    }
}

public class ConflictException : DomainException
{
    public ConflictException(string message) : base(message) { }
}

public class ForbiddenException : DomainException
{
    public ForbiddenException(string message = "Access denied.") : base(message) { }
}

// Usage
var product = await _repo.GetByIdAsync(id)
    ?? throw new NotFoundException(nameof(Product), id);

if (await _repo.ExistsByNameAsync(dto.Name))
    throw new ConflictException($"A product named '{dto.Name}' already exists.");
```

---

### 6.3 Using for Resource Disposal

```csharp
// using statement — Dispose() is called automatically when leaving scope
using var file = new StreamWriter("output.txt");
file.WriteLine("Hello, .NET!");
// file.Dispose() called here automatically

// using block (older syntax, equivalent)
using (var connection = new SqlConnection(connectionString))
{
    connection.Open();
    // use connection
} // connection.Dispose() called here

// await using — for async IAsyncDisposable
await using var client = new HttpClient();
var response = await client.GetAsync("https://api.example.com");

// Multiple usings
using var reader = new StreamReader("input.txt");
using var writer = new StreamWriter("output.txt");

while (!reader.EndOfStream)
{
    string? line = await reader.ReadLineAsync();
    if (line != null)
        await writer.WriteLineAsync(line.ToUpper());
}
```

---

### 6.4 Result Pattern (alternative to exceptions)

For expected failures (not bugs), some prefer returning a `Result<T>` instead of throwing exceptions:

```csharp
public class Result<T>
{
    public bool   IsSuccess { get; }
    public T?     Value     { get; }
    public string Error     { get; }

    private Result(T value)        { IsSuccess = true;  Value = value; Error = string.Empty; }
    private Result(string error)   { IsSuccess = false; Value = default; Error = error; }

    public static Result<T> Success(T value) => new(value);
    public static Result<T> Failure(string error) => new(error);
}

// Usage in a service
public async Task<Result<Product>> GetByIdAsync(int id)
{
    var product = await _repo.GetByIdAsync(id);
    return product is null
        ? Result<Product>.Failure($"Product {id} not found.")
        : Result<Product>.Success(product);
}

// Consuming
var result = await _service.GetByIdAsync(id);
if (!result.IsSuccess)
    return NotFound(new { message = result.Error });

return Ok(result.Value);
```

---

### :pencil: Section 6 — Exercises

1. Create a hierarchy of custom exceptions: `AppException → NotFoundException`, `ValidationException`, `ConflictException`. Write a method that throws each based on input and handle them in a `switch` expression.
2. Write a `FileProcessor` class that reads a file, processes each line, and disposes resources correctly using `using`. Handle `FileNotFoundException` and `IOException` separately.
3. Implement a `Result<T>` class. Refactor a service method to return `Result<T>` instead of throwing exceptions and update the consumer accordingly.

---

## Section 7 — Logging <a name = "section-7"></a>

### 7.1 Built-in Logging

.NET has a built-in, extensible logging system. `ILogger<T>` is the primary interface.

```csharp
// Inject ILogger<T>
public class ProductService : IProductService
{
    private readonly ILogger<ProductService> _logger;

    public ProductService(ILogger<ProductService> logger)
        => _logger = logger;

    public async Task<Product?> GetByIdAsync(int id)
    {
        _logger.LogDebug("Fetching product {ProductId}", id);

        var product = await _repo.GetByIdAsync(id);

        if (product is null)
            _logger.LogWarning("Product {ProductId} was not found", id);
        else
            _logger.LogInformation("Product {ProductId} fetched: {Name}", id, product.Name);

        return product;
    }
}
```

**Log Levels (lowest → highest severity):**

| Level | Method | When to use |
|-------|--------|-------------|
| `Trace` | `LogTrace` | Very detailed diagnostics — only for deep debugging |
| `Debug` | `LogDebug` | Useful during development |
| `Information` | `LogInformation` | Normal operational events |
| `Warning` | `LogWarning` | Unexpected but non-critical events |
| `Error` | `LogError` | Failures that need attention |
| `Critical` | `LogCritical` | App-breaking failures |

---

### 7.2 Structured Logging

Always use **message templates** (with `{Placeholders}`) instead of string interpolation. This enables structured logging providers to index individual fields.

```csharp
// ✅ Correct — structured logging (searchable fields)
_logger.LogInformation("User {UserId} purchased {ProductName} for {Price:C}", userId, product.Name, product.Price);

// ❌ Wrong — string interpolation loses structure
_logger.LogInformation($"User {userId} purchased {product.Name} for {product.Price:C}");
```

---

### 7.3 Logging Configuration

```json
// appsettings.json
{
  "Logging": {
    "LogLevel": {
      "Default": "Information",          // minimum level for all categories
      "Microsoft": "Warning",            // suppress verbose Microsoft logs
      "Microsoft.Hosting.Lifetime": "Information",
      "Microsoft.EntityFrameworkCore": "Warning",
      "MyApp.Services": "Debug"          // verbose for your own services
    }
  }
}
```

---

### 7.4 Serilog (popular structured logging library)

```bash
dotnet add package Serilog.AspNetCore
dotnet add package Serilog.Sinks.Console
dotnet add package Serilog.Sinks.File
dotnet add package Serilog.Sinks.Seq      # optional — structured log server
```

```csharp
// Program.cs
using Serilog;

Log.Logger = new LoggerConfiguration()
    .MinimumLevel.Information()
    .MinimumLevel.Override("Microsoft", LogEventLevel.Warning)
    .Enrich.FromLogContext()
    .Enrich.WithMachineName()
    .Enrich.WithEnvironmentName()
    .WriteTo.Console(outputTemplate:
        "[{Timestamp:HH:mm:ss} {Level:u3}] {SourceContext}: {Message:lj}{NewLine}{Exception}")
    .WriteTo.File(
        path: "logs/app-.log",
        rollingInterval: RollingInterval.Day,
        retainedFileCountLimit: 30)
    // .WriteTo.Seq("http://localhost:5341") // optional
    .CreateLogger();

try
{
    Log.Information("Starting application...");
    var builder = WebApplication.CreateBuilder(args);
    builder.Host.UseSerilog(); // replace default logger with Serilog
    // ... rest of setup
    app.Run();
}
catch (Exception ex)
{
    Log.Fatal(ex, "Application terminated unexpectedly.");
}
finally
{
    Log.CloseAndFlush();
}
```

---

### :pencil: Section 7 — Exercises

1. Add `ILogger<T>` to a service. Log at `Debug` level on entry, `Information` on success, and `Error` on exception. Adjust `LogLevel` in `appsettings.json` to control output.
2. Replace the built-in logger with Serilog. Configure it to write to both the console and a rolling file. Verify structured properties appear in the output.
3. Write a middleware that logs every request and response: method, path, status code, and elapsed time. Use `LogInformation` with a structured template.

---

## Section 8 — NuGet & Package Management <a name = "section-8"></a>

### 8.1 NuGet Essentials

**NuGet** is the package manager for .NET. Packages are published to [nuget.org](https://www.nuget.org).

```bash
# Search for packages
dotnet add package <name>                    # latest stable version
dotnet add package <name> --version 3.1.1   # specific version
dotnet add package <name> --prerelease       # include pre-release

# List and inspect
dotnet list package                          # all packages in project
dotnet list package --outdated               # show newer versions available
dotnet list package --vulnerable             # show packages with known CVEs

# Remove
dotnet remove package <name>

# Restore (downloads all listed packages)
dotnet restore
```

---

### 8.2 Essential .NET Packages

```bash
# ─── Serialization ──────────────────────────────────────────
dotnet add package Newtonsoft.Json           # popular JSON library
dotnet add package System.Text.Json         # built-in, fast (no install needed)

# ─── Logging ────────────────────────────────────────────────
dotnet add package Serilog.AspNetCore
dotnet add package Serilog.Sinks.Console
dotnet add package Serilog.Sinks.File

# ─── ORM / Database ─────────────────────────────────────────
dotnet add package Microsoft.EntityFrameworkCore
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
dotnet add package Dapper                   # lightweight micro-ORM

# ─── HTTP / REST Clients ────────────────────────────────────
dotnet add package Refit                    # typed REST client
dotnet add package Polly                    # resilience (retry, circuit breaker)

# ─── Mapping ────────────────────────────────────────────────
dotnet add package AutoMapper
dotnet add package AutoMapper.Extensions.Microsoft.DependencyInjection
dotnet add package Mapster                  # alternative, faster

# ─── Validation ─────────────────────────────────────────────
dotnet add package FluentValidation
dotnet add package FluentValidation.AspNetCore

# ─── Testing ────────────────────────────────────────────────
dotnet add package xunit
dotnet add package Moq
dotnet add package FluentAssertions
dotnet add package Microsoft.AspNetCore.Mvc.Testing

# ─── Security ───────────────────────────────────────────────
dotnet add package Microsoft.AspNetCore.Authentication.JwtBearer
dotnet add package BCrypt.Net-Next

# ─── Background Jobs ────────────────────────────────────────
dotnet add package Hangfire.AspNetCore
dotnet add package Quartz.Extensions.Hosting
```

---

### 8.3 NuGet Configuration

```xml
<!-- NuGet.Config — custom package sources -->
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <packageSources>
    <add key="nuget.org" value="https://api.nuget.org/v3/index.json" />
    <add key="MyCompanyFeed" value="https://pkgs.dev.azure.com/mycompany/_packaging/feed/nuget/v3/index.json" />
  </packageSources>
  <packageSourceCredentials>
    <MyCompanyFeed>
      <add key="Username" value="user" />
      <add key="ClearTextPassword" value="token" />
    </MyCompanyFeed>
  </packageSourceCredentials>
</configuration>
```

---

### :pencil: Section 8 — Exercises

1. Add `AutoMapper` to a project. Create a mapping between `Product` and `ProductDto`. Use it to map a list of products.
2. Add `FluentValidation`. Write a `CreateProductValidator` that validates name length, price range, and required fields. Integrate it with ASP.NET Core's pipeline.
3. Run `dotnet list package --vulnerable` on a project with several packages. Research one of the reported CVEs.

---

## Section 9 — Testing <a name = "section-9"></a>

### 9.1 Unit Testing with xUnit

**Unit tests** test a single unit of code in isolation, with all dependencies mocked.

```bash
dotnet new xunit -n MyApp.Tests
dotnet add MyApp.Tests/MyApp.Tests.csproj reference MyApp/MyApp.csproj
dotnet add MyApp.Tests package Moq
dotnet add MyApp.Tests package FluentAssertions
```

```csharp
// Tests/Services/ProductServiceTests.cs
public class ProductServiceTests
{
    private readonly Mock<IProductRepository> _repoMock;
    private readonly Mock<ILogger<ProductService>> _loggerMock;
    private readonly ProductService _sut; // SUT = System Under Test

    public ProductServiceTests()
    {
        _repoMock   = new Mock<IProductRepository>();
        _loggerMock = new Mock<ILogger<ProductService>>();
        _sut        = new ProductService(_repoMock.Object, _loggerMock.Object);
    }

    // [Fact] — single test case
    [Fact]
    public async Task GetByIdAsync_WhenProductExists_ReturnsProduct()
    {
        // Arrange
        var product = new Product { Id = 1, Name = "Laptop", Price = 2999m };
        _repoMock.Setup(r => r.GetByIdAsync(1)).ReturnsAsync(product);

        // Act
        var result = await _sut.GetByIdAsync(1);

        // Assert
        result.Should().NotBeNull();
        result!.Name.Should().Be("Laptop");
        _repoMock.Verify(r => r.GetByIdAsync(1), Times.Once);
    }

    [Fact]
    public async Task GetByIdAsync_WhenProductNotFound_ReturnsNull()
    {
        _repoMock.Setup(r => r.GetByIdAsync(99)).ReturnsAsync((Product?)null);

        var result = await _sut.GetByIdAsync(99);

        result.Should().BeNull();
    }

    // [Theory] — multiple test cases with [InlineData]
    [Theory]
    [InlineData("",        10.00, "Name is required")]
    [InlineData("A",       10.00, "Name too short")]
    [InlineData("ValidName", 0,   "Price must be positive")]
    [InlineData("ValidName", -5,  "Price must be positive")]
    public async Task CreateAsync_WithInvalidInput_ThrowsValidationException(
        string name, decimal price, string reason)
    {
        var dto = new CreateProductDto { Name = name, Price = price };

        await _sut.Invoking(s => s.CreateAsync(dto))
                  .Should()
                  .ThrowAsync<ValidationException>(reason);
    }

    [Fact]
    public async Task CreateAsync_WhenNameExists_ThrowsConflictException()
    {
        _repoMock.Setup(r => r.ExistsByNameAsync("Laptop")).ReturnsAsync(true);

        var dto = new CreateProductDto { Name = "Laptop", Price = 999m };

        await _sut.Invoking(s => s.CreateAsync(dto))
                  .Should()
                  .ThrowAsync<ConflictException>();
    }
}
```

---

### 9.2 Mocking with Moq

```csharp
var mock = new Mock<IProductRepository>();

// Setup return value
mock.Setup(r => r.GetByIdAsync(1))
    .ReturnsAsync(new Product { Id = 1, Name = "Test" });

// Setup with any argument
mock.Setup(r => r.GetByIdAsync(It.IsAny<int>()))
    .ReturnsAsync(new Product { Id = 99, Name = "Any" });

// Setup to throw
mock.Setup(r => r.GetByIdAsync(-1))
    .ThrowsAsync(new ArgumentException("Invalid id"));

// Verify a method was called
mock.Verify(r => r.GetByIdAsync(1), Times.Once);
mock.Verify(r => r.CreateAsync(It.IsAny<Product>()), Times.Never);

// Capture arguments
Product? capturedProduct = null;
mock.Setup(r => r.CreateAsync(It.IsAny<Product>()))
    .Callback<Product>(p => capturedProduct = p)
    .ReturnsAsync((Product p) => p);

// Verify with captured argument
capturedProduct!.Name.Should().Be("Laptop");
```

---

### 9.3 Assertions with FluentAssertions

```csharp
// Value assertions
result.Should().Be(42);
result.Should().NotBe(0);
result.Should().BeGreaterThan(10);
result.Should().BeInRange(1, 100);
result.Should().BeNull();
result.Should().NotBeNull();

// String assertions
name.Should().Be("Alice");
name.Should().StartWith("Al");
name.Should().Contain("ice");
name.Should().HaveLength(5);
name.Should().NotBeNullOrWhiteSpace();

// Collection assertions
list.Should().HaveCount(3);
list.Should().Contain("item");
list.Should().NotBeEmpty();
list.Should().BeInAscendingOrder();
list.Should().ContainSingle(x => x.Id == 1);
list.Should().AllSatisfy(x => x.IsActive.Should().BeTrue());

// Exception assertions
action.Should().Throw<NotFoundException>()
      .WithMessage("*not found*")
      .And.ResourceType.Should().Be("Product");

await asyncAction.Should().ThrowAsync<ValidationException>();
action.Should().NotThrow();

// Object assertions
product.Should().BeEquivalentTo(expected,
    options => options.Excluding(p => p.CreatedAt));
```

---

### 9.4 Code Coverage

```bash
# Run tests with coverage collection
dotnet test --collect:"XPlat Code Coverage"

# Install report generator
dotnet tool install --global dotnet-reportgenerator-globaltool

# Generate HTML report from coverage results
reportgenerator \
  -reports:"**/coverage.cobertura.xml" \
  -targetdir:"coverage-report" \
  -reporttypes:Html

# Open coverage-report/index.html in browser
```

---

### :pencil: Section 9 — Exercises

1. Write unit tests for a `CartService` with methods `AddItem`, `RemoveItem`, `GetTotal`, and `Clear`. Mock the product repository. Achieve at least 90% branch coverage.
2. Use `[Theory]` with `[MemberData]` to provide complex test data (objects) to a test method.
3. Generate an HTML coverage report for your test project. Identify uncovered branches and write tests to cover them.

---

## :books: References <a name = "references"></a>

| Resource | Link |
|----------|------|
| .NET Documentation | [docs.microsoft.com/dotnet](https://docs.microsoft.com/en-us/dotnet/) |
| Microsoft Learn — .NET | [learn.microsoft.com/dotnet](https://learn.microsoft.com/en-us/dotnet/) |
| .NET and C# for Beginners Roadmap | [roadmap.sh/ai/course](https://roadmap.sh/ai/course/net-and-c-for-beginners-a-comprehensive-guide) |
| .NET Runtime GitHub | [github.com/dotnet/runtime](https://github.com/dotnet/runtime) |
| NuGet Gallery | [nuget.org](https://www.nuget.org/) |
| Serilog | [serilog.net](https://serilog.net/) |
| xUnit Documentation | [xunit.net](https://xunit.net/) |
| Moq Documentation | [github.com/moq/moq4](https://github.com/moq/moq4) |
| FluentAssertions | [fluentassertions.com](https://fluentassertions.com/) |
| .NET Performance | [github.com/dotnet/performance](https://github.com/dotnet/performance) |

---

## :page_with_curl: License

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)

This project is licensed under the MIT License.

---

<p align="center">Made with ❤️ for .NET developers worldwide</p>
