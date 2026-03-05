Report ID: SOC-20260305-01

Analyst: Ahmed Yasser (Junior SOC Analyst)

Date: March 5, 2026

Status: Closed – Mitigation Successful
1. Executive Summary

On March 5, 2026, a network traffic analysis was conducted on two PCAP files identifying a high-volume Brute-Force Attack targeting the internal FTP server. The attack originated from a single internal host. Analysis of the protocol response codes confirms that the attack was unsuccessful, as no administrative or user access was granted during the captured window.
2. Incident Details

    Target IP: 192.168.0.20 (FTP Server)

    Attacker IP: 192.168.0.21 (Suspected Compromised Host/Attacker Tool)

    Protocol: FTP (Port 21)

    Attack Method: Credential Stuffing / Dictionary Brute-Force

    Tools Used for Analysis: Wireshark, Tshark

3. Technical Analysis & Evidence

The traffic shows a rapid succession of USER and PASS commands from the attacker (.21) to the server (.20).
Key Findings:

    Failure Pattern: The server consistently returned FTP Response Code 530 (Login Incorrect) for all attempted credential pairs.

    Volume: High frequency of connection requests suggests an automated tool (e.g., Hydra or Medusa).

    Success Verification: A global filter for FTP Response Code 230 (User Logged In) returned zero results across both PCAP files, confirming that the attacker failed to gain access.

Protocol Sequence Observed:

    Request: USER [username]

    Response: 331 Password required

    Request: PASS [password]

    Response: 530 Login incorrect (Repeated across all sessions)

4. Conclusion

The attempt to breach the FTP server at 192.168.0.20 was identified as a failed brute-force attack. There is no evidence of unauthorized data access, file exfiltration, or directory traversal.
5. Recommendations

    Account Lockout Policy: Implement a policy to temporarily lock accounts after 5 failed attempts to thwart automated tools.

    IP Shunning: Configure the firewall or IPS to automatically drop traffic from 192.168.0.21 if it exceeds a specific threshold of failed logins.

    Transition to SFTP: Move from plain-text FTP to SFTP (Port 22) to encrypt credentials and prevent sniffing within the network.

    Investigation of .21: Scan the attacker host (192.168.0.21) for unauthorized scripts or malware that may have initiated the attack.

End of Report
