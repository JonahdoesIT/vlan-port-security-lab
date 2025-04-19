![cisco_packet_tracer](https://github.com/user-attachments/assets/b47e6c0f-9a45-4528-a898-a50349be11d8)

# 🔐 VLAN + Port Security Lab


## 🧠 Overview
This lab simulates a realistic network security scenario using Cisco Packet Tracer. 
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

 ![topology](https://github.com/user-attachments/assets/18f54419-ab7a-4e19-99c7-6c07c15d7626)


---

## ⚙️ Step-by-Step Configuration

### 🔧 Step 1: VLAN Creation + Assign VLANs to Ports
Start by creating two VLANs to segment traffic by department:
📘 VLAN 10 and 20 are now created and labeled for HR and IT. This allows us to separate network traffic within the same switch.

![Vlan creationg+port assignment](https://github.com/user-attachments/assets/1db8b41a-8d1a-4743-b281-aaf297787391)

### Assign VLAN 10 to port Fa0/1 (HR) and VLAN 20 to port Fa0/2 (IT):

#### 📘 This configuration ensures that each department’s device is isolated at Layer 2 unless routing is explicitly allowed later.

In these steps, we created two VLANs to represent different departments — HR and IT — and assigned them to specific switch ports. VLANs (Virtual Local Area Networks) allow us to segment traffic within a single switch, providing better organization, reduced broadcast traffic, and enhanced security.

---

### 🛡️ Step 2: Configure Port Security
The goal here is to limit which devices can connect to specific switch ports by using **port security** with **sticky MAC address learning**.

#### 🔧 Commands Used:
interface fa0/1
 switchport port-security
 
 switchport port-security maximum 1
 
 switchport port-security violation restrict
 
 switchport port-security mac-address sticky

exit

 ![fix 1](https://github.com/user-attachments/assets/bc9239b3-cfc3-4749-a5a7-bc5b4f046f76)


In this step, we secured the switch ports by enabling port security and configuring them to allow only one device per port. We used the "sticky MAC" feature so the switch could automatically learn the MAC address of the connected device and lock it in. We set the violation mode to "restrict", which blocks unauthorized devices while keeping the port active and logging the violation.

---

### 🔍 Step 3: Testing VLAN Segmentation
- Assign static IPs:
  - PC0: `192.168.10.10`
  - PC1: `192.168.20.10`
- Attempt ping between VLANs (will fail)

 ![PC0-IP config](https://github.com/user-attachments/assets/47aadf9c-a411-455c-8749-a88acc2ea84e)

 ![PC1-IP config](https://github.com/user-attachments/assets/4acb9bbc-9c00-4f26-82db-62f3f82a0576)

 ![ping test fail](https://github.com/user-attachments/assets/541d2878-1242-424d-ba26-ae9698947f1d)

After assigning VLANs and IP addresses to each PC, we attempted to ping from PC0 (VLAN 10) to PC1 (VLAN 20). The ping failed, which is expected. Since there is no inter-VLAN routing configured, traffic between VLANs is blocked by default — confirming that segmentation is working as intended.

---

### 🔥 Step 4: Simulate Unauthorized Access
1. Disconnect PC0 from Fa0/1
2. Plug PC2 into Fa0/1
3. Assign PC2 IP: `192.168.10.200`
4. Generate traffic (ping, open browser)
5. Check for violation:

show port-security interface fa0/1



 ![disconnect pc0](https://github.com/user-attachments/assets/7d51ab2d-53c2-4d4b-87db-9229e66f56aa)

 ![pc2 violation attempt](https://github.com/user-attachments/assets/abb85935-ca79-4ee3-9872-19737e1803ea)

 ![pc2 ping fail](https://github.com/user-attachments/assets/e51c969c-79b6-4fa2-a1f6-69fb299e5938)

 ![fix 2](https://github.com/user-attachments/assets/ba9cb48b-4e5d-4f64-ad1f-144ea00ef523)


To simulate an unauthorized device connecting to the network, we unplugged the authorized device from Fa0/1 and connected PC2 (the attacker) in its place. Since PC2's MAC address didn't match the one learned by the switch, the violation was triggered. The switch responded by blocking the traffic and incrementing the violation count, demonstrating how port security can prevent rogue devices from gaining access.

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
## 🧾 Conclusion

In this lab, we successfully configured VLANs to segment network traffic and implemented port security to restrict access based on MAC addresses. This setup enhances network security by ensuring that only authorized devices can communicate within designated VLANs.


