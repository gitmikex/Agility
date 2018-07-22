LAB 1 Device Management
=======================

Goal:

The following labs will get you familiar with the management of BIG-IP
devices.

The following steps have been completed for you to ensure the proper
setup of the lab environment.

-  A Data Collection Device (DCD) for statistical analysis has been
   added to the BIG-IQ CM.

-  Two standalone BIG devices have been imported to BIG-IQ CM, and the
   services on them have been imported.

-  A DNS Sync Group has been established with the two standalone BIG-IP
   DNS devices

Tasks:

1. Import BIG-IP device to BIG-IQ for centralized management and
   inventory

2. Review BIG-IP cluster status and configurations

3. Automate device backups and archive a copy of the backup file

Note

**Important:** Before you attempt to add the BIG-IP cluster, make sure
that the devices are **‘In Sync’** from a configuration standpoint, or
you will get an error when attempting to import. You will need to access
one of the devices directly to do this. Log in to either BOS-BIGIP and
sync the configurations if they are not in sync.

Task 1.1: Import BIG-IP devices for management and inventory
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To start managing a BIG-IP® device, you must add it to the BIG-IP
Devices inventory list on the BIG-IQ® system.

Adding a device to the BIG-IP Devices inventory is a two-stage process.

Stage 1:

-  You enter the IP address, port (if other than default), and
   credentials of the BIG-IP device you're adding, and associate it with
   a cluster (if applicable).

-  BIG-IQ opens communication (establishes trust) with the BIG-IP
   device.

-  BIG-IQ discovers the current configuration for any selected services
   you specified are licensed on the BIG-IP system, like LTM®
   (optional).

Stage 2:

-  BIG-IQ imports the licensed services configuration you selected in
   stage 1 (optional).

The basic discovery allows for device inventory, device health
monitoring, backup and restore of the managed device, integration with
F5’s iHealth service, software upgrade, and device template deployment.
As part of the discovery process, you can choose to manage other parts
of the BIG-IP configuration.

In this scenario, we will import a BIG-IP device and associate it with
an existing Cluster, review the device information available in BIG-IQ,
export our inventory to a CSV file, and review the inventory.

    Adding devices to BIG-IQ Inventory:

Dependencies:

1. The BIG-IP device must be located in your network.

2. The BIG-IP device must be running a compatible software version.

3. Port 22 and 443 must be open to the BIG-IQ management address, or any
   alternative IP address used to add the BIG-IP device to the BIG-IQ
   inventory.

K34133507: BIG-IQ Centralized Management compatibility matrix

K15073: BIG-IQ software support policy

Big-IP Devices
^^^^^^^^^^^^^^

1. Log in to the BIG-IQ system with your user name (admin) and password
   (admin).

2. On the top menu bar, select Devices from the BIG-IQ menu.

3. On the left-hand menu bar, click BIG-IP Devices.

4. Click the Add Device button in the main pane.

   a. In the IP Address field, type the IP address of the device:
      **10.1.10.10**

   b. In the User Name and Password fields, type the user name (admin)
      and password (admin) for the device.

   c. Cluster Display Name: **Use Existing**.

   d. Select a Cluster: **BostonCluster**

   e. Leave everything else default.

|image0|

1. Click the Add button to add this device to BIG-IQ.

2. BIG-IQ now exchanges certs with the BIG-IP and pops up a window for
   the administrator to select which modules to manage from BIG-IQ. For
   this device, select LTM, ASM, AFM and DNS Services. Keep the
   Statistics monitoring boxes all checked, and then click the Continue
   button.

    |image1|

1. The discovery process will start, and you should see a screen similar
   to the following screenshot. At this point, BIG-IQ is using REST
   calls to the BIG-IP to pull the selected parts of the BIG-IP
   configuration into BIG-IQ.

|image2|

Allow the import jobs to complete. At this point, the configuration of
the BIG-IPs that have been imported are not yet editable in BIG-IQ. To
make the configurations editable in BIG-IQ, we need to complete the
import tasks.

1. On the Device Inventory screen, click the |image3|\ link in the
   Services column for BOS-vBIGIP01. (you may need to scroll right to
   see the services column)

2. In the Local Traffic (LTM) Section, select the check box for “Create
   a snapshot of the current configuration before importing” and click
   the Import button.

|image4|

