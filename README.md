# Cisco-failover
Conf for Cisico failover router with HSRP

R1 (Primary/Active)
R2 (Secondary/Standby)
VIP: 192.168.1.1
R1 real IP: 192.168.1.2
R2 real IP: 192.168.1.3
Interface: GigabitEthernet0/0



R! conf :
R1(config)# interface GigabitEthernet0/0
R1(config-if)# ip address 192.168.1.2 255.255.255.0
R1(config-if)# standby 1 ip 192.168.1.1
R1(config-if)# standby 1 priority 110   
R1(config-if)# standby 1 preempt        
R1(config-if)# standby 1 authentication md5 key-string secretpass
R1(config-if)# no shutdown


R2 conf :
R2(config)# interface GigabitEthernet0/0
R2(config-if)# ip address 192.168.1.3 255.255.255.0
R2(config-if)# standby 1 ip 192.168.1.1
R2(config-if)# standby 1 priority 100   
R2(config-if)# standby 1 preempt
R2(config-if)# standby 1 authentication md5 key-string secretpass
R2(config-if)# no shutdown


                          +-----------+
                          |   ISP     |
                          +-----------+
                                |
                                | (Public IP)
                                |
                        +-----------------+
                        |     Switch      |
                        +-----------------+
                            |         |
                            |         |
                 +----------------+   +----------------+
                 |    R1 (Active)  |   |   R2 (Standby) |
                 | 192.168.1.2     |   | 192.168.1.3    |
                 | VIP: 192.168.1.1|   | VIP: 192.168.1.1|
                 +----------------+   +----------------+
                            |  
                            | (LAN)
                            |
                    +-------------------+
                    |      Clients       |
                    | (PCs, Servers, APs)|
                    +-------------------+


