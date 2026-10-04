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
   ## 8. Proposed Solution

      ### Network
         one vnet: 10.20.0.0/16
           subents:
            1. web subnet 10.20.1.0/24
            2. database subnet 10.20.2.0/24
         
      ### Web Access
        public internet access
      ### Database Access
         just form applicaton subnet with port tcp 1433
      ### Security
          public internt access with application gatway(HTTPS alowed)
          all traffic except port mentioned is denied for db subnet
      ### Future Scalability
      for aplicaton it grow from 1 to several
      for db just one is required
---

## 5. Missing Information

What information do I need before proceeding?
- How many additional servers are expected in the future?

- Is web application firewall protection required?
---

## 6. Knowledge Blockers

What do I need to understand before I can continue?


---