1. In the Application Security (ASM) Section, select the check box for
   “Create a snapshot of the current configuration before importing” and
   click the Import button.

|image5|

There may be a window that pops up and ask you to Resolve Import
Conflicts, click Accept to resolve.

A conflict is when an object that is already in the BIG-IQ working
config has the same name, but different contents as an object that
exists on the BIG-IP that is being imported. The user must select
whether to keep the object from BIGIP or BIGIQ configuration. Storage
will be updated accordingly. Review the differences that have been
discovered as part of this import by clicking on each row in the
difference view.

Leave all default to “BIG-IQ” to keep the version of the objects that
are already in BIG-IQ.

|image6|

Click the continue button.

A window reminds us that the BIG-IP will be modified to use the BIG-IQ
objects during the next deployment. Click the Resolve button to
continue.

|image7|

1. In the Advanced Firewall (AFM) Section, select the check box for
   “Create a snapshot of the current configuration before importing” and
   click the Import button.

|image8|

Again, you will experience the conflict resolution screens. Choose to
keep the objects that are already on the BIG-IQ.

1. In the BIG-IP (DNS) Section, click the Import button.

|image9|

1. Click the back arrow button at the top of the section to return to
   the inventory.

   |image10|

2. Once you have completed all of the import tasks for BOS-vBIGIP02,
   click the arrow in the upper left of the Services panel to return to
   the device inventory screen.

   |image11|

3. Click on the BOS-vBIGIP01.termmarc.com device link to review the
   device Properties, Health, and Services information for the device.

4. Click through the Properties, Health, Statistics Collection, and
   Services tabs to review the information.

   |image12|

5. Click the arrow in the upper left of the Services panel to return to
   the device inventory screen.

   |image13|

6. Click the Export Inventory button in the main pane to review the
   contents of the device inventory CSV file

7. The CSV file is automatically downloaded to your client. Launch the
   CSV file from your downloads folder. For example, in Chrome the CSV
   file will appear in the lower left.

   |image14|

8. Review the contents of the file and understand all of the information
   that is provided.

   |image15|

Task 1.2: Review BIG-IP cluster status and configurations
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

BIG-IQ provides visibility in to the BIG-IP cluster configuration and
status. You can review the sync status, drill down to review cluster
configuration, and initiate cluster sync activities from BIG-IQ.

**Big-IP Clusters - (DSC) Device Service Clusters**

    Device Service Clustering, or DSC, is a BIG-IP TMOS feature that
    lets you organize BIG-IP devices in groups allowing for
    synchronization of configuration objects.

    To start managing DSC devices you must add devices configured in DSC
    to the BIG-IP Device Inventory. Discover the devices into BIGIQ
    Inventory by assigning the clustered devices to a BIGIQ clustered
    group.

1.  Log in to the BIG-IQ system with your user name and password.

2.  On the top menu bar, select Devices from the BIG-IQ menu.

3.  On the left, click BIG-IP CLUSTERS.

4.  Click DSC Groups.

    |image16|

5.  You can skip the discovery steps since the DSC groups have been
    discovered for you.

6.  | Mouse over the status icons to see the status of each of the
      discovered clusters
    | |image17|

7.  | Click on the cluster Name to view more details about the cluster.
    | Choose the device-group failover link for your BostonCluster.
    | |image18|

8.  Review the Properties tab and notice that you can start a cluster
    sync operation from the bottom of the screen.

    |image19|

9.  Review the Traffic Groups tab\ |image20|

10. Click the n objects link to view the objects in the traffic group.
    Close the window after the review.

Task 1.3: Automate device backups and archiving a copy of the backup file 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

BIG-IQ provides the ability to backup individual or groups of managed
devices on an ad-hoc or a scheduled basis. The admin can decide how long
to retain the backups on BIG-IQ and has the option of archiving a copy
of the UCS backup off to an external device for DR or deeper storage
purposes.

In this scenario, we are going to create a group of all of the devices
in our Boston data center and schedule a nightly backup that archives a
copy off to our archive for DR purposes.

First, we need to create the group for our backup schedule to reference.
We have two options in BIG-IQ: static groups, where devices are added
and removed manually and dynamic groups, where devices are selected from
a source group based on filter criteria. In this lab setup, the devices
have BOS in the name to indicate that they are in the Boston data
center. This makes the dynamic group the logical choice.

