## Local development

### Pre-requisites

First install:

* Node.js (8.11.1 or later)
* Npm (5.6.0 or later)
* .NET 7.0 SDK (dotnet should be on your path)

### Build and run the extension

To run and develop the extension do the following:

* Run `npm i`
* Run `npm i -g gulp`
* Run `gulp roslyn:languageserver` (this will download the Roslyn language server executables as specified by the version in the [package.json](package.json))
* _Optional:_ run `gulp razor:languageserver` to install the Razor language server.
* Open in Visual Studio Code (`code .`)
* _Optional:_ run `npm run watch`, make code changes
* Press <kbd>F5</kbd> to debug

To **test** do the following: `npm run test` or <kbd>F5</kbd> in VS Code with the "Launch Tests" debug configuration.

### Using a locally developed Roslyn server

https://dnceng.visualstudio.com/internal/_git/dotnet-roslyn?version=GBfeatures%2Flsp_tools_host contains the server implementation.  Follow the instructions there to build the repo as normal.  Once built, the server executable will be located in the build output directory, typically 

`$roslynRepoRoot/artifacts/bin/Microsoft.CodeAnalysis.LanguageServer/Debug/net7.0/Microsoft.CodeAnalysis.LanguageServer.exe`

depending on which configuration is built.  Then, launch the extension here and change the VSCode setting `dotnet.server.path` to point to the Roslyn executable path you built above and restart the language server.

If you need to debug the server, you can set the VSCode setting `dotnet.server.waitForDebugger` to true.  This will trigger a `Debugger.Launch()` on the server side as it starts.

### Creating VSIXs

VSIXs can be created using the gulp command `gulp vsix:release:package`.  This will create all the platform specific VSIXs that you can then install manually in VSCode.

## Updating the Roslyn server version

To update the version of the roslyn server used by the extension do the following:
1.  Find the the Roslyn signed build you want from [here](https://dnceng.visualstudio.com/internal/_build?definitionId=327&branchFilter=246637%2C246637).  Typically the latest successful build is fine.
2.  In the official build stage, look for the `Publish Language Server Executables` step.  In there you will see it publishing the `Microsoft.CodeAnalysis.LanguageServer` package with some version, e.g. `4.6.0-3.23158.4`.  Take note of that version number.
3.  In the [package.json](package.json) inside the `defaults` section update the `roslyn` key to point to the version number you found above in step 2.
4.  Build and test the change (make sure to run `gulp roslyn:languageserver` to get the new version!).  If everything looks good, submit a PR.