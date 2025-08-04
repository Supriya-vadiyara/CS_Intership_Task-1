# Task 1: Local Network Security Report

This report details the findings of a network scan performed on a local network as part of the Elevate Labs Cyber Security Internship.

---

### 1. Objective

The objective of this task was to use `Nmap` to scan the local network, identify active hosts, discover open ports, and analyze the potential security risks associated with the discovered services.

---

### 2. Setup and Execution on Windows

#### **Tool Installation**

*   **Nmap (Network Mapper)**
    1.  Navigate to the official Nmap download page: `https://nmap.org/download.html`
    2.  Download the latest stable release executable installer (e.g., `nmap-7.9x-setup.exe`).
    3.  Run the installer and follow the on-screen prompts to complete the setup.

*   **Wireshark (Optional Packet Analyzer)**
    1.  Navigate to the official Wireshark download page: `https://www.wireshark.org/download.html`
    2.  Download and run the installer for Windows.

#### **Commands Used in Command Prompt**

1.  **To find the network range:**
    ```bash
    ipconfig
    ```

2.  **To perform the scan and save the results:**
    ```bash
    nmap -sS 10.70.139.0/24 -oN scan_results.txt
    ```

---

### 3. Summary of Findings

The scan identified **three active hosts** on the network. The analysis revealed several open ports running services with significant security vulnerabilities, including an unencrypted `FTP` server, an open `Remote Desktop Protocol (RDP)` port, and exposed `Windows SMB` services.

---

### 4. Detailed Host Analysis

#### **Host 1: `10.70.139.110`**
*   **Open Ports:** None found.
*   **Filtered Ports:** `5060/tcp` (SIP)
*   **Analysis:** This host is likely a VoIP device protected by a firewall. The firewall is correctly filtering probes, which is good security practice. The risk is currently low.

#### **Host 2: `10.70.139.195`**
| Port | Service | State | Security Risk | Recommendation |
| :--- | :--- | :--- | :--- | :--- |
| 21/tcp | FTP | Open | **High** | The unencrypted FTP protocol exposes credentials. It should be disabled and replaced with SFTP. |
| 80/tcp | HTTP | Open | **Medium** | This web server uses unencrypted HTTP. It should be configured to use HTTPS to protect data in transit. |
| 111/tcp| rpcbind | Open | **Medium** | Exposes information about RPC services. Access should be restricted. |
| 3389/tcp| RDP | Open | **Critical** | A primary target for ransomware. Access should be placed behind a VPN or strictly firewalled, and use a strong password. |

#### **Host 3: `10.70.139.197` (My Host Machine)**
| Port | Service | State | Security Risk | Recommendation |
| :--- | :--- | :--- | :--- | :--- |
| 135/tcp | msrpc | Open | **Medium** | Used for remote operations. Should be firewalled from untrusted networks. |
| 139/tcp | netbios-ssn | Open | **Medium** | Legacy service for file/printer sharing. Often redundant if port 445 is used. |
| 445/tcp | microsoft-ds | Open | **High** | **SMB protocol.** A major historical target for exploits like WannaCry. Patching is critical. |
| 903/tcp | iss-console-mgr | Open | **Low** | VMWare management service. Lower risk but contributes to the attack surface. |
| 5357/tcp | wsdapi | Open | **Low** | Used for network device discovery. Lower risk. |

---

### 5. Conclusion

This reconnaissance task successfully identified critical security weaknesses that create a tangible risk of unauthorized access, data theft, and operational disruption from ransomware.

The most urgent priorities are to **remediate the exposed RDP and FTP services on host `10.70.139.195`**. Failure to do so leaves a clear and direct path for an attacker on the local network. Furthermore, the analysis of the host machine itself (`10.70.139.197`) serves as a powerful reminder that **all devices** on a network contribute to its overall security posture. Maintaining a host-based firewall and ensuring timely application of security patches are fundamental and non-negotiable security practices.
