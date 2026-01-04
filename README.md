
---

## Networking Components Implemented

### 1. Virtual Network
A dedicated virtual network was created to host all networking components.

- **Name**: vnet-az104-core  
- **Address space**: 10.0.0.0/16  

**Evidence**  
`screenshots/vnet-overview.png`

---

### 2. Subnet Segmentation
The VNet was segmented into three subnets to support separation of duties and security boundaries.

| Subnet Name | Address Range | Purpose |
|------------|---------------|--------|
| sub-management | 10.0.1.0/24 | Administrative access |
| sub-workloads | 10.0.0.0/24 | Application workloads |
| sub-endpoints | 10.0.2.0/24 | Azure service endpoints |

**Evidence**  
`screenshots/vnet-subnets.png`

---

### 3. Network Security Groups (NSGs)

A Network Security Group was created and associated at **subnet level** to enforce consistent security rules.

- **NSG name**: nsg-management
- **Association**: Subnet-level (not NIC-level)

#### Inbound Rules
- **AllowSSH-Inbound**
  - Source: 10.0.1.0/24 (management subnet)
  - Destination: 10.0.0.0/24 (workloads subnet)
  - Port: 22
  - Protocol: TCP
  - Priority: 110
  - Action: Allow

- **DenyAllOtherInbound**
  - Source: Any
  - Destination: Any
  - Priority: 120
  - Action: Deny

#### Outbound Rules
- Allow HTTP (80)
- Allow HTTPS (443)
- Default Azure outbound rules retained

This configuration enforces **least-privilege administrative access** while maintaining secure defaults.

**Evidence**  
`screenshots/nsg-rules.png`

---

### 4. Service Endpoints

To enable secure access to Azure PaaS services without exposing traffic to the public internet, a service endpoint was configured.

- **Subnet**: sub-endpoints  
- **Service enabled**: Microsoft.Storage  

This allows Azure Storage traffic to remain on the Azure backbone network.

**Evidence**  
`screenshots/service-endpoints.png`

---

### 5. Network Monitoring (Network Watcher)

Azure Network Watcher was enabled automatically in the region and used to validate network topology.

- Verified VNet and subnet visibility
- Confirmed NSG associations
- Reviewed topology diagram for connectivity understanding

This ensures the network can be monitored and troubleshot effectively.

**Evidence**  
`screenshots/network-watcher-topology.png`

---

## Governance Alignment

### Resource Tags
Standard governance tags were applied at the **resource group level**:

| Tag Name | Value |
|--------|------|
| Environment | Lab |
| Project | AZ104-Networking |
| CostCentre | Training |
| Owner | Tatenda Zhou |

### Resource Locks
A **Delete lock** was applied at the resource group scope to prevent accidental deletion of critical networking resources.

- **Lock name**: Do not delete  
- **Lock type**: Delete  

**Evidence**  
`screenshots/resource-group-lock.png`

---

## Design Decisions

The following design decisions were made to align with enterprise Azure networking practices:

- Subnets were segmented to separate administrative access, workloads, and platform services.
- NSGs were applied at **subnet level** to enforce consistent security across all resources.
- Management access was explicitly restricted to a trusted subnet.
- A deny-by-default inbound strategy was implemented using explicit deny rules.
- Service endpoints were chosen instead of public access to secure Azure PaaS connectivity.
- Network Watcher was enabled early to support monitoring and troubleshooting.
- Governance controls (tags and locks) were applied consistently with Project 1.

---

## Outcome

This project demonstrates the ability to:

- Design and implement secure Azure virtual networks
- Apply subnet-level network security controls
- Enforce least-privilege access using NSGs
- Secure PaaS connectivity with service endpoints
- Monitor network topology and security
- Maintain governance consistency across Azure resources

---

## Author

**Tatenda Zhou**  
Azure Administrator (AZ-104 Certified)
