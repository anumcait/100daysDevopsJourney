## 🚀 Day 14 | KodeKloud 100 Days DevOps Challenge 🚀

### Problem Statement: 
The production support team of xFusionCorp Industries has deployed some of the latest monitoring tools to keep an eye on every service, application, etc. running on the systems. One of the monitoring systems reported about Apache service unavailability on one of the app servers in Stratos DC.

Identify the faulty app host and fix the issue. Make sure Apache service is up and running on all app hosts. They might not have hosted any code yet on these servers, so you don’t need to worry if Apache isn’t serving any pages. Just make sure the service is up and running. Also, make sure Apache is running on port 8082 on all app servers.

**Issue:** Apache service failing to start due to port 8082 already in use.

**🛠️ Fix Steps Performed**

1. Identify the app server where Apache was down

Log into each of the app servers and check Apache service status:

```bash
systemctl status httpd
```

If it’s not running, proceed to investigate further on that host.

2. Check if port 8082 is already in use

Run the following command on the problematic app server:
```bash
sudo netstat -tuln | grep 8082
```

You’ll see output like:

COMMAND   PID USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
someproc  1234 root   12u  IPv4 123456      0t0  TCP *:8082 (LISTEN)

**3. **Kill the process occupying port 8082

Once the process using the port is identified, kill it:
```bash
sudo kill -9 <PID>
```

Replace <PID> with the actual process ID (e.g., 1234).

You can confirm it has been killed by running the lsof or netstat command again.

4. Restart Apache HTTPD

Now restart the Apache service:
```bash
sudo systemctl start httpd
```

Enable it on boot:
```bash
sudo systemctl enable httpd
```
5. Confirm Apache is running on port 8082

Check if Apache is now listening on port 8082:

```bash
sudo netstat -tulnp | grep 8082
```

You should see something like:

tcp6       0      0 :::8082                :::*                    LISTEN      1234/httpd


Also confirm Apache status:
```bash
systemctl status httpd
```
✅ Result

🔧 Process occupying 8082 killed

🔄 Apache restarted successfully

📡 Apache confirmed running on port 8082 on all app servers

##
