Class 1 - Device Management
============================

**Goal:**

The following labs will get you familiar with the management of BIG-IP devices.

The following steps have been completed for you to ensure the proper setup of the lab environment.

-  A Data Collection Device (DCD) for statistical analysis has been added to the BIG-IQ CM.

-  Two standalone BIG devices have been imported to BIG-IQ CM, and the services on them have been imported.

-  A DNS Sync Group has been established with the two standalone BIG-IP DNS devices

**Tasks:**

1. Import BIG-IP device to BIG-IQ for centralized management and inventory

2. Review BIG-IP cluster status and configurations

3. Automate device backups and archive a copy of the backup file

.. NOTE::
    Before you attempt to add the BIG-IP cluster, make sure that the devices are **‘In Sync’** from a configuration standpoint, or you will get an error when attempting to import. You will need to access one of the devices directly to do this. Log in to either BOS-BIGIP and sync the configurations if they are not in sync.

.. toctree::
   :maxdepth: 1
   :glob:

   module*/module*