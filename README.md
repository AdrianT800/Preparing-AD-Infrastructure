![1035255](https://github.com/user-attachments/assets/7cdae74a-7836-4365-af4e-737124531edf)

# Preparing Active Directory Infrastructure
This guide provides an overview of the process for setting up a resource group, a virtual network, and two virtual machines (VMs). One VM will be configured to run Windows Server, while the other will operate on Windows 10. The Windows 10 VM will serve as a client that joins the domain, allowing us to log in using domain user accounts. To achieve this, the client (Windows 10 VM) must be configured to use the Domain Controller (Windows Server VM) as its DNS server. This requires setting the DNS IP address on the client’s network interface card to match the IP address of the Domain Controller.


<br />


<h2>Video Demonstration</h2>

- ### [Google Drive: Preparing Active Directory Infrastructure](https://drive.google.com/file/d/1C12szlKd_w5XQ_bD_jE1e8UosCuA5yRE/view?usp=drive_link)


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Resources Group, Virtual Machines/Computer, Virtual Machines/Window Server, Virtual Network and Subnet)
- Remote Desktop
- Powershell
- Command Prompt
- Windows Defenders Firewall

<h2>Operating Systems Used </h2>

- Windows 10 (Pro, version 22H2 - x64 Gen2)
- Windows Server 2022 Datacenter: (Azure Edition - x64 Gen2)

<h2>Preparing Active Directory Infrastructure Objectives</h2>

- Step 1: Set Up the Domain Controller in Azure
- Step 2: Set Domain Controller’s (DC-1) NIC Private IP address to be static
- Step 3: Disable Windows Firewall (for testing connectivity) in DC-1
- Step 4: Setup Client-1 in Azure
- Step 5: Set Domain Controller’s NIC Private IP address to be static in client-1

<h2>Infrastructure Steps</h2>

Step 1: Set Up the Domain Controller in Azure
<p> 
<img src="https://i.imgur.com/KPOpTem.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Begin by logging into your Microsoft Azure account. If you don’t have one, create a new account. Once logged in, you’ll need to create a resource group, a virtual network with a subnet, and two virtual
machines—in that order. It’s strongly recommended that all resources be created in the same Azure region to ensure compafibility and performance. Start by creating a resource group named "labtest". After
naming it, click Review + Create, then Create. It’s crucial that both VMs, the virtual network, and the subnet are placed within this resource group.
</p>
<br />

<p>
<img src="https://i.imgur.com/XHeclck.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Next, create a virtual network and subnet named "Active-Directory-VNet". After putting the name, click Review + Create.
</p>
<br />

<p>
<img src="https://i.imgur.com/AKiLBwU.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Now, create your first virtual machine, which will serve as the domain controller. Name this VM "DC-1".
</p>
<br />

<p>
<img src="https://i.imgur.com/WdyHh3Y.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Under the Image option, select Windows Server 2022 Datacenter: Azure Edifion – x64 Gen2, and for the Size, choose Standard_D2s_v3 (2 vCPUs, 8 GiB memory). These specificafions are essenfial for proper domain controller funcfionality. Use the following credenfials: Username: "labdemo", Password: "Vmdemo12345$".
</p>

<p>
<img src="https://i.imgur.com/a9qwNyd.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
In the Networking section, ensure the VM is connected to the Acfive-Directory-VNet. Then click Review + Create, and finally Create.
</p>


Step 2: Set Domain Controller’s (DC-1) NIC Private IP address to be static
<p>
<img src="https://i.imgur.com/1u207Y6.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Once the DC-1 VM is deployed, configure its network interface to use a stafic private IP address. To do this, navigate to the VM in the Azure portal, click on Networking, then select the Network Interface. Under IP Configurafions, change the private IP allocafion from Dynamic to Stafic, and save the changes.
</p>
<br />


Step 3: Disable Windows Firewall (for testing connectivity) in DC-1
<p>
<img src="https://i.imgur.com/trOqniX.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Afterward, log into the DC-1 VM using the credenfials provided. For testing connectivity, disable the Windows Firewall. Open the Run dialog (right-click the Start menu and select Run), type: "wf.msc", and press Enter. In the Windows Defender Firewall seftings, turn off the firewall for the Domain, Private, and Public profiles, then click Apply.
</p>
<br />


Step 4: Setup Client-1 in Azure
<p>
<img src="https://i.imgur.com/oZ89ukA.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Return to the Azure portal and begin creafing your second VM that will serve as the client machine. This VM will need to be configured to use the domain controller’s private IP address as its DNS server. Name the VM :Client-1". 
</p>
<br />

<p>
<img src="https://i.imgur.com/Hd2QLkP.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
 Under the image selecfion, choose Windows 10 Pro, version 22H2 – x64 Gen2. For the size, select Standard_D2s_v3 (2 vCPUs, 8 GiB memory), which is essenfial for compafibility and performance. Use the following credenfials: Username: labdemo, Password: Vmdemo12345$.
</p>
<br />

<p>
<img src="https://i.imgur.com/xB3HwoM.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
 In the networking section, ensure that Active-Directory-VNet is selected as the virtual network. Once all seftings are configured, click Review + Create, then Create.
</p>
<br />


Step 5: Set Domain Controller’s NIC Private IP address to be static in client-1
<p>
<img src="https://i.imgur.com/Npyf2T4.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
After the VM is deployed, configure its DNS seftings to point to the domain controller (DC-1). First, go to the DC-1 VM and copy its private IP address from the Overview section. Then, navigate to Client-1, go to its Network Seftings, and click on the Network Interface. Under DNS Servers, select Custom, enter DC-1’s private IP address, and save the changes.
</p>
<br />

<p>
<img src="https://i.imgur.com/uh0cmqn.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Restart Client-1 from the Azure portal to apply the configurafion.
</p>
<br />


<p>
<img src="https://i.imgur.com/Pns8SEG.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
After the restart, log into Client-1 using the local admin credenfials. Open PowerShell from the search bar and test connecfivity by pinging DC-1’s private IP address (e.g., ping 10.0.0.4). To confirm that the DNS seftings are correctly applied, run the command ipconfig /all in PowerShell. The output should display DC-1’s private IP address listed under DNS Servers, confirming that the client is properly configured to communicate with the domain controller.
</p>
<br />
