# 🔐 VLAN + Port Security Lab

## 🧠 Overview
This lab simulates a realistic network security scenario using Cisco Packet Tracer. You will:
- Configure VLANs to segment departments
- Apply port security to lock ports to specific MAC addresses
- Trigger and detect violations when unauthorized devices connect

Aimed at reinforcing network segmentation and switch security fundamentals.

---

## 🎯 Project Objectives
- Create VLANs for HR and IT departments
- Assign VLANs to specific switch ports
- Enable and configure port security with sticky MAC
- Simulate and document a port security violation
- Capture key CLI configurations and testing steps

---

## 🖥️ Lab Topology
| Device | Role         | VLAN     | Switch Port |
|--------|--------------|----------|-------------|
| PC0    | HR Dept      | VLAN 10  | Fa0/1       |
| PC1    | IT Dept      | VLAN 20  | Fa0/2       |
| PC2    | Unauthorized | N/A      | Fa0/3 → Fa0/1 (attack) |
| Switch | Core Switch  | -        | Fa0/1–Fa0/3 |

📸 **Screenshot:** `/screenshots/topology.png`

---

## ⚙️ Step-by-Step Configuration

### 🔧 Step 1: VLAN Creation
enable
configure terminal
vlan 10
 name HR
exit
vlan 20
 name IT
exit

📸 /screenshots/vlan-config.png

### 🔧 Step 2: Assign VLANs to Ports
interface fa0/1
 switchport mode access
 switchport access vlan 10
exit
interface fa0/2
 switchport mode access
 switchport access vlan 20
exit

📸 /screenshots/vlan-port-assignment.png

---

### 🛡️ Step 3: Configure Port Security

interface fa0/1
 switchport port-security
 switchport port-security maximum 1
 switchport port-security violation restrict
 switchport port-security mac-address sticky
 switchport port-security mac-address [PC0-MAC]
exit

interface fa0/2
 switchport port-security
 switchport port-security maximum 1
 switchport port-security violation restrict
 switchport port-security mac-address sticky
exit

📸 /screenshots/port-security/config.png

---

### 🔍 Step 4: Testing VLAN Segmentation
- Assign static IPs:
  - PC0: `192.168.10.10`
  - PC1: `192.168.20.10`
- Attempt ping between VLANs (will fail)

📸 `/screenshots/vlan-segmentation/pc0-ip.png`
📸 `/screenshots/vlan-segmentation/pc1-ip.png`
📸 `/screenshots/vlan-segmentation/ping-fail.png`

---

### 🔥 Step 5: Simulate Unauthorized Access
1. Disconnect PC0 from Fa0/1
2. Plug PC2 into Fa0/1
3. Assign PC2 IP: `192.168.10.200`
4. Generate traffic (ping, open browser)
5. Check for violation:
```bash
show port-security interface fa0/1
```
Expected: Violation Count > 0

📸 `/screenshots/port-security/pc2-violation.png`
📸 `/screenshots/port-security/show-result.png`

---

## ✅ Outcome
- Verified VLAN segmentation between HR and IT
- Successfully secured switch ports to specific devices
- Detected and documented unauthorized device attempts

---

## 💡 Key Takeaways
- VLANs isolate network segments for security and organization
- Port security helps prevent unauthorized access
- `Sticky MAC` is powerful for dynamic environments
- `Violation mode restrict` silently blocks attackers while logging the attempt

---

## 📂 File Structure
```
├── screenshots/
│   ├── topology.png
│   ├── vlan-config.png
│   ├── vlan-port-assignment.png
│   ├── vlan-segmentation/
│   │   ├── pc0-ip.png
│   │   ├── pc1-ip.png
│   │   └── ping-fail.png
│   ├── port-security/
│       ├── config.png
│       ├── pc2-violation.png
│       └── show-result.png
└── README.md
```

---

## 🧠 Author Notes
This lab was completed and documented manually by the creator to reinforce practical understanding of network segmentation and switch-level endpoint defense. Built using Cisco Packet Tracer v8.x.

