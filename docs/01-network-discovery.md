## What I did: 
In powershell:

```powershell
nmap -sn 192.168.0.0/24
```

So I ran a host discovery scan against my home network which is a lightweight scan that just checks which devices respond, without touching any ports on them. 

## Why: 
Before scanning any specific device for open ports, I need to know what is actually on the network. This is the reconnaissance/mapping step that comes first in any assessment.

## What I found: 

| IP Address | Device | How I identified it |
|---|---|---|
| 192.168.0.1 | Router | Default gateway address, MAC vendor = Commscope |
| 192.168.0.10 | Ring device | MAC vendor = Ring |
| 192.168.0.59 | Apple device | MAC vendor = Apple |
| 192.168.0.21 | My PC | Matches my own IPv4 from ipconfig, no MAC (can't ARP itself) |
| 192.168.0.84 / .101 / .103 | Phone | Same MAC across all three |
| 192.168.0.97 | ? | Different MAC to the group above |

The scan reported 8 "hosts up" but going through the MAC addresses showed it was really only 5 physical devices: My router (Commscope), my Ring device, an Apple device, my own PC, and a phone. Three of the 8 enteries all shared one identical MAC address, meaning they were the same physical device appearing at three different IP addresses over time, not three separate devices.

Not fully sure about `.97` — could be the phone showing up under yet another randomized MAC, or a separate 6th device, but I couldn't confirm either way from this scan alone.

## What it means: 
Two things worth drawing out. First, a raw scan count isnt automatically the truth as you have to actually read the data to know what you're really looking at, rather than reporting the first number you see. Second, the reason the phone kept reappearing under a different lookng, "Unknown" MAC each time is MAC address randomization which is a privacy feature modern phones use by default. Generating a random hardware-address-lookalike per network specifically so companies/trackers can't follow a device across networks by its permanent MAC address. That's also why I censored the device-unique portion of MACs before publishing this.