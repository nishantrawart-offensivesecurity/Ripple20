
# "Ripple20" Treck IOT/ICS device discovery and exploit detection
<p align="center">
  <img width="230" height="150" src="r20_logo.png">
</p>

<br>
Ripple20 is a set of 19 vulnerabilities discovered in 2020 within a TCP/IP software library developed by Treck, Inc
</br>
  
## Summary:  
A Zeek package for the passive detection of Treck devices, discovery/scanning attempts and exploitation of the "Ripple20" set of vulnerabilities in the Treck TCP/IP stack. 

## References: 
- https://corelight.blog/2020/06/30/ripple20-zeek-package-open-sourced/
- https://www.jsof-tech.com/ripple20/    
- python/scapy scanning package provided by JSOF.  
- https://treck.com/vulnerability-response-information/
- https://www.us-cert.gov/ics/advisories/icsa-20-168-01
- https://www.kb.cert.org/vuls/id/257161

## Notices raised:   

| Notice | Fidelity  |
| -------- | ---------------------- |
|Treck device has been observed based on IP/TTL artifacts - method 1| medium/high | 
|Treck device has been observed based on IP/TTL artifacts - method 2| medium/high |
|Treck device has been observed based on TCP artifacts| medium |
|Treck device has been observed based on ICMP artifacts|high |
|The JSOF scanning tool (or derivative) has been observed - method 1| high |
|The JSOF scanning tool (or derivative) has been observed - method 2| high |
|Treck IP-in-IP encapsulation exploit outer packet detected| high|
|Treck IP-in-IP encapsulation exploit inner packet detected| high|
  
High Fidelity means high confidence of a True Positive.
By default all high and medium notices are raised, however if you like you can turn the medium notice off with `enable_medium_fidelity_notices = F` in `scripts/config.zeek`.


Each notice includes a small amount of packet metadata which is useful for triage and refinement. 


| msg in notice.log | debug added to msg |
| -------- | ---------------------- |
|Treck device ICMP artifacts have been observed. If 10.1.2.3 is an unpatched Treck device, it could be impacted by the 'Ripple20' vulnerabilities involving the Treck TCP/IP stack https://www.jsof-tech.com/ripple20/ |<debug info:icmp=[orig_h=10.1.2.3, resp_h=10.1.133.37, itype=166, icode=0, len=6, hlim=1, v6=F]>|
JSOF Ripple20 scanner has been observed coming from 10.1.133.37 (window scale=123). https://www.jsof-tech.com/ripple20/ | <debug info: pkt=[is_orig=T, DF=F, ttl=64, size=44, win_size=8192, win_scale=123, MSS=0, SACK_OK=F]>
Treck device TCP artifacts have been observed. If unpatched, the device at 10.1.2.3 could be impacted by the 'Ripple20' vulnerabilities involving the Treck TCP/IP stack https://www.jsof-tech.com/ripple20/ |<debug info: pkt=[is_orig=F, DF=F, ttl=64, size=48, win_size=8760, win_scale=0, MSS=1460, SACK_OK=F]>|
|Treck device TTL artifacts have been observed (method1). If 10.1.2.3 is an unpatched Treck device, it could be impacted by the 'Ripple20' vulnerabilities involving the Treck TCP/IP stack https://www.jsof-tech.com/ripple20/|<debug info: get_current_packet_header() = [l2=[encap=LINK_ETHERNET, len=62, cap_len=62, src=_mac redacted_, dst=_mac redacted_, vlan=<uninitialized>, inner_vlan=<uninitialized>, eth_type=2048, proto=L3_IPV4], ip=[hl=20, tos=0, len=48, id=32027, ttl=64, p=6, src=10.1.2.3, dst=10.1.133.37], ip6=<uninitialized>, tcp=[sport=80/tcp, dport=18902/tcp, seq=3766815773, ack=1001, hl=28, dl=0, reserved=0, flags=18, win=8760], udp=<uninitialized>, icmp=<uninitialized>]>|
|JSOF Ripple20 scanner has been observed coming from 10.1.133.37 (RST from responder on ports 40509->40508) . https://www.jsof-tech.com/ripple20/||
|Treck device TTL artifacts have been observed (method2). If 10.1.2.4 is an unpatched Treck device, it could be impacted by the 'Ripple20' vulnerabilities involving the Treck TCP/IP stack https://www.jsof-tech.com/ripple20/ |<debug info: get_current_packet_header() = [l2=[encap=LINK_ETHERNET, len=54, cap_len=54, src=_mac redacted_, dst=_mac redacted_, vlan=<uninitialized>, inner_vlan=<uninitialized>, eth_type=2048, proto=L3_IPV4], ip=[hl=20, tos=16, len=40, id=33734, ttl=64, p=6, src=10.1.2.4, dst=10.1.133.37], ip6=<uninitialized>, tcp=[sport=40508/tcp, dport=40509/tcp, seq=0, ack=1, hl=20, dl=0, reserved=0, flags=20, win=0], udp=<uninitialized>, icmp=<uninitialized>]>|
|JSOF Ripple20 scanner has been observed coming from 10.1.133.37 (window scale=123). https://www.jsof-tech.com/ripple20/ |<debug info: pkt=[is_orig=T, DF=F, ttl=64, size=44, win_size=8192, win_scale=123, MSS=0, SACK_OK=F]>|


## Usage:
- To use against a pcap you already have ```zeek -Cr scripts/__load__.zeek your.pcap```  

- If using in a live environment, the script ```ripple20_nonclusterized.zeek``` is written for a non clustered live Zeek environment. This script can still be loaded and will work as intended for some of the notices in a clustered environment, however to be fully effective a different version of this script that supports Zeek clusters is required. Contact the author for more detail (Ben Reardon, Research Team @Corelight. @benreardon or ben.reardon [at] corelight.com)
