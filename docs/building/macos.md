# Build PowerShell on macOS

This guide supplements the [Linux instructions](./linux.md), as
building on macOS is almost identical.

.NET Core 2.0 (and by transitivity, us) only supports macOS 10.12.

## Environment

You will want [Homebrew](http://brew.sh/), the missing package manager for macOS.
Once installed, follow the same instructions to download and
install a self-hosted copy of PowerShell on your macOS machine,
and use`Start-PSBootstrap` to install the dependencies.

The `Start-PSBootstrap` function does the following:

- Uses `brew` to install CMake, OpenSSL, and GNU WGet
- Uninstalls any prior versions of .NET CLI
- Downloads and installs a preview version of .NET Core SDK 2.0 to `~/.dotnet`

If you want to use `dotnet` outside of `Start-PSBuild`,
add `~/.dotnet` to your `PATH` environment variable.

### error: Too many open files

Due to a [bug][809] in NuGet, the `dotnet restore` command will fail without the limit increased.
Run `ulimit -n 2048` to fix this in your session;
add it to your shell's profile to fix it permanently.

We cannot do this for you in the build module due to #[847][].

[809]: https://github.com/dotnet/cli/issues/809
[847]: https://github.com/PowerShell/PowerShell/issues/847

## Build using our module

Instead of installing the Ubuntu package of PowerShell,
download the `pkg` from our GitHub releases page using your browser, complete the wizard,
start a `powershell` session, and use `Start-PSBuild` from the module.

After building, PowerShell will be at `./src/powershell-unix/bin/Linux/netcoreapp2.0/osx.10.12-x64/publish/powershell`.
Note that configuration is still `Linux` because it would be silly to make yet another separate configuration when it's used solely to work-around a CLI issue.
