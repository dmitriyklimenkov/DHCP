# DHCP

# Задание:
- Настроить DHCPv4
- Настроить DHCPv6

# 1 Настроим DHCPv4.

 # Схема лабораторного стенда:
 
 ![](https://github.com/dmitriyklimenkov/DHCP/blob/main/%D0%A1%D1%85%D0%B5%D0%BC%D0%B0.PNG)
 
 # Таблица адресации:
| Устройство | Интерфейс  |   IP адрес   | Маска подсети | Шлюз по умолчанию |
| :----------|:----------:| :-----------:|:-------------:| -----------------:|
| R1         | G0/0/0   | 10.0.0.1  | 255.255.255.252 |                   |
|            | G0/0/1   |            |                |                   | 
|            |          |              |               |                     |
|            | G0/0/1.100   |            |               |                   | 
|            | G0/0/1.100   |           |               |                    |
|            | G0/0/1.1000  |             |               |                   |  
| R2         | G0/0/0       | 10.0.0.2    |255.255.255.252 |                    |
|            | G0/0/1       |             |               |                   |
| S1         | VLAN200      |  |  |        |
| S2         | VLAN 1     |  |  |        |
| PC-A       | NIC        | DHCP  | DHCP | DHCP       |
| PC-B       | NIC        | DHCP  | DHCP | DHCP       |

# Таблица VLAN:
|     VLAN      | Имя | Интерфейс |
| :------------ |:---------------:| -----:|
| 1      | N\A        |  S2: F0/18  |
| 100      | Clients        | S1: Fa0/6 |
| 200      | MGMT        | S1: F0/1-4, F0/7-24, G0/1-2 S2: F0/2-17, F0/19-24, G0/1-2 |
| 999      | Parking Lot            |               |
| 1000      | Native            |      N\A         |


# Конфигурация устройств:

S1:
```
!
interface FastEthernet0/1
 description Parking_Lot
 switchport access vlan 999
 switchport mode access
 shutdown
!
interface FastEthernet0/2
 description Parking_Lot
 switchport access vlan 999
 switchport mode access
 shutdown
!
interface FastEthernet0/3
 description Parking_Lot
 switchport access vlan 999
 switchport mode access
 shutdown
!
interface FastEthernet0/4
 description Parking_Lot
 switchport access vlan 999
 switchport mode access
 shutdown
!
interface FastEthernet0/5
 switchport trunk native vlan 1000
 switchport trunk allowed vlan 100,200
 switchport mode trunk
!
interface FastEthernet0/6
 switchport access vlan 100
 switchport mode access
!
interface FastEthernet0/7
 description Parking_Lot
 switchport access vlan 999
 switchport mode access
 shutdown
!
interface FastEthernet0/8
 description Parking_Lot
 switchport access vlan 999
 switchport mode access
 shutdown
!
interface FastEthernet0/9
 description Parking_Lot
 switchport access vlan 999
 switchport mode access
 shutdown
!
interface FastEthernet0/10
 description Parking_Lot
 switchport access vlan 999
 switchport mode access
 shutdown
!
interface FastEthernet0/11
 description Parking_Lot
 switchport access vlan 999
 switchport mode access
 shutdown
!
interface FastEthernet0/12
 description Parking_Lot
 switchport access vlan 999
 switchport mode access
 shutdown
!
interface FastEthernet0/13
 description Parking_Lot
 switchport access vlan 999
 switchport mode access
 shutdown
!
interface FastEthernet0/14
 description Parking_Lot
 switchport access vlan 999
 switchport mode access
 shutdown
!
interface FastEthernet0/15
 description Parking_Lot
 switchport access vlan 999
 switchport mode access
 shutdown
!
interface FastEthernet0/16
 description Parking_Lot
 switchport access vlan 999
 switchport mode access
 shutdown
!
interface FastEthernet0/17
 description Parking_Lot
 switchport access vlan 999
 switchport mode access
 shutdown
!
interface FastEthernet0/18
 description Parking_Lot
 switchport access vlan 999
 switchport mode access
 shutdown
!
interface FastEthernet0/19
 description Parking_Lot
 switchport access vlan 999
 switchport mode access
 shutdown
!
interface FastEthernet0/20
 description Parking_Lot
 switchport access vlan 999
 switchport mode access
 shutdown
!
interface FastEthernet0/21
 description Parking_Lot
 switchport access vlan 999
 switchport mode access
 shutdown
!
interface FastEthernet0/22
 description Parking_Lot
 switchport access vlan 999
 switchport mode access
 shutdown
!
interface FastEthernet0/23
 description Parking_Lot
 switchport access vlan 999
 switchport mode access
 shutdown
!
interface FastEthernet0/24
 description Parking_Lot
 switchport access vlan 999
 switchport mode access
 shutdown
!
interface GigabitEthernet0/1
!
interface GigabitEthernet0/2
!
interface Vlan1
 no ip address
 shutdown
!
interface Vlan100
 mac-address 0050.0f11.0301
 ip address 192.168.1.2 255.255.255.192
!
interface Vlan200
 mac-address 0050.0f11.0302
 ip address 192.168.1.66 255.255.255.224
!
ip default-gateway 192.168.1.65
!
```
Подобно на S2.

Конфигурация R1:
```
ip dhcp excluded-address 192.168.1.1 192.168.1.5
!
ip dhcp pool POOL-1
 network 192.168.1.0 255.255.255.192
 default-router 192.168.1.1
 domain-name CCNA-lab.com
ip dhcp pool R2_Client_LAN
 network 192.168.1.96 255.255.255.240
 default-router 192.168.1.97
 domain-name CCNA-lab.com
!
!
interface GigabitEthernet0/0/0
 ip address 10.0.0.1 255.255.255.252
 ip helper-address 10.0.0.2
 duplex auto
 speed auto
 ipv6 address FE80::1 link-local
 ipv6 address 2001:DB8:ACAD:2::1/64
!
interface GigabitEthernet0/0/1
 no ip address
 duplex auto
 speed auto
 ipv6 address FE80::1 link-local
 ipv6 address 2001:DB8:ACAD:1::1/64
 !
interface GigabitEthernet0/0/1.100
 description USERS
 encapsulation dot1Q 100
 ip address 192.168.1.1 255.255.255.192
!
interface GigabitEthernet0/0/1.200
 description MGMT
 encapsulation dot1Q 200
 ip address 192.168.1.65 255.255.255.224
!
ip route 0.0.0.0 0.0.0.0 10.0.0.2 
!
ip flow-export version 9
!
ipv6 route ::/0 2001:DB8:ACAD:2::2
!
```

Конфигурация R2:
```
!
interface GigabitEthernet0/0/0
 ip address 10.0.0.2 255.255.255.252
 duplex auto
 speed auto
 ipv6 address FE80::2 link-local
 ipv6 address 2001:DB8:ACAD:2::2/64
!
interface GigabitEthernet0/0/1
 description MGMT
 ip address 192.168.1.97 255.255.255.240
 ip helper-address 10.0.0.1
 duplex auto
 speed auto
 ipv6 address FE80::1 link-local
 ipv6 address 2001:DB8:ACAD:3::1/64
!
ip route 0.0.0.0 0.0.0.0 10.0.0.1 
!
ip flow-export version 9
!
ipv6 route ::/0 2001:DB8:ACAD:2::1
!
```
Пропингуем с R1 до R2:
```
R1#ping 10.0.0.1
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.0.0.1, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 4/7/14 ms
```
Проверка на ПК1:
```
C:\>ipconfig /renew

   IP Address......................: 192.168.1.6
   Subnet Mask.....................: 255.255.255.192
   Default Gateway.................: 192.168.1.1
   DNS Server......................: 0.0.0.0

C:\>
```
Проверка на ПК2:
```
C:\>ipconfig /renew

   IP Address......................: 192.168.1.98
   Subnet Mask.....................: 255.255.255.240
   Default Gateway.................: 192.168.1.97
   DNS Server......................: 0.0.0.0

C:\>
```
Пропингуем шлюз:
```
C:\>ping 192.168.1.97

Pinging 192.168.1.97 with 32 bytes of data:

Reply from 192.168.1.97: bytes=32 time<1ms TTL=255
Reply from 192.168.1.97: bytes=32 time<1ms TTL=255
Reply from 192.168.1.97: bytes=32 time<1ms TTL=255
Reply from 192.168.1.97: bytes=32 time<1ms TTL=255
```

# 2 Настроим DHCPv6.

 # Таблица адресации:
| Устройство | Интерфейс  |   IPv6 адрес   | 
| :----------|:----------:| :-------------:|
| R1         | G0/0/0     |2001:db8:acad:2::1 /64      | 
|            | G0/0/1     |2001:db8:acad:1::1/64       | 
| R2         | G0/0/0       | 2001:db8:acad:2::2/64   |
|            | G0/0/1       | 2001:db8:acad:3::1 /64   |   
| PC-A       | NIC          | DHCP        | 
| PC-B       | NIC          | DHCP        | 

Конфигурация R1:
```
int g0/0/0
ipv6 address 2001:db8:acad:2::1/64
ipv6 address fe80::1 link-local
no shutdown
exit
int g0/0/1
ipv6 address 2001:db8:acad:1::1/64
ipv6 address fe80::1 link-local
no shutdown
exit
ipv6 route ::/0 2001:db8:acad:2::2
ipv6 unicast-routing
```
Конфигурация R2:
```
int g0/0/0
ipv6 address 2001:db8:acad:2::2/64
ipv6 address fe80::2 link-local
no shutdown
exit
int g0/0/1
ipv6 address 2001:db8:acad:3::1/64
ipv6 address fe80::1 link-local
no shutdown
exit
ipv6 route ::/0 2001:db8:acad:2::1
ipv6 unicast-routing
```
Проверка связности:
```
R1#ping 2001:db8:acad:3::1
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 2001:db8:acad:3::1, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 0/0/1 ms
```
Проверка SLAAC от R1:
```
C:\>ipconfig

FastEthernet0 Connection:(default port)

   Connection-specific DNS Suffix..: CCNA-lab.com
   Link-local IPv6 Address.........: FE80::20A:41FF:FE55:61CB
   IPv6 Address....................: ::
   ```
Настройка на R1 DHCP сервера:
```
ipv6 dhcp pool R1-STATELESS
dns-server 2001:db8:acad::254
domain-name STATELESS.com
exit
interface е0/1
ipv6 nd other-config-flag
ipv6 dhcp server R1-STATELESS
exit
```
Проверка на ПК1:
```
VPCS> sh ipv6 all

NAME   IP/MASK                                 ROUTER LINK-LAYER  MTU
VPCS1  fe80::250:79ff:fe66:6803/64
       2001:db8:acad:1:2050:79ff:fe66:6803/64  aa:bb:cc:00:10:10  1500
```
Настройка на R2 DHCP relay:
```
interface e0/1
ipv6 nd managed-config-flag
ipv6 dhcp relay destination 2001:db8:acad:2::1 e0/0
```
Проверка на ПК2:
```
VPCS> sh ipv6 all

NAME   IP/MASK                                 ROUTER LINK-LAYER  MTU
VPCS1  fe80::250:79ff:fe66:6804/64
       2001:db8:acad:3:2050:79ff:fe66:6804/64  aa:bb:cc:00:20:10  1500
```
