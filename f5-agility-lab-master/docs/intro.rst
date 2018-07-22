Getting Started
---------------

.. TODO:: Complete getting started instructions

Overview
This document details the lab exercises and demonstrations that comprise the hands-on component of the BIG-IQ v6.0. The environment is setup with basic configuration and associated traffic generation to populate dashboards for easy demos.  Additional configuration can be added to support items that are not currently covered. 

This lab environment is designed to allow for quick and easy demos of a significant portion of the BIG-IQ product.  The Linux box in the environment has multiple cron jobs that are generating traffic that populates the monitoring tab.  

An F5® BIG-IQ® Centralized Management solution can involve a number of different elements. The topology for these elements depends on your needs, and on whether you include data collection devices (DCDs) in your solution. A typical solution can include the following elements:
•	BIG-IQ system(s)
•	BIG-IP devices
•	Data collection devices (optional)
•	Remote storage devices


.. NOTE::
	 We recommend that all work for this lab be performed from your local Web browser or from the windows jumphost. No installation with your local system is required.

Lab Topology
~~~~~~~~~~~~

.. TODO:: Complete lab topology

The following components have been included in your lab environment:

- 2 x F5 BIG-IP VE (v12.1)
- 1 x F5 iWorkflow VE (v2.1)
- 1 x Linux LAMP Webserver (xubuntu 14.04)
- 1 x Windows Jumphost

Lab Components
^^^^^^^^^^^^^^

.. TODO:: Complete lab components table

The following table lists VLANS, IP Addresses and Credentials for all
components:

Management
Network 10.1.1.0/24

BIGIQ_CM_v6.0.0-0.0.1591				10.1.1.4		primary
SEA-vBIGIP01.termmarc.com.v13.1.0.5 (VPN)		10.1.1.7		primary
BOS-vBIGIP01.termmarc.com.v13.1.0.5			10.1.1.8		primary
BOS-vBIGIP02.termmarc.com.v13.1.0.5			10.1.1.10	primary
ESXi 6.5.0 + vCenter					10.1.1.9		primary
BIGIQ_DCD_v6.0.0-0.0.1591				10.1.1.6		primary
Ubuntu 16.04 Lamp Server, Radius and DHCP		10.1.1.5		primary

Subnet 10 (External)
Network 10.1.10.0/24

BOS-vBIGIP01.termmarc.com.v13.1.0.5			10.1.10.8	primary
10.1.10.18
ESXi 6.5.0 + vCenter					10.1.10.9	primary
10.1.10.91
10.1.10.90
SEA-vBIGIP01.termmarc.com.v13.1.0.5 (VPN)		10.1.10.7	primary
Ubuntu 16.04 Lamp Server, Radius and DHCP		10.1.10.5	primary
BIGIQ_CM_v6.0.0-0.0.1591				10.1.10.4	primary
BIGIQ_DCD_v6.0.0-0.0.1591				10.1.10.6	primary
BOS-vBIGIP02.termmarc.com.v13.1.0.5			10.1.10.10	primary
10.1.10.19

.. list-table::
    :widths: 20 40 40
    :header-rows: 1
    :stub-columns: 1

    * - **Component**
      - **VLAN/IP Address(es)**
      - **Credentials**
    * - Sample Host
      - - **Management:** 10.1.1.250
        - **Internal:** 10.1.10.250
        - **External:** 10.1.20.250
      - ``admin``/``admin``


