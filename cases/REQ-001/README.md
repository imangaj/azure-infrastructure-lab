# Ticket ID - Ticket Title

## 1. Business Requirement

What does the business need?
The application will initially consist of:

- One web server
- One database server

Both servers will run as Azure virtual machines.

Employees should be able to access the web application.

The database server must not be directly accessible from the public Internet.

The Application Team expects the environment to grow in the future,
so the network design should allow additional application servers to
be added later.
1. The database must not be directly exposed to the Internet.

2. Only traffic required for the application should be allowed
   between the web tier and the database tier.

3. Administrative access should be limited.

4. The network design must avoid unnecessary public exposure.
---

## 2. Environment

Azure region:
West Europe

Environment:
Production design exercise

Existing on-premises network:
10.10.0.0/16

Expected Azure workload:
Web tier
Database tier

Current number of Azure VMs:
2

Potential future growth:
Additional web/application servers

---

## 3. Task

What exactly must be completed?
Design the initial Azure network architecture for this application.

Do NOT deploy anything yet.

First analyze the requirement and prepare a proposed design.

---

## 4. Initial Analysis

What do I currently think?

Possible solutions or causes:

### FACT
   we need to vm on for aplication and one for database
### FACT 
   db most not dirctly accessible from public internt 
### FACT
   in future we may need an extra vm for application
### PROPOSAL
   we need somthing like swich and router in azure to ba able to connect servers and also connct clinet to application
### PROPOSAL 
we need to seprate the db and applicaton server 
### PROPOSAL
   - One VNet
   - Separate Web and Database subnets
   - Private IP for DB01
   - Non-overlapping address space
   - NSG rules allowing only required Web-to-Database traffic
   - No direct public exposure for the database
### PROPOSAL

   - Do not assign a public IP directly to WEB01 at this stage.
   - First confirm whether employees need corporate-only access or Internet access.
   - If public HTTPS access is required, consider Azure Application Gateway as the controlled web entry point.
   - Keep DB01 private and accessible only from the Web tier on the required database port.
   

## 5. Missing Information

What information do I need before proceeding?

---

## 6. Knowledge Blockers

What do I need to understand before I can continue?
## 7. Investigation / Research

## 8. Proposed Solution
   Employees access the web application over the Internet using HTTPS.
   Traffic first reaches Azure Application Gateway, which forwards approved web traffic to WEB01.
   WEB01 communicates with DB01 over TCP port 1433 when database access is required.
        VNet: 10.20.0.0/16

         10.20.0.0/24  -> Application Gateway subnet
         10.20.1.0/24  -> Web subnet
         10.20.2.0/24  -> Database subnet
         10.20.3.0/26  -> AzureBastionSubnet
         
      ### Web Access
         Employees access the application from the Internet using HTTPS.
         Internet traffic reaches Azure Application Gateway first.
         Application Gateway forwards approved web traffic to the web tier.
         WEB01 does not require a directly assigned public IP.
      ### Database Access
         DB01 uses a private IP and has no public IP.
         TCP 1433 is allowed from the Web subnet to the Database subnet.
         Unnecessary Web-to-Database traffic is denied.
      ### Security
          public internt access with application gatway(HTTPS alowed)
          Unnecessary application traffic from the Web subnet to the Database subnet is denied.
          Azure Bastion provides secure administrative access to Azure VMs over RDP or SSH without requiring public IP addresses on the VMs.
          Administrators connect to Azure Bastion, and Bastion then connects privately to the target VM inside the VNet.
      ### Future Scalability
          Additional web servers can later be added to the Web subnet
          and registered as Application Gateway backend targets without
          redesigning the database subnet.
---
---
