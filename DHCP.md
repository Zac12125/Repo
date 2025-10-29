How a Computer Obtains a Dynamic IP Address



When a computer powers on and connects to a network, it needs an IP address to communicate with other devices. This address is typically obtained automatically through a protocol called DHCP (Dynamic Host Configuration Protocol), which uses a four-step process commonly abbreviated as DORA: Discover, Offer, Request, and Acknowledge.



&nbsp;Initial Boot and Network Interface Initialization



When the computer first boots up, the network interface card (NIC) initializes during the hardware startup sequence. At this point, the device has no IP address configured and cannot communicate using standard TCP/IP protocols. The NIC is activated and prepared to send and receive network traffic, but the operating system recognizes that it lacks the necessary network configuration parameters to function on the network.



&nbsp;The DHCP Discovery Phase



The first step begins when the client broadcasts a DHCPDISCOVER message across the local network. This message is sent as a broadcast because the client doesn't yet know where any DHCP servers are located, or even if any exist on the network. The broadcast is sent to the IP address 255.255.255.255 using UDP port 67, which is the standard port for DHCP server communication. The discover packet contains the client's MAC address, which serves as its hardware identifier, and indicates that the device is requesting network configuration information.



Since broadcast packets typically don't cross subnet boundaries, networks with multiple subnets often employ DHCP relay agents. These relay agents listen for DHCP broadcasts on their local segment and forward them as unicast messages to a centralized DHCP server located on a different subnet, inserting the gateway address (giaddr) field to indicate which subnet the client belongs to.



&nbsp;The DHCP Offer Phase



When a DHCP server receives the discovery broadcast, it responds with a DHCPOFFER message. The server selects an available IP address from its configured address pool, along with other network parameters like the subnet mask, default gateway, DNS server addresses, and lease duration. The offer is sent back to the client using a broadcast at the network layer (255.255.255.255) but is specifically addressed to the client's MAC address at the data link layer. This dual-layer addressing ensures the message reaches the correct device even though the client still lacks an IP address.



If multiple DHCP servers exist on the network, the client may receive multiple offers. The server also determines the lease time, which defines how long the client can use the assigned IP address before needing to renew it.



&nbsp;The DHCP Request Phase



After receiving one or more offers, the client selects one (typically the first offer received) and broadcasts a DHCPREQUEST message. This request is still broadcast rather than unicast because the client hasn't yet been officially assigned an IP address and wants to inform all DHCP servers on the network about its decision. The request message indicates which server's offer the client has accepted and implicitly rejects any other offers. This broadcast serves the dual purpose of confirming the client's choice to the selected server while notifying other servers that their offers were declined.



&nbsp;The DHCP Acknowledgement Phase



The chosen DHCP server responds with a DHCPACK (acknowledgement) message sent via unicast to the client. This final message confirms the IP address assignment and includes all the configuration parameters: the leased IP address, subnet mask, default gateway, DNS servers, and the lease duration. Upon receiving this acknowledgement, the client configures its network interface with the provided settings and can now fully participate in network communications. The client also starts internal timers to track when it needs to renew its lease.]



&nbsp;IP Address Lease Management



The assigned IP address isn't permanent; it comes with a lease time that determines how long the client can use it. Two critical timers begin counting down once the lease is granted: the T1 renewal timer and the T2 rebinding timer. The T1 timer is set to 50% of the lease duration, and when it expires, the client attempts to renew its lease by sending a unicast DHCPREQUEST directly to the original DHCP server. If the server is available and approves, it sends back a DHCPACK, and the lease timer resets.]



If the original server doesn't respond and the T2 timer expires (at 87.5% of the lease time), the client enters a rebinding state. At this point, it broadcasts a DHCPREQUEST to any available DHCP server on the network, attempting to extend its lease with an alternative server. If renewal or rebinding fails and the lease fully expires, the client must release its IP address back to the pool and restart the entire DORA process from the beginning.



&nbsp;Network Communication Protocols



Throughout this process, DHCP relies on UDP (User Datagram Protocol) rather than TCP because UDP supports broadcasting, which is essential for the initial discovery and request phases. The client sends messages from UDP port 68 to the server's port 67, while server responses travel from port 67 to the client's port 68. This port structure maintains clear communication channels between clients and servers even when multiple devices are simultaneously requesting addresses.



While DHCP handles IP address assignment, another protocol called ARP (Address Resolution Protocol) becomes important for actual network communication. ARP maps IP addresses to MAC addresses, allowing devices to send data frames to the correct physical hardware on the local network. Once a computer has obtained its dynamic IP address through DHCP, it uses ARP to discover the MAC addresses of other devices it needs to communicate with.

