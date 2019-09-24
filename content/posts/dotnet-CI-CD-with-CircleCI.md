---
title: "Dotnet(.NET) CI CD With CircleCI"
date: 2019-09-24T17:30:53+03:00
draft: false
toc: false
images:
tags: 
  - dotnet
  - circleci
  - ci/cd
---

## What is .NET sdk

[.NET Core SDK](https://docs.microsoft.com/en-us/dotnet/core/sdk) consists of two parts: the *.NET Core runtime* and the *.NET CLI*. The runtime allows you to run .NET Core applications, whereas the CLI tools are a command-line interface that allows you to develop .NET Core applications.

In this blog post, we will build a small **.NET Core console** application, then, we will use [CircleCi](https://circleci.com) to show how continuous Integrations can be setup.

### Step 1

- [Download](https://dotnet.microsoft.com/download) and install the .NET Core SDK.
- You should see .NET CLI’s version number when running:

```bash
$ dotnet --version
3.0.100
```

- For demonstration purposes we will use the [dotnet-cli](https://github.com/daumie/dotnet-cli.git) application hosted on Github. Feel free to clone and make changes as desired.

dotnet-cli's directory structure

```text
.
├── Program.cs
├── README.md
├── dotnet-cli.csproj

3 files
```

`Program.cs`: the C# source file for our application.

`dotnetcore-ci.csproj`: a C# project for our application.

These are the main files  .NET CLI needs to build and run our application.

- Resolve the applications dependencies with:

```bash
dotnet restore
```

- Next, we can build our application with:

```bash
dotnet build
```

The above command will build the application and  output some diagnostic information i.e

```bash
Microsoft (R) Build Engine version 16.3.0+0f4c62fea for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Restore completed in 275.89 ms for /Users/dominic/workspace/personal/dotnet-cli/dotnet-cli.csproj.
  dotnet-cli -> /Users/dominic/workspace/personal/dotnet-cli/bin/Debug/netcoreapp2.2/dotnet-cli.dll

Build succeeded.
    0 Warning(s)
    0 Error(s)

Time Elapsed 00:00:02.40
```

- Let's now run the application with:

```bash
$ dotnet run
  _   _      _ _         __        __         _     _ _
 | | | | ___| | | ___    \ \      / /__  _ __| | __| | |
 | |_| |/ _ \ | |/ _ \    \ \ /\ / / _ \| '__| |/ _` | |
 |  _  |  __/ | | (_) |    \ V  V / (_) | |  | | (_| |_|
 |_| |_|\___|_|_|\___( )    \_/\_/ \___/|_|  |_|\__,_(_)
                     |/
```

### Adding CI/CD using CircleCI

Add `.circleci` config to the application's root directory by creating a directory *.circleci* and add a *config.yml* file inside.

```bash
mkdir .circleci
touch .circleci/config.yml
```

Add the following content to the *config.yml* file

```yaml
version: 2
jobs:
  build:
    working_directory: /temp
    docker:
      - image: microsoft/dotnet:sdk
    environment:
      DOTNET_SKIP_FIRST_TIME_EXPERIENCE: 1
      DOTNET_CLI_TELEMETRY_OPTOUT: 1
    steps:
      - checkout
      - run: dotnet restore
      - run: dotnet build
      - run: dotnet run
```

In the first line, we specify that we want to use version 2 of CircleCI, which is currently in beta. With version 2, builds are based on Docker images. That allows us to use the official .NET Core SDK Docker image to build our application.

Once again, we also set the .NET CLI environment variables.

Finally, we specify the steps to execute. First, we checkout our repository and then we run `dotnet restore` followed by `dotnet build` then `dotnet run`

The output of the last command will be similar to what we had locally.

### Conclusion

Using CircleCI for continuous integration with .NET applications is straightforward.
