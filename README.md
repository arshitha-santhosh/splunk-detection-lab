# splunk-detection-lab
Splunk SIEM home lab (Kali + Windows 10, VirtualBox) for building and testing
MITRE ATT&CK-mapped detection rules against real attack simulations.

**Status:** 🚧 In progress

## Objective
Build hands-on SOC / detection engineering experience: ingest logs, simulate
attacks, write SPL detections, and document triage.
## 1.Lab Architecture
| Component | Role |
|---|---|
| Windows 10 VM | Victim endpoint, Universal Forwarder, Windows Event Logs |
| Kali Linux VM | Attacker machine (Hydra, Nmap) |
| Splunk Enterprise | SIEM: indexing, search, alerting |
| VirtualBox | Virtualisation, [Host-only / NAT Network] |
## 2. VM Creation
  -Created a Windows 10 VM in VirtualBox to act as the log source (victim endpoint).
  -Created a Kali Linux VM to act as the analyst/attacker machine.
  -Configured both VMs on the same VirtualBox network mode so they can reach each other(Host-only adapter)
## 3. Static IP Configuration
    -Set a static IP on the Windows 10 VM so the Splunk forwarder always points to a predictable address and doesn't break after a reboot or DHCP lease change.    (192.168.17.105)
## 4. Splunk Installation
  -Installed Splunk (fill in: on Kali, on Windows, or on the host machine).
  -Basic verification used to confirm it was running
  -     --> sudo /opt/splunk/bin/splunk status
   <img width="876" height="708" alt="image" src="https://github.com/user-attachments/assets/9dcd8a4a-53b4-4884-bdfa-a763ac86b31c" />
   
    - Splunk web UI runs at http://<splunk-host-ip>:8000.
    
## 5.Install the Universal Forwarder on Windows
    -Run the Universal Forwarder installer on the Windows 10 VM.
    -Point it at the Splunk indexer: <splunk-host-ip>:9997.
    -Set a deployment/receiving configuration so it knows where to send data.
   <img width="963" height="180" alt="image" src="https://github.com/user-attachments/assets/05496e14-4b53-4561-8eb6-587311112005" />

## 4. Configure inputs.conf
    -On the Windows VM, edit (or create) inputs.conf under the forwarder's etc/system/local/ directory:
  <img width="1007" height="424" alt="image" src="https://github.com/user-attachments/assets/ad3dc11c-bc22-45ad-af05-b2da443e0ad8" />
  Restart the forwarder service after editing.

## 5. Start everything in order
    -Start Splunk Enterprise.
    -Start/confirm the Universal Forwarder service on Windows is running.
    -Start Kali.
    Verifying Ingestion

## In Splunk's Search & Reporting app, run:
    -->index=* | stats count by host, sourcetype
You should see your Windows VM's hostname with sourcetypes like WinEventLog:Security.
<img width="864" height="404" alt="image" src="https://github.com/user-attachments/assets/785198ea-3735-4c85-a39e-bfb3e4000a27" />


