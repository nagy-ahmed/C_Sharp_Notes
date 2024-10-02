# .NET
We found .NET sdk , runtime , what is the diffrence between?  
- `SDK` (Software Development Kit) kit for developer to access all he need 
- `Runtime` , just for running the application in the client

## .NET CLI 
**cross platform tool chain command line interface using for developing , building , running , publishing .NET APPs**
[Overview](https://learn.microsoft.com/en-us/dotnet/core/tools/)  

### some basic DOS commands
- `dir` directories
- `cd` change directory
- `cd..` using to step back
- `mkdir` make directory
  - `makdir folder1, folder2, folder3` 
- `rmdir` folder1  
- `echo file1.txt` using to make file with name file1.txt
- `notepad file1.txt` using to open file using notepad prog

## dotnet driver 
1. options
   - dotnet --help
   - dotnet --info
   - dotnet --list-sdks  -> all sdk installed 
   - dotnet --version  -> last version

2. commands
   - `new` -> new project or file  
    `dotnet new --helper` to see how to use it    
        - `dotnet new --list` used to get all tempelets , can write `<name>` or not to search 
        - dotnet new console 
      - `name` used to specify neme of the project you create 
      - `dotnet new console --name Demo`
      - `dotnet new console --framework net5.0 --name Demo01`
      - `dotnet new sln --name DemoSln`

   - `build` -> build the project to make dll and exe
     - `dotnet build`
     - to run it can do `dotnet .\bin\Debug\net5.0\Demo01.dll`      
   - `run` build dll and run it
     - `dotnet run` inside the project solution
   - `add`    
        - `reference`   
         if we have more project in same sol and want to add to proj ref of anther project we make     
        `dotnet add .\APP\APP.csproj reference .\ABC\ABC.csproj`   
        abc ref added to GHI
        - `package`  add package same as nuget package
            `dotnet add package newtonsoft.json`  
            `dotnet remove package newtonsoft.json`
   - `sln`   
     add project to sln  
     `dotnet sln DemoSln add  .\ABC\ABC.csproj`  
     `dotnet sln DemoSln remove  .\ABC\ABC.csproj`