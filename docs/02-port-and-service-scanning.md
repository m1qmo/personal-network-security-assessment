## What I did:
In PowerShell

Ran this command to scan all 65,535 ports using -p- to bypass the 1,000 port scan limiter and -T4 ("aggressive") for the pacing of the scan. I found -T4 to be the most suitable for my home wifi where I'm not worried about being detected or the network struggling to keep up with the scan.
```powershell
nmap -p- -T4 --min-rate 500 192.168.0.1 -oN scans/02-router-fullports.txt
```

I ran the command above previously and port 5422 was open. However when I ran the scan again it didnt appear this time so I had it rechecked in case it was missed due to the -T4 ("aggressive") scan which had likely missed a few ports.
```powershell
nmap -p 5422 192.168.0.1 -oN scans/02b-port5422-recheck.txt
```

I finished up the port scan by actually detecting the services and versions of the ports that were detected using the previous commands. By running -sV I was able to see the type of service it used whether it was encrypted (ssl/http) or not (http) and found out that they all run on lighttpd (open-source web server)
```powershell
nmap -sV -p 80,443,5422 192.168.0.1 -oN scans/03-router-serviceversion.txt
```

## Why I did this:
I needed to know what's actually running on the ports found during the discovery.
The recheck of port 5422 was due to me previously running scan and having port 80, 443 and 5422 show up but this time 5422 did not show up so I wanted to make sure if it was actually closed or the scan just missed it due to the aggressive pacing using -T4.  

## What I found:
After rechecking with a slower, targeted scan, all three ports were confirmed open, all running the same web server software:

| Port | Service | Version |
|---|---|---|
| 80 | http | lighttpd |
| 443 | ssl/http | lighttpd |
| 5422 | http | lighttpd |

## What it means
From this assessment I've done on my local network using nmap scans here are two things I discovered. 
A. The pacing of the scan - an aggressive scan can produce incorrect results, so anything unexpected is worth re-checking with a slower, targeted scan before trusting it.
B. The router's setup interface on port 80 loads over HTTP with no forced HTTPS redirected. I had to test it out by typing http://192.168.0.1 to see if it forces it into a https://192.168.0.1 but unfortunately  it didnt as shown in screenshots/port80.png