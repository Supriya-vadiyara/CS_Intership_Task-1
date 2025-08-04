# Task 1: Local Network Port Scan

This report details the findings of a network scan performed on a local network as part of the Elevate Labs Cyber Security Internship.

---

## 1. Objective

The objective of this task was to use Nmap to scan the local network, identify active hosts, discover open ports, and analyze the potential security risks associated with the discovered services.

---

## 2. Tools and Command Used

*   **Tool:** Nmap version 7.97
*   **Command:** `nmap -sS 10.70.139.0/24`

---

## 3. Summary of Findings

The scan identified three active hosts on the network. The analysis revealed several open ports running services with significant security vulnerabilities, including an unencrypted FTP server, an open Remote Desktop Protocol (RDP) port, and exposed Windows SMB services.

---

## 4. Detailed Host Analysis

### Host 1: 10.70.139.110
*   **Open Ports:** None found.
*   **Filtered Ports:** `5060/tcp` (SIP)
*   **Analysis:** This host is likely a VoIP device protected by a firewall. The firewall is correctly filtering probes, which is good security practice. The risk is currently low.

### Host 2: 10.70.139.195
| Port | Service | State | Security Risk | Recommendation |
| :--- | :--- | :--- | :--- | :--- |
| 21/tcp | FTP | Open | **High** | The unencrypted FTP protocol exposes credentials. It should be disabled and replaced with SFTP. |
| 80/tcp | HTTP | Open | **Medium** | This web server uses unencrypted HTTP. It should be configured to use HTTPS to protect data in transit. |
| 111/tcp| rpcbind | Open | **Medium** | Exposes information about RPC services. Access should be restricted. |
| 3389/tcp| RDP | Open | **Critical** | A primary target for ransomware. Access should be placed behind a VPN or strictly firewalled, and use a strong password. |

### Host 3: 10.70.139.197 (My Host Machine)
*   **Open Ports:** `135`, `139`, `445`, `903`, `5357`
*   **Analysis:** This is the machine used to perform the scan. The open ports (`135`, `139`, `445`) are standard for Windows networking but represent a significant attack surface.
*   **Recommendation:** Ensure the operating system is always up-to-date with security patches to protect against vulnerabilities like EternalBlue. A host-based firewall should be active.

---

## 5. Conclusion

The scan successfully identified critical security weaknesses on the network. The most urgent priorities are to **secure the RDP service** and **disable the FTP service** on host `10.70.139.195` to prevent unauthorized access and data theft.
