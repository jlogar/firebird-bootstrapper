Firebird bootstrapper for ClickOnce
===================================

[Clickonce](http://msdn.microsoft.com/en-us/library/t71a733d(v=vs.100).aspx) is Microsofts deployment/autoupdate technology and [Firebird](http://www.firebirdsql.org/) an opensource RDBMS.

In general an application that requires a database server would require the server preinstalled in a previous step. By bootstrapping the Firebird installer to your clickonce deployed application you can have a single installer that includes both your application and the database server.

The bootstrapper itself is

Firebird installation
--------

Is installed in silent mode. Installer failure (return code 1) is ignored to avoid Clickonce install failure when Firebird is already installed.

Firebird return codes are described in the \doc directory of the installation.

Usage
--------------

- put the [Firebird installer executable](http://www.firebirdsql.org/en/server-packages/#Win32) (superserver) into the root directory of your local working copy
- update the `product.xml.template` file, replacing `#FIREBIRD_EXECUTABLE#` with the name of the Firebird installer executable (version dependent usually) and #WEB_LOCATION# with the place where you will be hosting the Firebird installer executable. It's best to use (and rename the file itself) lowercase to avoid any case sensitivy issues.
- save the updated file as product.xml.
- copy everything except this readme and the template file to your Windows SDK bootstrapper packages location, subdirectory `"Firebird"` (the registry should know the location `hklm:\SOFTWARE\Microsoft\Microsoft SDKs\Windows\CurrentVersion`. There's always confusion about this and no two machines have this the same if you ask me :S). On my dev machine this was `C:\Program Files (x86)\Microsoft SDKs\Windows\v7.0A\Bootstrapper\Packages`. So that would make it: `C:\Program Files (x86)\Microsoft SDKs\Windows\v7.0A\Bootstrapper\Packages\Firebird`

The bootstrapper can then be used in your bootstrapper creation task with the following ItemGroup entry:
    
    <BootstrapperItem Include="Firebird" >
 	    <ProductName>Firebird RDMBS</ProductName>
    </BootstrapperItem>

### MSBuild example

The following example will create a bootstrapper (setup.exe) that checks for .NET 4 and installs Firebird. It uses the MS provided Task `GenerateBootstrapper `.

    <?xml version="1.0" encoding="utf-8"?>
    <Project ToolsVersion="4.0" DefaultTargets="CreateBootstrapper" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
	    <ItemGroup>
		    <BootstrapperItem Include=".NETFramework,Version=v4.0" >
			    <ProductName>.NET Framework 4.0</ProductName>
		    </BootstrapperItem>
		    <BootstrapperItem Include="Microsoft.Windows.Installer.3.1" >
			    <ProductName>Windows Installer 3.1</ProductName>
		    </BootstrapperItem>
		    <BootstrapperItem Include="Firebird" >
			    <ProductName>Firebird RDMBS</ProductName>
		    </BootstrapperItem>
        </ItemGroup>
        <Target Name="CreateBootstrapper">
		    <GetFrameworkSdkPath>
		        <Output TaskParameter="Path" PropertyName="SdkPath" />
		    </GetFrameworkSdkPath>
            <GenerateBootstrapper ... BootstrapperItems="@(BootstrapperItem)" Path="$(SdkPath)\Bootstrapper" ... />
        </Target>
    </Project>


TODO
--------------

- Ask the Firebird team to return a different error code when the installation fails because of an existing server installation