
<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployment in the Cloud (Azure)</h1>
This guide outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Resources and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Computer)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022 Azure Edition x64 Gen2
- Windows 10 (21H2)(4vCPu)

<h2>High-Level Deployment and Configuration Steps</h2>

<ul><!--Start of main list-->
    <li>Create Resources in Azure
       <img src="https://kb.netop.com/assets/azure_main_screen.png"/>
        <ul><!--Start of nested list-->
         <li>Create a Domain Controller Vm (Windows Server 2022 Azure Edition) "DC-1" 
           <ul><!--Start of nested list-->
            <li>! Take note of the Resource Group and Virtual Network (Vnet) that get created at this time !</li>
           </ul><!--End of nested list-->
             <li>Set "DC-1" NIC (Network Interface Card) Privcate IP Address to static
              <li>Create a User Vm (WInodws 10) named "Client-1". Use the same Resource Group and Vnet that was created during "DC-1" launch.
               <li>Once both Vm's are launched we need to make sure they are on the same Vnet (Use Network Watcher on right handside tab).</li>
       </ul><!--End of nested list-->
    </li>
</ul><!--End of main list-->
      
<ul><!--Start of main list-->
  <li>Verify connectivity of between "Client-1" and "DC-1"
    <ul><!--Start of nested list-->
     <li>Login to "Client-1" with Remote Desktop and ping "DC-1" private IP address with ping -t (ip address) (perpetual ping)
       <li>Login to the "DC=1" and enable ICMPv4 in on the local windows Firewall
         <img src="https://www.rootusers.com/wp-content/uploads/2016/03/windows-firewall-enable-icmpv4-in.png"/>
         <li>If launch was successful than "Client-1" should be getting pings.</li>
    </ul><!--End of nested list-->
  </li>
</ul><!--End of main list-->
         

<h2>Deployment and Configuration Steps</h2>

<ul><!--Start of main list-->
  <li>Install Active Directory
    <img src="https://bigfix-wiki.hcltechsw.com/blogs/bradsexton/resource/BLOGS_UPLOADED_IMAGES/1ad.png"/>
     <ul><!--Start of nested list-->
      <li>Login to DC-1 and install Active Directory Domain Services
        <li>Promote server as a Domain Controller: Setup a new forest as "mydomain.com" (you can name it whatever but just remember what it is)
          <li>Restart and then log back into "DC-1" as user: mydomain.com\labuser</li>
    </ul><!--End of nested list-->
  </li>
</ul><!--End of main list-->

<ul><!--Start of main list-->
  <img src="https://activedirectorypro.com/wp-content/uploads/2016/10/101616_1329_HowtoCreate1.jpg"/> <figcaption>"Example of Creating Users"</figcaption>
  <li>Create an Admin and User Account in AD
    <ul><!--Start of nested list-->
      <li>In Active Directory Users and Computers menu (ADUC), create an Organizational Unit (OU) called “+EMPLOYEES” (There is an employees tab already made but this is just for us to know how to make a new one)
        <li>Create a new OU named “+ADMINS”
          <li>Create a new employee named “Jane Doe” (same password) with the username of “jane_admin”
            <li>Add jane_admin to the “Domain Admins” Security Group
              <li>Log out/close the Remote Desktop connection to "DC-1" and log back in as “mydomain.com\jane_admin”
                <li>User jane_admin should have all admin privileges and you may use this as main admin acoount.</li>
    </ul><!--End of nested list-->
  </li>
</ul><!--End of main list-->

<ul><!--Star of main list-->
  <li>Join "Client-1" to your domain (mydomain.com)
    <ul><!--Star of nested list-->
      <li>From the Azure Portal, set "Client-1" DNS settings to the DC’s Private IP address
        <li>From the VM tab, restart/refresh "Client-1"
          <li>Login back in "Client-1" (Remote Desktop) as the original local admin (labuser) and join it to the Domain (computer will restart)
            <li>Login to the Domain Controller (Remote Desktop) and verify "Client-1" shows up in (ADUC) inside the “Computers” tab container on the root of the Domain.
              <li>Create a new OU named “+CLIENTS” and drag "Client-1" into there (This step isnt necessary but just helps keep things in order.)</li>
    </ul><!--End of nested list-->
  </li>
</ul><!--End of main list-->

<ul><!--Start of main list-->
  <li>Setup Remote Desktop for non-administrative users on "Client-1"
    <img src="https://i.pcmag.com/imagery/articles/07J3Zzrlg7xVwiZgRJLdfju-164..v1650900080.png"/>
    <ul><!--Start of nested list-->
      <li>Log into "Client-1" as mydomain.com\jane_admin and open system properties
        <li>Click “Remote Desktop”
          <li>Allow “domain users” access to remote desktop
            <li>You can now log into "Client-1" as a normal user now.</li>
    </ul><!--End of nested list-->
  </li>
</ul><!--End of main list-->
<h2>Congratulations! You have now launched and set up accounts in Active Directoy within Azure Cloud</h2>
<br />
