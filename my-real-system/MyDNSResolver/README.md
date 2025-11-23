What needs to be implemented as part of this project
A Recursive DNS Resolver  This means your resolver will:

✔ 1. Accept a domain query (like google.com)
✔ 2. Build a DNS query packet manually
✔ 3. Send it to the root DNS servers
✔ 4. Parse response to get TLD servers
✔ 5. Send query to TLD servers
✔ 6. Parse response to get authoritative servers
✔ 7. Send query to authoritative DNS
✔ 8. Parse final A/AAAA/CNAME records
✔ 9. Apply TTL-based caching
✔ 10. Implement iterative logic
✔ 11. Handle retries, UDP timeouts
✔ 12. Return final result to the user
✔ 13. Build a CLI like dig

The Idea is build your Recursive Resolver, deploy and then reroute all your DNS traffic to your local Resolver. 
    See if it works fine. TO-DO- Explore How can we do that.

Core Concept to learn
1) DNS Packet structure
2) UPD Socket Programming
3) Name compression in packets
4) Recusrice vs iterative resolution
5) How DNS chaching works
6) TTL and cache invalidation
7) EDNS basics
8) DNSSEC


                      [ Root DNS ]
                     /     |      \
                [.com]   [.org]   [.in]     ← TLD servers
                /   \
         google.com   facebook.com          ← Authoritative servers


Step1 - Stub Resolver - Its our OS or Browser, it has no intelligent, just forwards the resquest to your network's resolver
    like your ISP or 8.8.8.8
    Responsibility - Very Dumb, forwards Queury, caches for few seconds( OS -level), No Recursion

Step2 - Recursive Resolver - This is where all the work is done, it aggressively caches
    Contancts Root->TLD->Authoritative servers, Validated DNSSec, handle retries, naegative cache rate-limits. This is brain of DNS
    Responsibility - Recieves query from CLients, check cache, if miss go to root, retyr failures, paraller resolve NS if needed, DNSSec validation,
        Block Malicious domains, return final UP to CLient
Step3 - Root DNS
    Responsibilities - Tell recursive resolvers "which TLD nameserver handles this domain?", Never return an actual A/AAAA record.
Step4 - TLD servers( Top Level Domain servers)
Step4 - Authoritative DNS servers These are final Source of Truth Ex - Google authoritative DNS, AWS Route53, cloudfare Authoritativ DNS
    Custom DNS hosted on AZUE/AWS
    They store - A records, AAAA, CNAME, MX,NS,TXT,SOA,SRV. These servers actually return the actual IP of the domain.