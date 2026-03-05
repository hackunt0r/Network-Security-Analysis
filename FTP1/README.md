Incident Analysis Report: FTP Brute-Force Investigation

Report ID: SOC-20260305-01

Analyst: Ahmed Yasser (Hackunter)

Date: March 5, 2026

Status: Closed – Mitigation Successful
1. Executive Summary

    Incident Type: Brute Force Attack (FTP)

    Status: Blocked / Failed (No unauthorized access gained)

    Severity: Medium (High frequency of attempts)

    Detection Source: Network Traffic Analysis (PCAP)

On March 5, 2026, a network traffic analysis was conducted on two PCAP files identifying a high-volume Brute-Force Attack targeting the internal FTP server. Analysis of the protocol response codes confirms that the attack was unsuccessful, as no administrative or user access was granted during the captured window.
2. Incident Details

    Target Server IP: 192.168.0.21 (FTP Server)

    Attacker Source IP: 192.168.0.20 (Suspected Compromised Host)

    Target Port: 21 (FTP)

    Attack Method: Credential Stuffing / Dictionary Brute-Force

    Tools Identified: Likely Hydra or an automated script (indicated by high request frequency).

    Analysis Tools: Wireshark, Tshark.

3. Technical Analysis & Evidence

Based on the analysis of the provided PCAP files, the traffic shows a rapid succession of USER and PASS commands from the attacker (.20) to the server (.21).
Key Findings:

    Traffic Pattern: The traffic exhibits a classic Dictionary Attack pattern, iterating through multiple common usernames and passwords in a very short duration.

    Failure Pattern: 100% of server responses returned FTP Response Code 530 (Login incorrect).

    Success Verification: A global filter for ftp.response.code == 230 returned zero results across both PCAP files, confirming that no successful login occurred.

    Session Behavior: Frequent RST (Reset) and FIN (Finish) flags indicate sessions were terminated immediately following each failed attempt.

Protocol Sequence Observed:

    Request: USER [username]

    Response: 331 Password required

    Request: PASS [password]

    Response: 530 Login incorrect (Repeated across all sessions)

4. Technical Artifacts

    Wireshark Filter Applied: ip.addr == 192.168.0.20 && ip.addr == 192.168.0.21 && ftp

    Log Correlation (Hypothetical):

        Linux: Entries in /var/log/auth.log would reflect FAILED PASSWORD.

        Windows: Event ID 4625 (An account failed to log on) would be generated thousands of times.

5. Recommendations & Remediation
Immediate Action:

    IP Shunning: Permanently block Source IP 192.168.0.20 at the Network Firewall.

        Command: sudo iptables -A INPUT -s 192.168.0.20 -j DROP

Long-term Hardening:

    Account Lockout Policy: Implement a policy to temporarily lock accounts after 5 failed attempts.

    Intrusion Prevention: Deploy Fail2Ban to automate the detection and blocking of Brute Force sources.

    Secure Protocol: Decommission legacy FTP and migrate to SFTP (Port 22) to encrypt credentials and prevent sniffing.

    Host Investigation: Scan the attacker host (192.168.0.20) for unauthorized scripts or malware.

6. Analyst Conclusion

The incident was a high-volume but unsuccessful attempt to compromise FTP credentials on the server 192.168.0.21. No data exfiltration or system compromise was detected. However, the persistence of the attacker warrants inclusion of this IP in the corporate blacklist to prevent future escalation.

Analyst Signature: Ahmed Yasser (Hackunter) Junior SOC Engineer

End of Report
