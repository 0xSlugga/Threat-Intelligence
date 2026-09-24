# marioopy[.]site: Exposed Mythic C2 Panel

## Summary
Located a Mythic C2 operator panel exposed on port 7443 via a ThreatFox IP report, investigated the domain and the IP using WHOIS lookups and assigned SSL certificates, and determined that the server is likely malicious based on the age of the domain, and the fact that it was reported on ThreatFox. Reported abuse to the domain registrar and the server host. Currently still active, awaiting an update from Hostinger and HostVDS.

## Full IoC List
IP: 95.182.91[.]142\
Domain: marioopy[.]site\
HTML Title: Mythic\
Subject DN: O=Mythic\
Issuer DN: O=Mythic\
Application Port: 7443\
SSH Host Key Fingerprint: SHA-256 - 9956266b051dce46d62b3ec8c21911a925e0904ad2f58b01f9c12e48c24bab4b

## Discovery
On September 19th, I was browsing ThreatFox looking for live, uncharacterized, malicious infrastructure. While browsing, I located an IP IoC that had no associated tags. After finding this IoC, I entered it into Censys, and was able to determine that there were 3 web ports open (80, 443, and 7443).

## Fingerprinting
Since 7443 is a non-standard port I reviewed the HTTP response, and determined that the application being hosted on that port was an instance of Mythic C2 via the HTML Title (HTML Title: Mythic). On top of this, the SSL certificate has a subject and issuer DN of "O=Mythic", which further points at this being an instance of Mythic C2. In order to confirm, I connected to the domain (using Whonix) on port 7443, and was directed to a Mythic C2 login page. I did not attempt to authenticate with the application once I verified that I was looking at Mythic C2. While Mythic can be used for legitimate purposes, the upload of the IP to ThreatFox, coupled with the recently registered domain, strongly suggest malicious intent.

## Infrastructure
When looking this same IP up on Shodan, the server was showing as active in Russia, but on Censys it was showing active in the United States (San Francisco); but the SSH Host key fingerprint was exactly the same. Looking up the SSH Host Key Fingerprint on Censys returned no results other than the initially identified IP. While the geographic discrepancy was interesting, it was more than likely related to the geolookup services that each site uses.

## Reporting
After determining that the machine on Shodan and Censys was the same box reporting different geographic locations, I ran a WHOIS lookup on the associated domain name, which Censys showed as "marioopy[.]site". Through the WHOIS lookup, I was able to determine that the Domain Registrar was Hostinger, and I contacted their abuse team to have the domain suspended, or the DNS records wiped. I initially received a response from Hostinger informing that they had taken action and the application should no longer be up, but I was able to confirm that the Mythic C2 panel was still in fact online and operating as it was previously. On top of reporting this to the domain registrar, I also used the ASN data to find that the server is being hosted by the cloud provider HostVDS. I have also reported the abuse to the server host, and am awaiting a response from their end (9/22/26).

## Timeline
8/20/26: marioopy[.]site registered with Hostinger\
9/19/26: IP Discovered on ThreatFox, investigation begun, report sent to abuse@hostinger.com.\
9/21/26: Hostinger reports the "reported material(s) have been removed".\
9/22/26: marioopy[.]site still live, port 7443 still live (Censys, 16:10 UTC), domain still resolving, WHOIS not showing a hold, follow-up sent to Hostinger. Report also sent to abuse@hostvds.com
9/23/26: No response from either Hostinger or HostVDS.
9/24/26: Hostinger informed that the "malicious site is no longer accessble", the domain is still resolving so it was not suspended, and the site is still completely accessible on port 7443. Replied to Hostinger pushing for domain suspension.