1. On the top menu bar, select Devices from the BIG-IQ menu.

2. Click Device Groups in the left-hand menu

3. Click Create in the main pane

4. Complete the settings to create the group.

+-----------------+---------------------------------+
| Name            | **BostonDCGroup**               |
+=================+=================================+
| Group Type      | **Dynamic**                     |
+-----------------+---------------------------------+
| Parent Group    | **Root (All BIG-IP Devices)**   |
+-----------------+---------------------------------+
| Search Filter   | **BOS**                         |
+-----------------+---------------------------------+

|image21|

1. Click the Save & Close button to save the group.

Now, we can create our backup schedule that references this dynamic
group.

1. Click on the Back Up & Restore on the left-hand menu

2. Click on Backup Schedules

   |image22|

3. Click the Create button in the main pane

4. Fill out the Backup Schedule

+--------------------------+------------------------------------------------------+
| Name                     | **BostonNightly**                                    |
+==========================+======================================================+
| Local Retention Policy   | **Delete local backup copy 3 days after creation**   |
+--------------------------+------------------------------------------------------+
| Backup Frequency         | **Daily**                                            |
+--------------------------+------------------------------------------------------+
| Start Time               | 00:00 Eastern Standard Time                          |
+--------------------------+------------------------------------------------------+

Under Devices, select the **Groups radio button**

Select from the drop-down **BostonDCGroup**

In the Backup Archive section, enter the following:

+-------------+------------------------------------+
| Archive     | **Store Archive Copy of Backup**   |
+=============+====================================+
| Location    | **SCP**                            |
+-------------+------------------------------------+
| User name   | **F5**                             |
+-------------+------------------------------------+
| Password    | **default**                        |
+-------------+------------------------------------+
| Directory   | **/home/f5**                       |
+-------------+------------------------------------+

|image23|

|image24|

1. Click Save & Close to save the scheduled backup job.

.. |image0| image:: media/image1.png
   :width: 6.49583in
   :height: 4.29167in
.. |image1| image:: media/image2.png
   :width: 6.49583in
   :height: 4.41667in
.. |image2| image:: media/image3.png
   :width: 6.50000in
   :height: 1.54167in
.. |image3| image:: media/image4.png
   :width: 1.60397in
   :height: 0.21872in
.. |image4| image:: media/image5.png
   :width: 6.50000in
   :height: 1.04444in
.. |image5| image:: media/image6.png
   :width: 6.50000in
   :height: 0.73333in
.. |image6| image:: media/image7.png
   :width: 6.48750in
   :height: 3.29167in
.. |image7| image:: media/image8.png
   :width: 5.17917in
   :height: 2.06667in
.. |image8| image:: media/image9.png
   :width: 6.50000in
   :height: 0.71667in
.. |image9| image:: media/image10.png
   :width: 6.50000in
   :height: 0.55903in
.. |image10| image:: media/image11.png
   :width: 2.26013in
   :height: 0.93738in
.. |image11| image:: media/image11.png
   :width: 2.26013in
   :height: 0.93738in
.. |image12| image:: media/image12.png
   :width: 6.49583in
   :height: 4.40833in
.. |image13| image:: media/image13.png
   :width: 3.92659in
   :height: 1.02071in
.. |image14| image:: media/image14.png
   :width: 2.45803in
   :height: 0.56243in
.. |image15| image:: media/image15.png
   :width: 6.50000in
   :height: 1.82639in
.. |image16| image:: media/image16.png
   :width: 6.45000in
   :height: 1.71250in
.. |image17| image:: media/image17.png
   :width: 2.73924in
   :height: 1.46857in
.. |image18| image:: media/image18.png
   :width: 4.35362in
   :height: 2.17681in
.. |image19| image:: media/image19.png
   :width: 6.50000in
   :height: 3.75000in
.. |image20| image:: media/image20.png
   :width: 4.80625in
   :height: 0.88320in
.. |image21| image:: media/image21.png
   :width: 6.55833in
   :height: 3.10417in
.. |image22| image:: media/image22.png
   :width: 2.28096in
   :height: 1.23943in
.. |image23| image:: media/image23.png
   :width: 6.35479in
   :height: 5.69259in
.. |image24| image:: media/image24.png
   :width: 6.50000in
   :height: 2.21319in
