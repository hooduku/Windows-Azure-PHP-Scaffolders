1) Please install Windows Azure SDK 1.6 that is available in Web Platform Installer.

2) Install Windows Azure SDK for PHP. Please refer http://azurephp.interoperabilitybridges.com/articles/setup-the-windows-azure-sdk-for-php for details.

3) Update PATH environment variable to include PHP runtime and bin folder of
   the Windows Azure SDK for PHP. You can either modify system environemnt variable or set it for a specific
   command windows session using following command. Make sure to replace correct path for PHP runtime and Windows Azure
   SDK for PHP.

   SET PATH=%PATH%;C:\Program Files (x86)\PHP\v5.3\;C:\Program Files\Windows Azure SDK\bin

   Also add following line to your php.ini file. You need this to create .phar file using scaffolder.bat command.
   phar.readonly = Off
     

4) run build.bat command. This will produce joomla.phar file in build directory.

   build.bat
   ====================
   call scaffolder build -in="%PWD%\Joomla" -out="%PWD%\build\Joomla.phar"

5) Execute run_scaffolder.bat. It will create .\build\drupal folder and put all files needed for packaging.

   call scaffolder run -out="%PWD%\build\Joomla" -s="%PWD%\build\Joomla.phar" -db jbase 
   -user "demosql@xxxxxbujiw" -password "SamHoustonxxxx" -host "xxx4mbujxxw.database.windows.net" -sample_data 1 -admin_user "admin" -admin_password "admxxin"

   ==================
        --OutputPath=.\build\Joomla
        --DiagnosticsConnectionString="DefaultEndpointsProtocol=https;AccountName=*****;AccountKey=*****"
        --db=Database name
        --squser=*****@*****
        --sql_azure_password=*****
        --sql_azure_host=*****.database.windows.net
  	--sample_data =1 (sample data install) or 0 if not required.
		-- admin_user = Admin username
		-- admin-password = Admin Password
		
7) If needed, customize Joomla available in build\WebRole foTypycally user will include their custom components modules, templates and plugins
   The joomla.sql under installation/sql/sqlazure could be exported from your dev/QA environment
   The sample_data.sql under installation/sql/sqlazure could be exported from your dev/QA environment (Actual data, any data that need to be installed need to be in sample_data.sql)
   - .\build\joomla\WebRole\administrator/components
   - .\build\joomla\WebRole\components
   - .\build\joomla\WebRole\languages\
   - .\build\joomla\WebRole\administrator
   - .\build\joomla\WebRole\modules
   - .\build\drupal\WebRole\plugins
     You can also edit Config.class.php to modify the default links for Joomla, CDN and so on.
	 
8) User can also edit .\build\joomla\ServiceConfiguration.cscfg if they decide to modify settings provided while executing
   build.bat command.

   If user need to change the default VM size, he can update vmsize attribute in following line in .\build\joomla\ServiceDefinition.csdef file.
   <WebRole name="WebRole" enableNativeCodeExecution="true" vmsize="Small">

   Allowed values for vmsize attribute are "ExtraSmall", "Small", "Medium", "Large" and "ExtraLarge".

9) If you need to enable RDP acecss, you need to
   - Uncomment following lines from .\build\joomla\ServiceDefinition.csdef file
      <Import moduleName="RemoteAccess"/>
      <Import moduleName="RemoteForwarder"/>
   - Uncomment following lines from .\build\joomla\ServiceConfiguration.cscfg file
      <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.Enabled" value="true" />
      <Setting name="Microsoft.WindowsAzure.Plugins.RemoteForwarder.Enabled" value="true" />
      <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountUsername" value="****" />
      <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountEncryptedPassword" value="****" />
      <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountExpiration" value="2039-12-31T23:59:59.0000000-08:00" />
   - Refer MSDN documentation for settings above values.

10) Execute package_scaffolder.bat command. It will create follwing two files inside package folder
   .\package\joomla.cspkg and .\package\ServiceConfiguration.cscfg. Please make sure that you have installed
   FileSystemDurabilityPlugin before running package_scaffolder.bat command.
   
   package_scaffolder.bat
   ======================
   package.bat create --InputPath=.\build\joomla --RunDevFabric=false --OutputPath=.\package

11) If needed you can edit the .\package\ServiceConfiguration.cscfg

12) Finally deploy .\package\joomla.cspkg and .\package\ServiceConfiguration.cscfg file to Windows Azure.

13) Once joomla deployment is ready, visit the install.php page of your joomla site and confiure your joomla.
    i.e. visit http://yourapp.cloudapp.net/install.php
    Note: Visting homepage without installing will give an error. You must run install.php script first.

14) On Windows Azure, one must enable Windows Azure Storage module and configure it for storing all media files. Please
    refer http://azurephp.interoperabilitybridges.com/articles/how-to-deploy-joomla-to-windows-azure-using-the-joomla-scaffold for details.

15) Once your joomla is configured as per your requirement, you can increase instance count using Windows Azure portal.


See http://azurephp.interoperabilitybridges.com/articles/how-to-deploy-wordpress-using-the-windows-azure-sdk-for-php-wordpress-scaffold for usage instructions
