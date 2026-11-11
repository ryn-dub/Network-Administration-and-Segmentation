# Network-Administration-and-Segmentation
VLSM network design and IP allocation schema for a multi-department organizational lab.
# Enterprise Network Segmentation & VLSM Schema Design

## Project Overview
This project demonstrates the design and calculation of a custom IPv4 addressing scheme using **Variable Length Subnet Masking (VLSM)**. In an enterprise environment, standard subnetting often leads to massive IP address wastage. By implementing VLSM, this project successfully segments a single Class C private network range to efficiently meet the varying host requirements of distinct corporate departments while eliminating address exhaustion and minimizing the size of broadcast domains.

## Objective & Requirements
The goal was to partition an existing network allocation into three independent, non-overlapping subnets based on localized host demands. Best practice dictated organizing the subnets sequentially from largest host requirement to smallest.

*   **Base Network Allocation:** `192.168.10.0/24`
*   **Subnet A (Operations):** Requires 60 hosts
*   **Subnet B (Sales):** Requires 25 hosts
*   **Subnet C (IT / Management):** Requires 10 hosts

---

## Final Subnet Allocation Table

| Subnet | Department | Required Hosts | Subnet Mask | CIDR Prefix | Network ID | Usable Host Range | Broadcast ID |
| :--- | :--- | :---: | :--- | :---: | :--- | :--- | :--- |
| **A** | Operations | 60 | `255.255.255.192` | `/26` | `192.168.10.0` | `192.168.10.1` - `192.168.10.62` | `192.168.10.63` |
| **B** | Sales | 25 | `255.255.255.224` | `/27` | `192.168.10.64` | `192.168.10.65` - `192.168.10.94` | `192.168.10.95` |
| **C** | IT / Mgmt | 10 | `255.255.255.240` | `/28` | `192.168.10.96` | `192.168.10.97` - `192.168.10.110` | `192.168.10.111` |

---

## Engineering Mechanics & Methodology

### 1. Subnet A (Operations) Calculation
*   **Host Requirement:** 60 hosts.
*   **Formula Applied:** $2^h - 2 \ge 60$
*   **Result:** Leaving $h = 6$ host bits as zeros provides $2^6 = 64$ total addresses ($64 - 2 = 62$ usable). 
*   **Mask Modification:** 2 bits are borrowed into the 4th octet (`11000000` = `.192`), advancing the prefix to `/26`.
*   **Increment / Block Size:** 64. 

### 2. Subnet B (Sales) Calculation
*   **Host Requirement:** 25 hosts.
*   **Starting Boundary:** Picks up precisely where Subnet A terminated (`.64`).
*   **Formula Applied:** $2^h - 2 \ge 25$
*   **Result:** Leaving $h = 5$ host bits as zeros provides $2^5 = 32$ total addresses ($32 - 2 = 30$ usable).
*   **Mask Modification:** 3 bits are borrowed into the 4th octet (`11100000` = `.224`), advancing the prefix to `/27`.
*   **Increment / Block Size:** 32.

### 3. Subnet C (IT / Management) Calculation
*   **Host Requirement:** 10 hosts.
*   **Starting Boundary:** Picks up precisely where Subnet B terminated (`.96`).
*   **Formula Applied:** $2^h - 2 \ge 10$
*   **Result:** Leaving $h = 4$ host bits as zeros provides $2^1 = 16$ total addresses ($16 - 2 = 14$ usable).
*   **Mask Modification:** 4 bits are borrowed into the 4th octet (`11110000` = `.240`), advancing the prefix to `/28`.
*   **Increment / Block Size:** 16.

---
## 🗺️ Sequential IP Pool Timeline
This visual timeline tracks the sequential allocation of your network blocks across the base `192.168.10.0/24` range, mapping the precise boundaries and highlighting the remaining unallocated pool preserved for future enterprise expansion:

```text
[ Subnet A: Ops ] [ Subnet B ] [ Subnet C ] [----------- Unallocated Future Expansion Pool -----------]
.0            .63 .64      .95 .96     .111 .112                                                .255
```




## Key Competencies Demonstrated
*   **Binary Mathematics:** Mastery of base-2 conversions, determining bit values, and configuring octet boundaries.
*   **Network Optimization:** Calculating exact IP ranges to eliminate address waste using Variable Length Subnet Masking.
*   **Infrastructure Design:** Practical comprehension of core network boundaries, including Network IDs, Broadcast IDs, and valid gateway endpoints.
