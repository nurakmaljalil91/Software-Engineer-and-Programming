`dotnet ef` can be installed as either a global or local tool. Most developers prefer installing `dotnet ef` as a global tool using the following command:  
  
```bash  
dotnet tool install --global dotnet-ef
```  
To use it as a local tool, restore the dependencies of a project that declares it as a tooling dependency using a tool manifest file.  
  
Update the tool using the following command:  
  
```bash  
dotnet tool update --global dotnet-ef
```  
  
## Check version

```shell
dotnet ef --version
```
## Verify installation  
  
Run the following commands to verify that [[EF Core]] CLI tools are correctly installed:  
  
```bash  
dotnet ef
```  
  
The output from the command identifies the version of the tools in use:  
  
```bash  

                     _/\__
               ---==/    \\
         ___  ___   |.    \|\
        | __|| __|  |  )   \\\
        | _| | _|   \_/ |  //|\\
        |___||_|       /   \\\/\\

Entity Framework Core .NET Command-line Tools 2.1.3-rtm-32065  
  
<Usage documentation follows, not shown.>  

```  