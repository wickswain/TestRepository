<div class="WordSection1">

# 1 - Configuring Windows

## Table of Contents

1.1  [Physical Requirementszzz](#id-1ConfiguringWindows-1.1)  
1.2  [Administrative Rights](#id-1ConfiguringWindows-1.2)  
1.3  [.NET Framework (Multiple Versions)](#id-1ConfiguringWindows-1.3)  
1.4  [Adding the _rdbuild user](#id-1ConfiguringWindows-1.4)  
1.5  [Adding DNS suffixes](#id-1ConfiguringWindows-1.5)  
1.6  [Planning installation path](#id-1ConfiguringWindows-1.6)  
1.7  [Setting the LOGPATH environment variable](#id-1ConfiguringWindows-1.7)  
1.8  [Installing IIS](#id-1ConfiguringWindows-1.8)  
       1.8.1  [How to install IIS](#id-1ConfiguringWindows-1.8.1)  
       1.8.2  [Configure default settings for all app pools](#id-1ConfiguringWindows-1.8.2)  
       1.8.3  [Check the app pools](#id-1ConfiguringWindows-1.8.3)  
1.9  [Firewall Ports and Rules](#id-1ConfiguringWindows-1.9)  
       1.9.1  [Configure Windows firewall ports using PowerShell](#id-1ConfiguringWindows-1.9.1)  
       1.9.2  [Manually configure Windows firewall ports](#id-1ConfiguringWindows-1.9.2)

<div class="MsoNormal" align="center" style="text-align:center">

* * *

</div>

## <span id="id-1ConfiguringWindows-1.1"></span>1.1        Physical requirements

Many of these steps you will need to complete on a physical, wired connection, and on-site (a docking station is a wired connection so laptops are fine if docked).

## <span id="id-1ConfiguringWindows-1.2"></span>1.2        Administrative Rights

For installing the bulk of this software, including the software below, local Administrative rights will be necessary. Without this, you cannot continue without constantly pestering someone with Administrative rights to do this for you.

## <span id="id-1ConfiguringWindows-1.3"></span>1.3      .NET Framework (Multiple Versions)

This environment requires both .NET Framework 3.0 and .NET Framework 4.6.1. To check if these have already been installed, press the <span style="color:#3366FF">Windows Key</span> and type “<span style="color:#3366FF">Turn Windows features on or off.</span>” From here, you may be able to install both .NET Frameworks, as shown below:

![](1+-+Configuring+Windows.doc_files/image001.png)

The system requires the .NET Framework 4.6.1 Developer Pack in order to build solutions in Visual Studio. This download (at <span style="color:blue">[https://www.microsoft.com/en-us/download/details.aspx?id=49978](https://www.microsoft.com/en-us/download/details.aspx?id=49978)</span>) will install everything required.

## <span id="id-1ConfiguringWindows-1.4"></span>1.4      Adding the _rdbuild user

You’ll need this user for WebAdmins.  In the <span style="color:#3366FF">Local Users and Groups</span> applet, click the <span style="color:#3366FF">Groups</span> folder (on the left), and then double-click <span style="color:#3366FF">Administrators</span>.

![](1+-+Configuring+Windows.doc_files/image002.png)

Click the “Add…” button

![](1+-+Configuring+Windows.doc_files/image003.png)

Now add “<span class="inline-comment-marker"><span data-ref="1f804e23-04f6-4288-b193-656980d2f7c1"><span style="color:#3366FF">UCN\_rdbuild</span></span></span>”, and click the <span style="color:#3366FF">Check Names</span> button.  Then click <span style="color:#3366FF">OK</span>.

![](1+-+Configuring+Windows.doc_files/image004.png)

## <span id="id-1ConfiguringWindows-1.5"></span>1.5      Adding DNS suffixes

You’ll need to add a bunch of suffixes to your <span style="color:#3366FF">TCP/IP v4 Advanced DNS</span> settings.

Go to

*   <span style="color:#3366FF">Control Panel→All Control Panel Items→Network and Sharing Center</span>
*   <span style="color:#3366FF">Change adapter settings</span>

![](1+-+Configuring+Windows.doc_files/image005.png)

Click on <span style="color:#3366FF">Ethernet</span> and select <span style="color:#3366FF">Properties</span>

A properties dialog will appear.  Scroll down and select <span style="color:#3366FF">Internet Protocol Version 4 (TCP/IPv4)</span>.  Click the <span style="color:#3366FF">Properties</span> button.

![](1+-+Configuring+Windows.doc_files/image006.png)

An additional properties dialog will appear.  Ignore everything on this dialog, and click the <span style="color:#3366FF">Advanced…</span> button on the bottom (Make sure the <span style="color:#3366FF">General</span> tab is selected).

![](1+-+Configuring+Windows.doc_files/image007.png)

Another properties dialog will appear.  Select the <span style="color:#3366FF">DNS</span> tab.

![](1+-+Configuring+Windows.doc_files/image008.png)

Click <span style="color:#3366FF">Add…</span> in the lower part of the screen, and add these domain suffixes (they need to appear in this order):

*   ucn.net
*   in.lab
*   inucn.com
*   ineur.eu
*   dradev.lab
*   i-link.net
*   <s>ucnlabext.com</s>

<div>

<table class="MsoNormalTable" border="1" cellspacing="0" cellpadding="0" width="46%" style="width:46.78%;border-collapse:collapse;border:none"><colgroup><col style="width: 11.9512%;"><col style="width: 88.0488%;"></colgroup>

<tbody>

<tr style="page-break-inside:avoid">

<td style="border:solid windowtext 1.0pt;padding:3.75pt 3.75pt 3.75pt 3.75pt">

<div>

![](1+-+Configuring+Windows.doc_files/image009.png)

</div>

</td>

<td style="border:solid windowtext 1.0pt;border-left:none;padding:3.75pt 3.75pt 3.75pt 3.75pt">

_<span style="color:blue">The DNS suffix section above may be grayed out by a group policy, preventing manual additions. This shouldn't be a problem, as most of these suffixes will be automatically added externally. The full DNS suffix search list can be viewed by running "ipconfig /all" in a command prompt.</span>__<span style="color:blue">  

_Additionally, ucnlabext.com has been removed because it would respond to any hostname._</span>_

</td>

</tr>

</tbody>

</table>

</div>

## <span id="id-1ConfiguringWindows-1.6"></span>1.6        Planning installation path

The typical development computer is equipped with two hard drives, one smaller solid state drive as the boot drive (<span style="color:blue">C:\</span>), and one disk drive which is larger (<span style="color:blue">D:\</span> or <span style="color:blue">E:\</span>).

You should seriously consider creating some of these folders on your secondary drive. The <span style="color:blue">Proj</span> folder alone will be over 90 gigabytes in size once it is fully populated. Also, most of our production machines have the web sites, the services, and the data over on the secondary drive because the log files and other things get written to the <span style="color:blue">C:\</span> drive and may create a disk space shortage if the <span style="color:blue">C:\</span> drive already has a lot of stuff on it.

Decide where you will put these directories. The document will use <span style="color:blue"><INSTALL></span> to refer to this location throughout.

You will need to create the following directory structure on your hard drive:

*   <span style="color:blue"><INSTALL>\Uploads</span>
*   <span style="color:blue"><INSTALL>\Proj</span>

*   <span style="color:blue"><INSTALL>\Proj\LogFiles</span>
*   <span style="color:blue"><INSTALL>\Proj\InstallLogs</span>
*   <span style="color:blue"><INSTALL>\Proj\InDataDBInstaller</span>

Once you have set up the <span style="color:blue">Proj</span> folder, you will need to map an environment variable <span style="color:#3366FF">LOGPATH</span> to point to the <span style="color:blue">LogFiles</span> folder under the <span style="color:blue">Proj</span> folder (<span style="color:blue"><INSTALL>\Proj\LogFiles</span>).

## <span id="id-1ConfiguringWindows-1.7"></span>1.7      Setting the LOGPATH environment variable

Add an environment variable called "<span style="color:#3366FF">LOGPATH</span>" that points to your <span style="color:blue">C:\Proj\LogFiles</span>.  To do this, go to the system properties dialog. In Windows 7 and above, you can get here by holding down the <span style="color:#3366FF">Windows key</span> and pressing the <span style="color:#3366FF">Pause/Break key</span>. In Windows 8 and above, this will take you to <span style="color:#3366FF">System</span>. Click <span style="color:#3366FF">Advanced System Settings</span> on the left and <span style="color:#3366FF">Environmental Variables</span> on the bottom right of the window that appears.

 ![](1+-+Configuring+Windows.doc_files/image010.png)

In the lower section where it says <span style="color:#3366FF">System variables</span>, click the <span style="color:#3366FF">New…</span> button.

![](1+-+Configuring+Windows.doc_files/image011.png)

Now add the environment variable (_**make sure to use <span style="color:blue"><INSTALL>\Proj\LogFiles</span>**_):

![](1+-+Configuring+Windows.doc_files/image012.png)

You should see it in the list now:

![](1+-+Configuring+Windows.doc_files/image013.png)

## <span id="id-1ConfiguringWindows-1.8"></span>1.8      Installing IIS

Internet Information Server (IIS) uses the <span style="color:blue">inetpub\wwwroot</span> folder to host websites.  You can also put the <span style="color:blue">inetpub\wwwroot</span> folder on the secondary drive. However, pointing IIS to a non-windows partition takes extra effort.  Here is a blog which talks about the process of pointing IIS to a different drive: <span style="color:blue">[http://blogs.iis.net/thomad/moving-the-iis7-inetpub-directory-to-a-different-drive](http://blogs.iis.net/thomad/moving-the-iis7-inetpub-directory-to-a-different-drive)</span>.

If you move the location, use the new location consistently throughout. The document will use the term <span style="color:blue"><WEBINSTALL></span> to refer to this root directory (including the drive letter and <span style="color:blue">inetpub\wwwroot</span>).

If your computer already has .NET 4.0 installed before you set up IIS, you will need to run the <span style="color:blue">aspnet_regiis.exe</span> tool after you finish installing IIS (see <span style="color:blue">[https://msdn.microsoft.com/en-us/library/k6h9cz8h(v=vs.140).aspx](https://msdn.microsoft.com/en-us/library/k6h9cz8h(v=vs.140).aspx))</span>  If you need to run <span style="color:blue">aspnet_regiis.exe</span>, you will first need to find <span style="color:blue">aspnet_regiis.exe</span> in the .NET folder on your machine, then open a command prompt as administrator, then run the following command:

_**<span style="color:#3366FF">aspnet_regiis –ir</span>**_

### <span id="id-1ConfiguringWindows-1.8.1"></span>1.8.1         How to install IIS

Configure IIS 8.5 – Select all options for a complete installation. (Actual version is IIS 8.5 on Windows 8.1)(7.5 is the current version for windows 7)

1.<span style="font:7.0pt &quot;Times New Roman&quot;"></span> Go into the <span style="color:#3366FF">Windows Control Panel→Programs and Features</span>.  Click on the link on the left <span style="color:#3366FF">Turn Windows features on or off</span>. Enable the following check boxes and click <span style="color:#3366FF">OK</span>.  Please note that unless a feature folder is expanded with features within it having been ticked, simply tick the initial check box and move on.  
 ![](1+-+Configuring+Windows.doc_files/image014.png)

2.<span style="font:7.0pt &quot;Times New Roman&quot;"></span> Also, make sure <span style="color:#3366FF">Internet Information Services→World Wide Web Services→Common HTTP Features→Static Content</span> is checked as seen here:  
![](1+-+Configuring+Windows.doc_files/image015.png)

Click the <span style="color:#3366FF">OK</span> button when you’re done, and Windows will install IIS.  When it’s finished, you should be able to browse to <span style="color:blue">C:\inetpub\wwwroot</span> and see the root folder.

Also, you should be able to browse to <span style="color:blue">[http://localhost](http://localhost/)</span> and get an IIS place-holder page.

### <span id="id-1ConfiguringWindows-1.8.2"></span>1.8.2         Configure default settings for all app pools

Go to the start screen and type "<span style="color:#3366FF">IIS"</span>, then start Internet Information Services (IIS) Manager. Make sure to click <span style="color:#3366FF">No</span> to the popup box that appears, if it appears.

Before we can create the new app pools, we need to set some defaults.

1.<span style="font:7.0pt &quot;Times New Roman&quot;"></span> In the <span style="color:#3366FF">Connections</span> pane on the left, expand down to the <span style="color:#3366FF">Application Pools</span> folder.

2.<span style="font:7.0pt &quot;Times New Roman&quot;"></span> In the <span style="color:#3366FF">Actions</span> pane on the far right, click the link that says <span style="color:#3366FF">Set Application Pool Defaults…</span>

3.<span style="font:7.0pt &quot;Times New Roman&quot;"></span> Change the <span style="color:#3366FF">.NET CLR Version</span> to <span style="color:#3366FF">v4.0</span>

4.<span style="font:7.0pt &quot;Times New Roman&quot;"></span> Change the <span style="color:#3366FF">Identity</span> to <span style="color:#3366FF">NetworkService</span>

5.<span style="font:7.0pt &quot;Times New Roman&quot;"></span> Click <span style="color:#3366FF">OK</span>

![](1+-+Configuring+Windows.doc_files/image016.png)

#### 1.8.2.1        Create the app pools used by inContact

Start an elevated PowerShell command prompt by running PowerShell as Administrator. Copy and paste the following code **modifying it as necessary based on <span style="color:blue"><WEBINSTALL></span>**, then hit enter.

<div>

<table class="MsoNormalTable" border="1" cellspacing="0" cellpadding="0" style="margin-left:45.0pt;border-collapse:collapse;border:none"><colgroup><col></colgroup>

<tbody>

<tr style="page-break-inside:avoid">

<td style="border:solid windowtext 1.0pt;padding:3.75pt 3.75pt 3.75pt 3.75pt">

Import-Module WebAdministration

**$appPoolList** **=** "Agent"**,** "CacheSite"**,** "CostGuardWS"**,** "DBCWS"**,** "ExtensibleReporting"**,** "inContact"**,** "inContactAPI"**,** "inContactAuthorizationServer"**,** "inControl.NET"**,** "inSideWS"**,** "L7HealthCheck"**,** "ReportService"**,** "Security"**,** "WFOFileServerIntegration"**,** "QAAUTOMATION"**,** "RESTQAAUTOMATION"**,** "WebAdmins"**,** “WebScripting”

**<span style="color:blue">foreach</span>($appPool** **<span style="color:blue">in</span>** <span style="color:blue"></span> **$appPoolList)**

**{**

      **$appPoolPath** **=** <span style="color:#7A869A">'[<span style="color:#7A869A">IIS:/apppools/</span>](http://IIS/apppools/)'</span> **+** **$appPool**

      **$applicationPath** **=** <span style="color:#7A869A">'IIS:\Sites\Default Web Site\'</span> **+** **$appPool**

      **$physicalPath** **=** <span style="color:#7A869A">'C:\inetpub\wwwroot\'</span> **+** **$appPool**

      <span style="color:#7A869A">"Creating application and application pool: "</span> **+** **$appPool**

      **<span style="color:blue">if</span>** <span style="color:blue"></span> **(**<span style="color:purple">Test-Path</span> IIS**:**\AppPools\**$appPool)**

      **{**

            <span style="color:#7A869A">"App pool already exists: "</span> **+** **$appPool**

      **}**

      **<span style="color:blue">else</span>**

      **{**

            <span style="color:purple">new-item</span> **$appPoolPath**        

      **}**

      <span style="color:purple">set-ItemProperty</span> **$appPoolPath** **-**name managedRuntimeVersion **-**value <span style="color:#7A869A">'v4.0'</span>

      <span style="color:purple">New-Item</span> **$applicationPath** **-**force **-**physicalPath **$physicalPath** **-**<span style="color:#3366FF">type</span> Application **-**applicationPool **$appPool**

**}** 

</td>

</tr>

</tbody>

</table>

</div>

 The resulting Powershell window should look similar to this:

![](1+-+Configuring+Windows.doc_files/image017.png)

If there are any errors, you will have to create the app pools manually, which is described in the next section.

#### 1.8.2.2        Manual creation of the App Pools and Applications

You can skip this section if the PowerShell script in the previous section ran without errors.

In the left pane, right click on <span style="color:#3366FF">Application Pools</span>, and from the context menu select <span style="color:#3366FF">Add Application Pool</span>.

![](1+-+Configuring+Windows.doc_files/image018.png)

Add the following application pools.  A lot of these you will never use, but you’ll need them anyway, just so WebAdmins doesn’t bomb when you do a full push.

<span style="font-size:10.0pt;font-family:Symbol">·<span style="font:7.0pt &quot;Times New Roman&quot;"></span> </span> 

<span style="font-size:10.0pt;font-family:&quot;Courier New&quot;">o<span style="font:7.0pt &quot;Times New Roman&quot;"></span> </span> 

*   Agent
*   CacheSite
*   CostGuardWS
*   DBCWS
*   ExtensibleReporting
*   inContact
*   inContactAPI
*   inContactAuthorizationServer
*   inControl.NET
*   inSideWS
*   L7HealthCheck
*   ReportService
*   Security
*   WFOFileServerIntegration
*   QAAUTOMATION
*   RESTQAAUTOMATION
*   WebAdmins
*   WebScripting

### <span id="id-1ConfiguringWindows-1.8.3"></span>1.8.3      Check the app pools

You should now have a list of app pools like so:

 ![](1+-+Configuring+Windows.doc_files/image019.png)

## <span id="id-1ConfiguringWindows-1.9"></span>1.9      Firewall Ports and Rules

Before anything can work properly, you’ll have to open several ports in the Windows firewall.

### <span id="id-1ConfiguringWindows-1.9.1"></span>1.9.1      Configure Windows firewall ports using PowerShell

There is a nifty PowerShell script you can run to configure all the ports you need.  Remember to open the PowerShell window as administrator.

<div>

<table class="MsoNormalTable" border="1" cellspacing="0" cellpadding="0" style="margin-left:45.0pt;border-collapse:collapse;border:none"><colgroup><col></colgroup>

<tbody>

<tr style="page-break-inside:avoid">

<td style="border:solid windowtext 1.0pt;padding:3.75pt 3.75pt 3.75pt 3.75pt">

<pre>netsh advfirewall firewall add rule name="Co-op Service" dir=in action=allow protocol=TCP localport=9303,8897</pre>

<pre>netsh advfirewall firewall add rule name="File Server" dir=in action=allow protocol=TCP localport=9301,8899</pre>

<pre>netsh advfirewall firewall add rule name="Media Server" dir=in action=allow protocol=TCP localport=9303,8915,9001,9300,5060,7930</pre>

<pre>netsh advfirewall firewall add rule name="SQL Server (TCP)" dir=in action=allow protocol=TCP localport=1434,1433</pre>

<pre>netsh advfirewall firewall add rule name="SQL Server (UDP)" dir=in action=allow protocol=UDP localport=1434</pre>

<pre>netsh advfirewall firewall add rule name="VC" dir=in action=allow protocol=TCP localport=9300,9310,8898</pre>

<pre>netsh advfirewall firewall add rule name="IIS Health Checks" dir=in action=allow protocol=TCP localport=80</pre>

<pre>netsh advfirewall firewall add rule name="Co-op Service" dir=out action=allow protocol=TCP localport=9303,8897</pre>

<pre>netsh advfirewall firewall add rule name="File Server" dir=out action=allow protocol=TCP localport=9301,8899</pre>

<pre>netsh advfirewall firewall add rule name="Media Server" dir=out action=allow protocol=TCP localport=9303,8915,5060,7930</pre>

<pre>netsh advfirewall firewall add rule name="SQL Server (TCP)" dir=out action=allow protocol=TCP localport=1434,1433</pre>

<pre>netsh advfirewall firewall add rule name="SQL Server (UDP)" dir=out action=allow protocol=UDP localport=1434</pre>

<pre>netsh advfirewall firewall add rule name="VC" dir=out action=allow protocol=TCP localport=9300,9310,8898</pre>

<pre>netsh advfirewall firewall add rule name="IIS Health Checks" dir=out action=allow protocol=TCP localport=80</pre>

</td>

</tr>

</tbody>

</table>

</div>

If this script ran without errors, then you may skip the next section.

### <span id="id-1ConfiguringWindows-1.9.2"></span>1.9.2      Manually configure Windows firewall ports

You can skip this section if the PowerShell script in the previous section ran without errors.

Go to <span style="color:#3366FF">Control Panel→Administrative Tools→Windows Firewall with Advanced Security</span>

Create an inbound and outbound rule for each port listed:

<div>

<table class="MsoNormalTable" border="1" cellspacing="0" cellpadding="0" width="61%" style="width:61.58%;margin-left:45.0pt;border-collapse:collapse;border:none"><colgroup><col style="width: 40.5303%;"><col style="width: 44.8864%;"><col style="width: 14.5833%;"></colgroup>

<tbody>

<tr style="page-break-inside:avoid">

<td style="border:solid windowtext 1.0pt;padding:3.75pt 3.75pt 3.75pt 3.75pt" data-highlight-colour="blue">

Service

</td>

<td style="border:solid windowtext 1.0pt;border-left:none;padding:3.75pt 3.75pt 3.75pt 3.75pt" data-highlight-colour="blue">

Port

</td>

<td style="border:solid windowtext 1.0pt;border-left:none;padding:3.75pt 3.75pt 3.75pt 3.75pt" data-highlight-colour="blue">

UDP/TCP

</td>

</tr>

<tr style="page-break-inside:avoid">

<td style="border:solid windowtext 1.0pt;border-top:none;padding:3.75pt 3.75pt 3.75pt 3.75pt">

Co-op Service

</td>

<td style="border-top:none;border-left:none;border-bottom:solid windowtext 1.0pt;
  border-right:solid windowtext 1.0pt;padding:3.75pt 3.75pt 3.75pt 3.75pt">

9303, 8897

</td>

<td style="border-top:none;border-left:none;border-bottom:solid windowtext 1.0pt;
  border-right:solid windowtext 1.0pt;padding:3.75pt 3.75pt 3.75pt 3.75pt">

TCP

</td>

</tr>

<tr style="page-break-inside:avoid">

<td style="border:solid windowtext 1.0pt;border-top:none;padding:3.75pt 3.75pt 3.75pt 3.75pt">

File Server

</td>

<td style="border-top:none;border-left:none;border-bottom:solid windowtext 1.0pt;
  border-right:solid windowtext 1.0pt;padding:3.75pt 3.75pt 3.75pt 3.75pt">

9301, 8899

</td>

<td style="border-top:none;border-left:none;border-bottom:solid windowtext 1.0pt;
  border-right:solid windowtext 1.0pt;padding:3.75pt 3.75pt 3.75pt 3.75pt">

TCP

</td>

</tr>

<tr style="page-break-inside:avoid">

<td style="border:solid windowtext 1.0pt;border-top:none;padding:3.75pt 3.75pt 3.75pt 3.75pt">

Media Server

</td>

<td style="border-top:none;border-left:none;border-bottom:solid windowtext 1.0pt;
  border-right:solid windowtext 1.0pt;padding:3.75pt 3.75pt 3.75pt 3.75pt">

8915

</td>

<td style="border-top:none;border-left:none;border-bottom:solid windowtext 1.0pt;
  border-right:solid windowtext 1.0pt;padding:3.75pt 3.75pt 3.75pt 3.75pt">

TCP

</td>

</tr>

<tr style="page-break-inside:avoid">

<td style="border:solid windowtext 1.0pt;border-top:none;padding:3.75pt 3.75pt 3.75pt 3.75pt">

Media server media/signalling

</td>

<td style="border-top:none;border-left:none;border-bottom:solid windowtext 1.0pt;
  border-right:solid windowtext 1.0pt;padding:3.75pt 3.75pt 3.75pt 3.75pt">

5060, 20000-26000

</td>

<td style="border-top:none;border-left:none;border-bottom:solid windowtext 1.0pt;
  border-right:solid windowtext 1.0pt;padding:3.75pt 3.75pt 3.75pt 3.75pt">

UDP

</td>

</tr>

<tr style="page-break-inside:avoid">

<td rowspan="2" style="border:solid windowtext 1.0pt;border-top:none;
  padding:3.75pt 3.75pt 3.75pt 3.75pt">

SQL Server (TCP)  
SQL Server (UDP)

</td>

<td style="border-top:none;border-left:none;border-bottom:solid windowtext 1.0pt;
  border-right:solid windowtext 1.0pt;padding:3.75pt 3.75pt 3.75pt 3.75pt">

1434, 1433

</td>

<td style="border-top:none;border-left:none;border-bottom:solid windowtext 1.0pt;
  border-right:solid windowtext 1.0pt;padding:3.75pt 3.75pt 3.75pt 3.75pt">

TCP

</td>

</tr>

<tr style="page-break-inside:avoid">

<td style="border-top:none;border-left:none;border-bottom:solid windowtext 1.0pt;
  border-right:solid windowtext 1.0pt;padding:3.75pt 3.75pt 3.75pt 3.75pt">

1434

</td>

<td style="border-top:none;border-left:none;border-bottom:solid windowtext 1.0pt;
  border-right:solid windowtext 1.0pt;padding:3.75pt 3.75pt 3.75pt 3.75pt">

UDP

</td>

</tr>

<tr style="page-break-inside:avoid">

<td style="border:solid windowtext 1.0pt;border-top:none;padding:3.75pt 3.75pt 3.75pt 3.75pt">

VC

</td>

<td style="border-top:none;border-left:none;border-bottom:solid windowtext 1.0pt;
  border-right:solid windowtext 1.0pt;padding:3.75pt 3.75pt 3.75pt 3.75pt">

9300, 9310, 8898

</td>

<td style="border-top:none;border-left:none;border-bottom:solid windowtext 1.0pt;
  border-right:solid windowtext 1.0pt;padding:3.75pt 3.75pt 3.75pt 3.75pt">

TCP

</td>

</tr>

<tr style="page-break-inside:avoid">

<td style="border:solid windowtext 1.0pt;border-top:none;padding:3.75pt 3.75pt 3.75pt 3.75pt">

IIS Health Checks

</td>

<td style="border-top:none;border-left:none;border-bottom:solid windowtext 1.0pt;
  border-right:solid windowtext 1.0pt;padding:3.75pt 3.75pt 3.75pt 3.75pt">

80

</td>

<td style="border-top:none;border-left:none;border-bottom:solid windowtext 1.0pt;
  border-right:solid windowtext 1.0pt;padding:3.75pt 3.75pt 3.75pt 3.75pt">

TCP

</td>

</tr>

</tbody>

</table>

</div>

<span style="color:white"> </span>

<span style="color:red">All of these Services _<u>must</u>_ be in separate entries of the Windows Firewall Rules.</span>

To do this, follow these simple steps. First, click on <span style="color:#3366FF">Inbound Rules</span> and on the right context pane click <span style="color:#3366FF">New Rule…</span> Click <span style="color:#3366FF">Port</span>, as shown below, and then click <span style="color:#3366FF">Next</span>.

![](1+-+Configuring+Windows.doc_files/image020.png)

Now, click <span style="color:#3366FF">TCP</span> and specify the port.

![](1+-+Configuring+Windows.doc_files/image021.png)

Click <span style="color:#3366FF">Next</span> and specify the radial button <span style="color:#3366FF">Allow the connection</span>, and make sure it is allowed on the <span style="color:#3366FF">Domain</span>, <span style="color:#3366FF">Private</span>, and <span style="color:#3366FF">Public</span> levels. Lastly, make sure to name it properly, as listed in the table above.

![](1+-+Configuring+Windows.doc_files/image022.png)

Add the Drone Service to Windows Firewall

<span style="font-size:10.0pt;font-family:Symbol">·<span style="font:7.0pt &quot;Times New Roman&quot;"></span> </span> 

*   Open Windows Firewall (<span style="color:#3366FF">Control Panel→Windows Firewall</span>).
*   Click <span style="color:#3366FF">Advanced Settings</span><span style="color:black">.</span>
*   Select <span style="color:#3366FF">Outbound Rules</span><span style="color:black">.</span>
*   <span style="color:#3366FF">Add a New Rule</span><span style="color:black">.</span>

*   Rule Type: <span style="color:#3366FF">Program</span>. Click <span style="color:#3366FF">Next</span>.
*   Program Path: [Location of the <span style="color:#3366FF">DroneService.exe</span>]. Click <span style="color:#3366FF">Next</span>.
*   Select “<span style="color:#3366FF">Allow the connection</span>”. Click <span style="color:#3366FF">Next</span>.
*   Check <span style="color:#3366FF">Domain</span>, <span style="color:#3366FF">Private</span> and Public check boxes.  Click <span style="color:#3366FF">Next</span>.
*   Name: <span style="color:#3366FF">Drone Service</span>. Click <span style="color:#3366FF">Finish</span><span style="color:black">.</span>

*   <span style="color:black">Repeat for</span> <span style="color:#3366FF">Inbound Rules</span><span style="color:black">.</span>

</div>