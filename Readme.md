- Delete the top `#Requires -RunAsAdministrator` if you're just updating the config file (ie running without `-msiFile`)
  - I had to run twice, once as an admin with `-msiFile` to install the files, then again without admin to make sure the correct `applicationHost.config` file was being updated (when running as admin it tries to update the admin's config file, which doesn't exist)
- To run as admin I recommend starting an admin terminal, then starting a subshell with `pwsh.exe -ExecutionPolicy Unrestricted`, else you'll probably get an error about scripts being disabled.
- The install script assumes it has been run once without `-fileName` before using `-fileName` (only applicable if updating another `applicationHost.config` file besides the default)

# PowerShell Scripts to Install/Uninstall HttpPlatformHandler for IIS Express

> * Learn how to install CORS module from [this sibling project](https://github.com/lextm/iisexpress-cors).
> * Learn how to patch ASP.NET Core module on Windows 11 ARM64 from [this sibling project](https://github.com/lextm/ancm-arm64).
> * Learn how to use IIS Express with Visual Studio Code from [this sibling project](https://github.com/jexuswebserver/vscode-iis).

## The Open Source HttpPlatformHandler for IIS Express

LeXtudio Inc. started to ship its own version of HttpPlatformHandler, so you might use its IIS Express installers directly.

To learn more, you can leave this page and jump to [this post](https://docs.lextudio.com/blog/httpplatformhandler-v2/).

Only if you prefer to use Microsoft's HttpPlatformHandler v1.2 you can scroll down.

## Install

1. Install [IIS Express](https://learn.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview#installing-iis-express).
1. Download [Microsoft HttpPlatformHandler for IIS](https://www.iis.net/downloads/microsoft/httpplatformhandler) installer.
1. Install [lessmsi](https://github.com/activescott/lessmsi) tool and configure it in your PATH (so that `lessmsi` can be called by the install script).
1. Run the proper command to install HttpPlatformHandler to IIS Express,

   > If HttpPlatformHandler for IIS has been installed on this same machine in advance, then `-msiFile` switch can be omitted.
   > 
   > **For VS 2015 and above, you must use `-fileName` switch to guide the script and modify the correct `applicationHost.config` file for your Visual Studio solution file.** To learn more on how to locate the correct config file, you can [read this article](https://docs.jexusmanager.com/getting-started/features.html#add-iis-express-from-visual-studio-2015-2017-2019-solution-file).

   * At command prompt
   ``` cmd
   powershell.exe -file install.ps1 -msiFile httpPlatformHandler_amd64.msi
   ```
   * In PowerShell console   
   ``` powershell
   .\install.ps1 -msiFile httpPlatformHandler_amd64.msi
   ```

The configuration on IIS Express is exactly the same as IIS, and just make sure you are editing the right config files,

https://learn.microsoft.com/iis/extensions/httpplatformhandler/httpplatformhandler-configuration-reference

## Uninstall
1. Run `uninstall.ps1` to uninstall HttpPlatformHandler from IIS Express.

## Notes

* The current release only supports Windows x64 machines.
* The scripts must be run as administrator, because they need to copy/remove files to/from Program Files folder.
* **`install.ps1` and `uninstall.ps1` only manipulate the default IIS Express config file in current user's My Documents folder.**
* **If a custom config file needs to be modified, run the PowerShell scripts with arguments (like `install.ps1 -fileName custom.config`).**
