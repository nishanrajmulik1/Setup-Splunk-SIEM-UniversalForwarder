📂 Task 2: Ingest Syslog of a Remote Ubuntu Machine Using Universal Forwarder 🔄
Step 1: Install Splunk Universal Forwarder on the Remote Machine
Open Terminal on the remote Ubuntu machine and run:
```bash
wget -O splunkforwarder-ubuntu.deb https://download.splunk.com/products/universalforwarder/releases/latest/linux/splunkforwarder-latest-linux-2.0-amd64.deb
```
Install the Universal Forwarder:
```bash
sudo dpkg -i splunkforwarder-ubuntu.deb
```

Move to the Splunk Forwarder directory:
```bash
cd /opt/splunkforwarder/bin
```

Accept the license and enable Splunk Universal Forwarder at boot:
```bash
sudo ./splunk enable boot-start --accept-license
```

Step 2: Configure Universal Forwarder to Send Syslogs to Splunk Server
Set the Splunk indexer (Splunk Server) as the forwarding destination:
```bash
sudo ./splunk add forward-server <splunk-server-ip>:9997
```
Replace <splunk-server-ip> with your actual Splunk server's IP address.

Step 3: Configure Syslog Monitoring on the Remote Machine
Add a monitoring input for syslog:
```bash
sudo ./splunk add monitor /var/log/syslog -index main -sourcetype syslog
```

(Optional) Add authentication logs:
```bash
sudo ./splunk add monitor /var/log/auth.log -index main -sourcetype authlog
```

Restart the Universal Forwarder:
```bash
sudo ./splunk restart
```
Step 4: Enable Receiving on Splunk Indexer
On the Splunk Server, open Terminal and run:
```bash
sudo /opt/splunk/bin/splunk enable listen 9997
```

Verify that the Universal Forwarder is connected:
```bash
sudo /opt/splunk/bin/splunk list forward-server

```

Step 5: Verify Log Ingestion in Splunk
Open the Splunk Web Interface at:
```bash
http://<splunk-server-ip>:8000
```
Run the following Splunk Query in the Search & Reporting module:
```bash
index=main sourcetype=syslog | table _time host sourcetype message
```

If logs from the remote machine appear, syslog forwarding is successful! ✅


📸 Screenshots
![Successful Logs]./(screenshots/splunk%20syslog%20collection.png)


🎯 Key Learnings
- Hands-on experience with Splunk SIEM for security monitoring.
- Understanding log collection and analysis in cybersecurity.
- Configuring and managing Splunk services on Linux.
- Implementing Universal Forwarder for remote log ingestion.
