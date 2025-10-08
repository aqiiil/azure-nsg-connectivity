# Azure NSG & VM Connectivity

## Objective
Configure and analyze how Azure **Network Security Groups (NSGs)** control inbound and outbound access to a Windows VM with a public IP. Show how allowing/restricting specific ports and protocols (RDP, ICMP) changes real connectivity.

---

## Environment
- **Cloud:** Microsoft Azure (East US 2)
- **Host VM:** Windows 10 (Public IP assigned)
- **Client:** macOS Terminal for testing
- **Tools:** Azure Portal, RDP, Windows Defender Firewall

---

## Implementation Steps

### 1) Create Resource Group and VM
- Deploy a Windows VM with a public IP.
- Confirm associated resources (NIC, disk, IP) exist in the resource group.
- _Screenshot:_ `./screenshots/resource_groups.png`

### 2) Connect via RDP
- RDP to the VM to verify access.
- _Screenshot:_ `./screenshots/vm_connected_via_rdp.png`

### 3) Windows Firewall (for isolation)
- Temporarily turn off Windows Defender Firewall to observe only NSG behavior.
- _Screenshot:_ `./screenshots/turned_off_windows_defender_firewall.png`

### 4) NSG: Allow Only RDP
- Configure inbound NSG rule to allow **TCP 3389** (RDP). Keep defaults to deny others.
- _Screenshot:_ `./screenshots/nsg_allow_only_rdp_3389.png`

### 5) Test ICMP (Before Rule)
- From macOS, ping the VM's public IP. Expect timeouts.
```bash
ping <your_vm_public_ip>
```
- _Screenshot:_ `./screenshots/icmp_timeouts_before_rule.png`

### 6) Add ICMPv4 Inbound Rule
- Create an inbound rule allowing **ICMPv4** to the VM.
- _Screenshots:_
  - `./screenshots/allow_icmp_inbound_rule_created.png`
  - `./screenshots/icmp_packets_transmitting_after_rule.png`

### 7) Honeypot Simulation
- Add a permissive inbound rule (e.g., "AllowingEveryInbound") to expose the VM for monitoring.
- _Screenshots:_
  - `./screenshots/allowing_every_traffic_honeypot.png`
  - `./screenshots/updated_nsg_rules_honeypot.png`

---

## Results

| Scenario                | Expected Outcome                                 |
|-------------------------|---------------------------------------------------|
| Only RDP allowed        | Only port 3389 reachable; ICMP blocked           |
| ICMP rule added         | Ping responses succeed                            |
| All inbound allowed     | All traffic permitted; increased attack surface   |

---

## Key Learnings
- NSGs are virtual firewalls at NIC/subnet scopes and are **separate** from the OS firewall.
- Rule priority matters (lower number = higher priority).
- Opening ICMP confirms rule effectiveness quickly; opening "all inbound" is risky and should be used only for controlled experiments.


## Screenshots

![Allowing Every Traffic Honeypot](./screenshots/Allowing_Every_Traffic_Honeypot.png)
![ICMP After Firewall Update](./screenshots/ICMPAfterFirewallUpdate.png)
![ICMP Rule Created](./screenshots/ICMPRuleCreated.png)
![Allow ICMP Inbound Rule Created](./screenshots/Allow_ICMP_inbound.png)
![Testing ICMP Before Firewall Update](./screenshots/TestingICMPBeforeFirewallUpdate.png)
![NSG Allowing RDP](./screenshots/NSG_Allowing_RDP.png)
![Turned Off Windows Defender](./screenshots/Turned_Off_Windows_Defender.png)
![WF MSC](./screenshots/wf.msc.png)
![Resource Groups](./screenshots/Resource_Groups.png)
![Info](./screenshots/info.png)
![VM Connected via RDP](./screenshots/VM_connected_via_RDP.png)



### Skills Demonstrated
- Configured and validated Azure Network Security Groups (NSGs) to control inbound and outbound traffic.
- Applied a structured testing approach: establish baseline, implement configuration changes, and verify results.
- Demonstrated understanding of network security principles and secure cloud configuration practices.

