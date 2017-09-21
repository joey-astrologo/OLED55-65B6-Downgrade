This release 7.29 is a Stable Release

ENHANCEMENTS in 7.29

1) Major Debug and Code Fix for Zone Replication under heavy load

ENHANCEMENTS in 7.10

1) Subnet Selection can now be manually configured.
2) Target Relay Agent can be overridden.
3) HTTP Interface can be restricted.
4) HTTP Interface blocking fixed
5) Bug DNS Receive failure was fixed.
6) PDF Manual was included.

DOWNLOAD

The latest version can be downloaded from http://dhcp-dns-server.sourceforge.net/

INSTALLATION

This program if installed on a machine, which has manually configured for IP Address,
DNS Server, it detects these settings from server machine (If you want different
setting for clients, edit ini file) and uses them for configuring other machines on local
network.

1) Installer automatically installs the Program
2) For Windows NT/2000/XP/Vista/7 you should let installer install it as NT Service.

RUNNING

1) Start Menu -> Dual Server -> Run Standalone.
2) If you have installed as NT Service, goto Services Applet and may start/stop
   the service "Dual DHCP DNS Service".

   Now try getting IP Address for another machine, it should work.

CONFIGURATION

1) Configuration of Server

   The program automatically picks up settings of Server computer and 
   no configuration is required if:-

   a) Server machine is configured for DNS Servers.
   b) You use IP range 192.168.0.1 - 192.168.0.255 (Server is automatically excluded).
   c) Lease Time is 36000 secs/10 hours.
   
   You can change any of these settings in DualServer.ini file or in included file. You
   need to restart if any changes are made in configuration file. In Primary/Secondary
   opration, secondary server need not be restarted.
   
2) Configuration of Clients
   1. You can use windows/Linux/Unix mix of clients.
   2. The clients should be set to get IP Address automatically.
   3. Do not configure the clients for Dynamic DNS Update, as this server automatically
      does this.

GENERAL
   
1) This document is for guidance and is covered under GNU public license.

AVAILABILITY OF INTERFACES

DualServer needs at least one static interface to listen. If your static interface is not
available when system boots, You can use Recovery option on Windows Service Applet, let it
try starting every 1 minutes or so. Specify "Restart the Service" for First Failure, Second
Failure, Subsequent Failures. When the interface becomes active, DualServer would start.

INTERNET CONNECTION SHARING.

Internet connection sharing needs 3 services a) DNS Server b) DHCP Service c) Gateway Service.
DNS and DHCP Service is provided by this server. However gateway service is provided by 
Operating System itself.

If you are using any Windows Server version, you can enable IP Routing in Administration tools,
and run along with Dual DHCP DNS Server.

However with Windows XP, there is a Microsoft Article on this:-

http://support.microsoft.com/kb/315236 or look at http://www.home-network-help.com/ip-forwarding.html
It did not work for me. ICS do work but ICS Forces its own DNS and DHCP Services on target
network(s), which prevents Dual DHCP DNS Server being used. Dual DHCP DNS Server has rich
features compared to ICS DNS/DHCP Services. To prevent ICS using its DHCP/DNS Service, while
setting ICS, select some unused network (say Microsoft Loopback Adaptor) only as target network,
so that Dual DHCP DNS Server can still be used on other networks. ICS can be configured clicking 
"MyNetworkPlaces->Setup Home or Small Office Network". There are good blogs for this:-

http://blogs.msdn.com/virtual_pc_guy/archive/2005/10/04/477195.aspx
http://www.jyu.fi/erillis/thk/en/ic-guides/network/nat/winxp

Alternatively you can use some Proxy Server or better Socks Proxy Server like antinat. This alternative
would require the clients Browsers to be set to this proxy server.

DEBUG

If program is not assigning addresses or resolve them check:-

1) Check network hardware and ensure that client machines has different host names from server and
   each other.
2) No other service should be running on Server on ports 53 and 67.
3) If you get error like port 67 already in use means some other DHCP program or proxy server with DHCP 
   service is running. If you get error port 53 already in use means some other DNS Server or proxy server
   with DNS Service is already running. Use any port scanner program like Active Ports to detect which program is
   listening on these ports. It is also possible that another copy of dualserver itself is running.
   or Microsoft connection sharing (ICS) is running, which uses these ports. How to disable ICS DNS/DHCP
   Service on your target networks, see above INTERNET CONNECTION SHARING.
