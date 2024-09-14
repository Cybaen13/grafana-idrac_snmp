# Ultimate iDRAC Grafana Dashboard (Telegraf SNMP Based)
:computer: :bar_chart: :clipboard:
SNMP Based Dashboard to Monitor Dell Hosts via iDRAC 
Adapted to work with FLUX language for InfluxDB V2 from the work of [ilovepancakes95](https://github.com/ilovepancakes95)
Grafana Dashboard ID: [21915](https://grafana.com/grafana/dashboards/21915)    
![Screenshot 1](https://grafana.com/api/dashboards/12106/images/7943/image)
# iDRAC - Host Stats (Flux)
Dashboard based on dashboard [12106](https://grafana.com/grafana/dashboards/12106-idrac-host-stats/) by [ilovepancakes95](https://grafana.com/orgs/ilovepancakes95).
Modified to use flux language to work natively with infuxdb v2. Also added variables for easier configuration.
## Details
SNMP Based Dashboard to Monitor Dell Hosts via iDRAC

[Github Repository](https://github.com/Cybaen13/grafana-idrac_snmp) : please report issues or provide improvements 

## How To Use
1. Enable SNMPv1 in the iDRACs you wish to monitor. 
2. Install and setup Telegraf, InfluxDB, and Grafana to work with eachother. 
3. Use the provided idrac-input.conf file and replace the values for “idracURLx” under “agent” with your own iDRAC IPs or hostnames. 
4. Restart Telegraf. 
5. Import the dashboard json file (or use Grafana Dashboard ID) to add the dashboard and panels to Grafana, selecting your own InfluxDB database after clicking “Import”. 
6. Setup the variables according to your configuration (specifically the "bucket" variable to replace with your bucket name). See below.
### Variables description
* idrac_host : list of the idrac hosts referenced in influxDB : MODIFICATION AT YOUR OWN RISKS AND PERIL
* bucket : the name of the bucket to use in influxDB
* hostslist_key : the data key defined in idrac-input.conf (by default "idrac-hosts", should not need any modification)

### OS Data
The OS data part will need the installation of the  IDRAC Service module on the host os:
* [Manual](https://dl.dell.com/topicspdf/dell-idrac-service-module-20_install-guide_fr-fr.pdf)
* [Download](https://www.dell.com/support/home/en-us/product-support/)
* [Supported Servers](https://www.dell.com/support/manuals/en-us/idrac-service-module-5.0/ism_5.3.0.0_rn_pub/supported-platforms?guid=guid-8db41623-cac0-476b-aa5a-b12acc1b86c3&lang=en-us)
* [Supported OS](https://www.dell.com/support/manuals/en-us/idrac-service-module-5.0/ism_5.3.0.0_rn_pub/supported-operating-systems-and-hypervisors?guid=guid-f971f0d8-305c-45a0-9caa-8271edf809e2&lang=en-us)
* [Dell Details](https://www.dell.com/support/kbdoc/en-us/000178050/support-for-dell-emc-idrac-service-module#iDRAC-Download)

Data may take up to 2 minutes to fully populate the first time. Enjoy!

## Features
Same as the original:
* Uses Grafana variables to dynamically pull in all iDRACs listed in the Telegraf config file, and draw a new “row” section for each iDRAC that gets added.
* Displays summary table and global status “heat” map of all iDRACs being polled.
* Summary table pulls in each iDRAC URL so clicking System Name in the table brings you directly to that iDRAC’s logon page.
* Panels and table cells change color to indicate failures or other status messages.
* Variable selection box allows fine-tuning of which systems are displayed on the dashboard. (Default is “All”).
* Each system’s section on the dashboard is dynamically drawn based on variable selection to show the following data for each host:
   * Uptime, Global Status, Power State, PSU Status, CMOS Battery Status, RAID Battery Status, Storage Status, RAM Status, &     Thermal Status
   * Service Tag, BIOS Version, and Intrusion Sensor Status
   * OS Name and OS Version Table
   * System Power (in watts) Graph
   * CPU Temp Graph
   * System Air Temp Graph
   * Fan Speed Graph
   * Physical Disk Status Table (Disk Name, Capacity, Status, Predictive Fail Alarm)
   * System Log Entries Table
   * Network Interfaces Table (NIC Name, Vendor, Status, MAC Address)
* Adding more data is as simple as adding the appropriate iDRAC OID to the Telegraf config file, and adding a panel to display the new data on the dashboard (See below).

## Build Environment
* Grafana for visualization of data
* Telegraf for data collection with SNMP Input Plugin
* InfluxDB V2 for time series and table data storage
* iDRAC with SNMP Enabled (v1)
   * Tested with iDRAC 7 and iDRAC 8 on Dell Poweredge r720xd and r730xd servers by [ilovepancakes95](https://grafana.com/orgs/ilovepancakes95).
   * Tested with iDRAC 8 and iDRAC 9 on Dell Poweredge r730 and r340 servers by [me](https://grafana.com/orgs/cybaen).

## Adding Data and panels
To pull and use additional data, you can use the following resources:
* [Dell MIB Files](https://www.dell.com/support/article/en-us/sln285502/how-to-find-dell-management-information-base-mib-files?lang=en) : The MIBs files listing the OIDs delivered by the iDRAC. The idrac-input.conf is based on it.
* [MIB Browser](https://www.ireasoning.com/mibbrowser.shtml) : Browser for mibs file
* [Dell EMC OpenManage SNMP Reference Guide](https://dl.dell.com/topicspdf/openmanage-software-v92_connectivity-guide_en-us.pdf) : Official SNMP reference guide for iDRAC


## Pre-requirements
### Telegraf:
net-snmp or snmp installed (snmptable and snmptranslate) 
### Grafana:
install the following plugins:
* [clock](https://grafana.com/grafana/plugins/grafana-clock-panel)
* [statusmap](https://grafana.com/grafana/plugins/flant-statusmap-panel)

