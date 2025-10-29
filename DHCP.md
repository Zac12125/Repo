When a computer boots up and needs a dynamic IP address, it uses DHCP (Dynamic Host Configuration Protocol):



1\. \*\*DHCP Discover\*\*: The computer broadcasts a discovery message across the local network asking "Is there a DHCP server here?"



2\. \*\*DHCP Offer\*\*: Any DHCP server on the network responds with an offer containing an available IP address, subnet mask, default gateway, DNS servers, and lease duration.



3\. \*\*DHCP Request\*\*: The computer formally requests that specific IP configuration from the server by broadcasting a request message.



4\. \*\*DHCP Acknowledgment\*\*: The server confirms the assignment and marks that IP as leased to that computer's MAC address for the specified time period.



The whole exchange takes milliseconds. The computer now has network connectivity with its assigned IP address. Before the lease expires, the computer automatically attempts to renew it. If the DHCP server is unreachable, the computer may assign itself an APIPA address (169.254.x.x range) or remain without network access depending on configuration.



This process happens entirely automatically, which is why you can plug a device into most networks and get online without manual IP configuration.

