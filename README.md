<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Active Directory Deployed in the Cloud (Azure): Setup</h1>
This tutorial outlines the setup of Active Directory within Azure Virtual Machines.<br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (22H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Create Resource Group, and Vnet
- Create Domain Controller VM
- Create Client VM
- Configuration Steps / Testing

<h2>Deployment and Configuration Steps</h2>

![image](https://github.com/user-attachments/assets/aa89f2bf-e166-4c60-b116-f98d482fa804)

<p>
To start our walkthrough, we will create a resource group that will hold our virtual network with a Domain Controller Server and a Client Server. You can name the resource group anything. I will name mine "Active_Directory_Walkthrough" for the sake of the tutorial. In my case, I am setting the region as Canada Central. Leave everything else as default values.
</p>
<br />

![image](https://github.com/user-attachments/assets/4035f787-0d2d-4eb5-bf60-328e266e709c)

<p>
When creating your Vnet, give it a name, and make sure it is in the same region as your recource group. Other than that, leave all other settings as default. Next, we can create our domain controller. Make sure the following properties are used for this machine:

**Resource group**: The one we created earlier

**Virtual Machine name**: Whatever you want

**Region**: Same one as before

**Image**: Windows Server 2022 Datacenter: Azure Edition - x64 Gen2

**Size**: anything with 2 vcpu's

**Username and Password**: Anything you want (just make sure you log it in a notepad)

**Networking > Virtual network**: Network we createed before

After you are ready, click review + create and then Create.
</p>
<br />

![image](https://github.com/user-attachments/assets/a30f4ac2-bd7a-4733-bd82-14bd6ddb43c7)

<p>
Now, we need to create the Client VM. Use the exact same options as in the Domain Controller aside from the image being Windows 10 Pro.
</p>
<br />

![image](https://github.com/user-attachments/assets/2b884234-117f-4d17-9269-c62c0b3769ab)

<p>
In this walkthrough, we are going to configure the domain controller vm to act as a DNS server for the client VM. To do this, we have to set up a few things now, and then some later on. Let's change the domain controller's IP to be static rather than dynamic so that the client server will always be able to reliably find the dns at it's specified static address. To do this, we can click on the domain controller vm from the Virtual Machines window. Then click Networking > Network settings > blue link corresponding to the VM's Network Interface Card (NIC). On the next page, click the blue ipconfig and change the Private IP address settings Allocation from Dynamic to Static and click Save.
</p>
<br />

![image](https://github.com/user-attachments/assets/130c389d-5e8a-4bf9-b177-22d9f2bb651a)

<p>
Next, we need to ensure that the client VM will see the domain controller as a DNS server. Take note of the domain controller's private IP and then go back to the Virtual Machines page and click on the client VM. Then go to Networking > Network settings > blue link corresponding to the VM's Network Interface Card (NIC). Now, on the left side of the screen, click DNS servers. Now, just change the DNS servers from "Inherit from the virtual network" to "Custom" and put down the private IP of the domain controller we took note of earlier. CLick Save at the top.
</p>
<br />

<p>
Now, to apply the changes, restart the client vm:
</p>
<br />

![image](https://github.com/user-attachments/assets/5dcf37ee-3259-4bf9-9249-b3d0d975f704)

![image](https://github.com/user-attachments/assets/ba3dbb06-76b4-41b0-8b19-f8c11dd1d9b0)

<p>
We will need to disable the firewall in the domain controller for the purposes of this lab. To do so, remote into the domain controller. Then, Use the Run program and enter "wf.msc" to access the Windows Firewall. Next, right click "Windows Defender Firewall with Advanced Security on Local Computer" > Properties > Under all tabs, make sure the firewall state is set to Off > Apply > Ok.
</p>
<br />

![image](https://github.com/user-attachments/assets/6e71dc40-9bc5-4ae0-ac84-d2d3d5ad7b7a)

![image](https://github.com/user-attachments/assets/dd8721ab-0083-427a-b2a6-e6130c1b44a4)

<p>
To finish the first part of this walkthrough, we need to remote into the client VM and attempt to ping the domain controller to make sure the setup is working as expected. After opening the connection, open up the command prompt and ping the private IP address of the domain controller. Then, run the command ipconfig /all to check the DNS server for the client VM. The DNS server should be listed with the private IP of our domain controller:
</p>
<br />

![image](https://github.com/user-attachments/assets/8e868f12-bef7-4cd7-ba62-bbec75111bbd)


<p>
We are finished with the setup portion of this walkthrough. Good job!
</p>
<br />



