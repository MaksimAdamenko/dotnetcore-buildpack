# Heroku .NET Core Buildpack


This is the [Heroku buildpack](https://devcenter.heroku.com/articles/buildpacks) for [ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/).

[![Build status](https://ci.appveyor.com/api/projects/status/5864d533m5d35nsa?svg=true)](https://ci.appveyor.com/project/jincod/dotnetcore-buildpack)

## Usage

The Buildpack will search through the repository's folders to locate a `Startup.cs` or `Program.cs` file. If found, the `.csproj` in the containing folder will be used in the `dotnet publish <project>.csproj` command.

If repository contains **multiple** Web Applications (multiple `Startup.cs`), `PROJECT_FILE` and `PROJECT_NAME` environment variables allow to choose project for publishing.

### .NET Core latest stable

```
heroku buildpacks:set jincod/dotnetcore
```

### .NET Core edge

```
heroku buildpacks:set https://github.com/jincod/dotnetcore-buildpack
```

### Previous releases

```
heroku buildpacks:set https://github.com/jincod/dotnetcore-buildpack#version
```

Available [releases](https://github.com/jincod/dotnetcore-buildpack/releases)

More info

- [Heroku Buildpack Registry](https://devcenter.heroku.com/articles/buildpack-registry)
- [Buildpack references](https://devcenter.heroku.com/articles/buildpacks#buildpack-references)

## Entity Framework Core Migrations

You cannot run migrations with the `dotnet ef` commands once the app is built. Alternatives include:

### Enabling Automatic Migrations

- Ensure the `ASPNETCORE_ENVIRONMENT` environment variable is set to `Production`. ASP.NET Core scaffolding tools may create files that explicitly set it to `Development`. Heroku config will override this (`heroku config:set ASPNETCORE_ENVIRONMENT=Production`).
- Configure your app to automatically run migrations at startup by adding the following to the `.csproj` file:

```xml
<Target Name="PrePublishTarget" AfterTargets="Publish">
  <Exec Command="dotnet ef database update" />
</Target>
```

- Configure your connection string string appropriately, for example, for PostgreSQL:

`sslmode=Prefer;Trust Server Certificate=true`

### Manually Running Migration Scripts on the Database

- Manually run SQL scripts generated by Entity Framework Core in your app's database. For example, use PG Admin to connect your Heroku Postgres service.

## Node.js and NPM

```bash
heroku buildpacks:set jincod/dotnetcore
heroku buildpacks:add --index 1 heroku/nodejs
```

[Using Multiple Buildpacks for an App](https://devcenter.heroku.com/articles/using-multiple-buildpacks-for-an-app)

## Example

[ASP.NET Core Demo App](https://github.com/jincod/AspNet5DemoApp)

## Donation

If this project help you, you can give me a cup of coffee ☕

[![Donate](https://img.shields.io/badge/Donate-PayPal-green.svg)](https://www.paypal.me/jincod/5)