4) If you still get error Static Interfaces/Listening Ports not available, it may be because of 1) Another
   DHCP/DNS Server is running or Interfaces specified on [LISTEN-ON] section are not available.
   If your interface may not be ready when your computer/Service starts and due to this service
   fails to start, use Window's recovery option in Services applet to try starting service at later time.
5) Look at DualServer.log (if running as service) or Run in standalone mode, it will provide all debug
   information as it verbatim the activities.
6) If you use Broadband router, which also has DHCP Server, this program may still run, but some hosts
   configured by other DHCP Server may not use this DNS or DHCP Service.
7) Often you will find that other machines can resolve each other and server, but server resolves
   nothing. This is because server's DNS Server may be different. To resolve machines from server,
   specify (forwarding) DNS Servers in ini file and set server's DNS Server to itself.
8) Zone Transfer and Replication, if used on multihomed servers, make sure the IPs for Primary and Secondary
   server can be pinged from Each Other.
9) Dualserver.state file backs up current leases and is read back when server restarts. If you want to 
   clean previous leases, you may delete this file and restart the server.
10)If you are not able to receive DHCP Discover messages from clients, make sure that Dual DHCP DNS Server
   and client are on same physical network (not separated by routers). If it is separated by routers and it
   is same subnet, please allow routers to pass broadcast messages to Server on Port 67. If these are 
   different subnets, use the BOOTP relay agents.
11) Make sure that firewall is allowing udp/tcp packets on ports 53 and 67.
12) If you have just upgraded from previous version and program just dies or fails to start, please
    delete Dualserver.state file and retry.

UNINSTALLATION

	Goto control panel's add/remove programs and remove the program.

REPLICATION (for advance users only)

	Release 5.0 onwards support zone replication. Please read the ZONE REPLICATION file.

Domain Name Server and Child Zones. (for advance users only)

	If you have a domain with multiple child zones, each zone having its own DNS/DHCP or DNS server, You
	can use a multizone DNS Server like bind as Main Domain Name Server, which would transfer different
	zones from child servers. Dual Server can be very well used as Child Zone Servers with DHCP/DNS for
	zone and Domain Name Server would have all the zones transferred from Child Servers.

	Dual Server, being single zone server, still provides a way to work as main Domain Name Server for
	whole domain. It works differently but with same results, without even the need to transfer child zones.
	You can configure 1 or more Dual Servers as Master DNS Server (Domain DNS Server) for whole domain
	and these will forward queries to specific zone servers based on zone match. Queries not matching to zones
	specified in [CONDITIONAL_FORWARDERS] section will be forwarded to default forwarding DNS Servers. One query will
	be forwarded only once to Child server and then cache is used to resolve unless it has expired.
	Please see the [CONDITIONAL_FORWARDERS] section of ini file.
	
	Entries in [CONDITIONAL_FORWARDERS] behave differently than forwarding server entries in [DNS-SERVERS] section.
	Child Servers are basically meant for child  zones queries. For correct behavior you should specify
	child zone servers in [CONDITIONAL_FORWARDERS] like:-
	
	[CONDITIONAL_FORWARDERS]
	childzone1.workgroup.com=xxx.xxx.xxx.xxx, xxx.xxx.xxx.xxx
	1.168.192.in-addr-arpa=xxx.xxx.xxx.xxx, xxx.xxx.xxx.xxx
	childzone2.workgroup.com=xxx.xxx.xxx.xxx
	2.168.192.in-addr-arpa=xxx.xxx.xxx.xxx

	The child zone can have just one DNS Server or can have primary and secondary with zone replication between 
	them. Domain Server would contact primary or secondary child zone server, whichever is available. You can
	specify one server also, if child zone has only 1 server.

 	Main Domain Name servers should be specified as forwarding servers in child zone dns servers, so that
	each zone can also resolve entries in sibling child zones through main Domain Name Server. ISP or Public
	DNS Servers should only be specified in main Domain Name Servers.

	If you need full backup, minimum 2 DualServers should be used as main Domain Name Server for main domain
	with identical configuration and each child zone should have 2 DualServers, with zone replication between
	them only.
	
BUGS
	If you find any problem with this program or need more features, please send mail to achaldhir@gmail.com.
	You may also send thanks email if it works fine for you.
	
DONATIONS

	If you find that this program is suitable for your office environment and you are using it, Please consider
	some donation for this project. $10-$50 do not make any difference to office, but it does make difference
	to Project.

