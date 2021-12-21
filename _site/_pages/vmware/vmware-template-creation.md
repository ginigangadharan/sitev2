# Clone VM to Template - VMWare

## Create source VM
- Create a VM with RHEL minimal
- Update to latest
- Remove unwanted components if any
- Make sure necessory packages there (like vim, bind-utils, nmap etc)
- Make sure necessory services are enabled (ssh etc)
- Power-off VM 

## Create template from VM
- Goto "VM Customization Specifications" and Create profile with detail.
- Goto "Content Libraries" and create a folder to keep template
- Goto "VMs and Templates", Right click on Source VM and Clone -> Clone as Template to Library
  - Create a name for template
  - Choose the folder, location (Library) and cluster; create template.

## Test VM Template
- Either right click the template and create VM 
  or
  From Cluster -> Actions -> New Virtual Machine -> Choose "Deploy from template"

- Choose our custom template and give a name for VM
- Choose "Customize the Operating System"
- Choose the Custom profile we have create earlier
- Give a name for VM (we configured in profile like that or can automate too)


