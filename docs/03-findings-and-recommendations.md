# findings 1:

## What it is:
Web admin interface exposed on port 80 without HTTPS

## Evidence:
scans/03-router-serviceversion.txt
screenshots/port80ss.png

## Risk rating:
Medium

## Why it matters:
Because the interface uses HTTP instead of HTTPS, admin login credentials are sent across the network in plain text. Anyone with access to the network traffic - for example another device on the same network that's been compromised - could intercept those credentials with a basic packet sniffer and gain full control of the router.

## Reasoning
This is rated Medium rather than High because the interface is only reachable if an attacker is already on the local network somehow. If it were also reachable from the internet, this would be a High risk, since anyone could attempt to intercept or brute-force it.

## What to do about it to reduce the risk:
As the scan displayed in scans/03-router-serviceversion.txt that port 80 is only HTTP and isn't encrypted, I still tried going with https://192.168.0.1/ to see if it works to use HTTPS but it failed and redirected me back to HTTP.

To reduce the risk I've confirmed that WAN management is disabled so the interface is only reachable from inside my home network - this was the biggest risk reducer I could have done. I will avoid logging into the admin panel using public/shared Wi-Fi. And unfortunately I'll have to accept the risk of it and be cautious of how I log in.

# findings 2:

## What it is: 
Nmap host discovery overcounted the number of physical devices on the network

## Evidence:
scans/01-host-discovery.txt

## Risk rating:
Low / Informational

## Why it matters:
The host discovery scan (`nmap -sn 192.168.0.0/24`) reported 8 hosts up. However three of those IPs (192.168.0.84, 192.168.0.101, and 192.168.0.103) all shared the exact same MAC address (CA:AA:BD:XX:XX:XX), meaning it's one physical device showing as three different IP addresses and not three separate devices. Adding on, 192.168.0.21 did not have a MAC address listed, which is typical behaviour from nmap as it doesn't ARP-resolve the machine it's being run from. 192.168.0.21 being the laptop's IP address (the device the whole scan was run through) was confirmed via the command (`ipconfig`) in PowerShell. What I understood from all of this is that the network actually has 5 distinct devices and not 8. This isn't a vulnerability or anything, but what I learnt from this scan is that the raw scan shouldn't be valued without actually analysing the data it gives you, and that it could waste quite a bit of time having to analyse each IP, MAC address and other data it gives you.

The "Unknown" label is something minor, but then again, if deep analysis on the data given isn't done then unauthorized devices could easily blend in between them while being unnoticed. 

## Reasoning:
I rated this Low/Informational rather than a true vulnerability rating (High/Medium) because this finding is about scan interpretation and network visibility, not an exploitable weakness.

## What I did about it:
I checked my router's admin panel for a client list to try to identify the CA:AA:BD device by name, but it did not resolve to a friendly name and I could not identify it. As a next step, I would isolate it by temporarily disconnecting known devices one at a time to see which one continues to respond at the MAC address. For this assessment, I'm documenting it as an unresolved item - a genuine example of a scan raising a question rather than answering one, which is realistic for this kind of first-pass network review.

## Overall Risk Summary

| Rating | Count |
|--------|-------|
| High | 0 |
| Medium | 1 |
| Low / Informational | 1 |

## Lessons Learned

The biggest thing I took away from this project is that raw scan output on its own doesn't tell you
much - it's the analysis afterwards that actually matters. The host discovery scan reporting 8 hosts
when there were really only 5 distinct devices was a good example of that: if I'd just taken the
number at face value, I'd have drawn the wrong conclusion about my own network.

I was also surprised by how much of "fixing" a finding is actually just testing your assumptions.
I assumed the port 80 admin interface would just support HTTPS if I typed it in manually, and it
didn't - so the real fix ended up being about reducing exposure (confirming WAN management was off)
rather than the fix I originally expected to make.

If I extended this project further, I'd want to look into using Wireshark to actually capture and
inspect the plain-text traffic from the HTTP admin interface, to see the credential exposure
first-hand rather than just reasoning about it.