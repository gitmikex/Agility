Lab 5 Partial Deployment & Roll Back
====================================

Goal:

In this lab, we will demonstrate how to manage Virtual Servers on the
managed BIG-IP devices.

The objects that you manage using BIG-IQ® depend on associations with
other, supporting objects. These objects are called \ ***shared
objects***.

When the BIG-IQ evaluates a deployment to a managed device, it starts by
deploying the device-specific objects. Then it examines the managed
device to compile a list of the objects that are needed by other objects
on that device. Then (based on the recent analysis) the BIG-IQ deletes
any shared objects that exist on the managed device but are not used. So
if there are objects on a managed device that are not being used, the
next time you deploy changes to that device, the unused objects are
deleted.

You have the option to choose whether you want to evaluate all of the
changes, or specify which changes to evaluate. Select either \ **All
Changes** or **Partial Changes** from the selected source.

If you choose to do a partial deployment, additional controls are
displayed.

Tasks:

5.1: Create multiple changes. Deploy single change

5.2: Create and deploy multiple changes with selected roll-back

Task 5.1: Create multiple changes. Deploy single change
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Objective
^^^^^^^^^

-  The user has the ability to select a specific change out of many made
   for deploy. We will try to add an additional node to the existing
   pool in this task.

Partial Deployment
^^^^^^^^^^^^^^^^^^

-  From the tab Configuration, click on LOCAL TRAFFIC > Pools, enter
   “app1pool” in the upper right Filter and search, select a pool by
   clicking on name “app1pool” on either BOS-vBIGIP01 or 02.

   |image0|

-  1\ :sup:`st` change

   -  Click on New Member, select from Existing Node “app1node2” on port
      80 HTTP

    |image1|

-  Leave everything else default, and click on “Save and Close” on lower
   right

   We have just made a change to the BIG-IQ configuration for app1pool
   on the BOS HA pair.

-  2\ :sup:`nd` change – Create a New Monitor “mon-https”

   -  Click into Configuration > LOCAL TRAFFIC > Monitors and then click
      on “Create” button.

      |image2|

   -  New Monitor

      -  Name: **mon-https**

      -  Type: **HTTPS**

      -  Monitor template: **https**

      -  username: **admin**

      -  password: **admin** and confirm password.

   -  Click “Save and Close”

|image3|

Next, we will add the new monitor to the app2pool.

-  Add newly created Health Monitor “mon-https” to Pool “app2pool”

   -  Under Configuration > LOCAL TRAFFIC > Pools, search app2pool in
      the upper right filter

   -  Select a pool by clicking on name “app2pool” on either
      BOS-vBIGIP01 or 02

   -  On Health Monitors, select /Common/mon-https

|image4|

-  Click Save and Close

-  Switch back to Deployment tab, under EVALUATE & DEPLOY, click on
   Local Traffic & Network

   Next, we will create evaluation and deploy this change we just made
   above

   -  Click on top Deployment tab, select under EVALUATE & DEPLOY: Local
      Traffic & Network

   -  Click Create under Evaluations and enter the following:

      Name: **partial-deploy**

      From Evaluation > Source Scope, Select “\ **Partial Changes**\ ”

      From Source Objects > Available, select “Pools”, from pool list,
      select **only “app1pool**\ ” for Both BOS-vBIGIP01 & 02, and add
      them to Selected on the right

      Under Target Devices, click “Find Relevant Devices”, select both
      and add to right

      Click “Create” to complete

|image5|

After the evaluation is done, you can click on the “view” link under the
Difference column for “partial-deployment” evaluation.

|image6|

|image7|

    Note

*Only changes to “app1pool” will be deployed.* The monitor change on
app2pool will not be deployed.

-  Deploy changes

   -  Cancel to dismiss the popup window and click on Deploy under
      Evaluation

   -  Confirm by click on Deploy button again.

|image8|

After deployment is complete, click into the “partial-deploy” to view
the details of the deployment.

|image9|

    Note

The deployment could fail if the targeted BIG-IP devices are not in full
sync on configurations, due to timeout on waiting for sync to complete
on target devices. Ensure the devices are in full sync before deploying
changes.

Task 5.2: Create and deploy multiple changes with selected roll-back. 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Objective
^^^^^^^^^

In this deployment, we will be using the 1\ :sup:`st` change made to
app1pool, as well as 2\ :sup:`nd` change made to app2pool in the
previous task 5.1, to demonstrate the ability to partially roll back one
of the two changes for this deployment.

First, we will need to deploy the 2\ :sup:`nd` change that consists of a
new monitor to the app2pool.

-  Starting from the Deployment tab on the top, and under EVALUATE &
   DEPLOY, click on Local Traffic & Network on the left.

-  Click on Create under Deployments.

   Name: **Test-Deploy**

-  Ensure the default “Source Scope: All Changes” is selected

-  Select the BOS BIG-IP HA Pair from the Devices Targeted box and move
   them to the right Selected box.

-  Click on Create button on the bottom right to create the Evaluation
   of the deployment

-  Verify all changes are part of the deployment.

   -  New mon-https Health Monitor

      |image10|

-  Cancel the differences window to return to Evaluation list window,
   select **Test-deploy** and click on Deploy button above.

   |image11|

-  Click on Deploy button again to confirm and observe completion

   What we have done so far, is to deploy the 2\ :sup:`nd` change made
   to the HA pair in task 5.1, since we only deployed 1\ :sup:`st`
   change.

   Next, we will do a restore of the configurations on the HA pair, by
   rolling back one of the two changes we just made. We will need to use
   a previous snapshot made prior to the two changes done earlier, in
   order to restore one of the changes.

-  Create **a Partial Restore Evaluation**.

   -  Locate RESTORE section on the left and click on Local Traffic &
      Network.

   -  Under Restores section, click on Create button to start a task

   -  

|image12|

Name: **partial-restore**

Snapshot: **Reimport-10.1.1.10 (snapshot before the changes in task
5.1)**

Create Snapshot: check the box “\ **Create a snapshot prior to
restoring**\ ”.

Restore Scope: **Partial Restore**

Method: **Create evaluation**

    Note

Duplicate names are allowed for a snapshot; therefore, the Deployment
Date is provided as a reference.

User can narrow the scope of the restore from Full to Partial. For this
lab let’s select Partial Restore from the Restore Scope section.

User can “Create Evaluation” or if urgent “Restore Immediately”.

|image13|

-  Select “Add” for Source Objects

-  Select “/Common/app1pool” and click on “Add” to add the object to
   Selected tab.

-  Verify difference between BIG-IQ and Snapshot.

    |image14|

    |image15|

-  Click on Save to close the Select Object window, and then click on
   Create to start the evaluation

-  The user can restore the partial change defined from the Snapshot
   deployment.

    |image16|

    |image17|

    Click on Restore to complete the partial restore of the change made
    to app1pool.

    Close the complete window and click on View to see the restored
    configuration. You can see that the added member has been removed
    from app1pool.

.. |image0| image:: media/image1.png
   :width: 6.49583in
   :height: 4.47500in
.. |image1| image:: media/image2.png
   :width: 6.49583in
   :height: 4.87500in
.. |image2| image:: media/image3.png
   :width: 6.22917in
   :height: 2.67708in
.. |image3| image:: media/image4.png
   :width: 6.48958in
   :height: 4.21875in
.. |image4| image:: media/image5.png
   :width: 6.50000in
   :height: 4.22917in
.. |image5| image:: media/image6.png
   :width: 6.50000in
   :height: 4.92361in
.. |image6| image:: media/image7.png
   :width: 6.49583in
   :height: 2.84583in
.. |image7| image:: media/image8.png
   :width: 6.50000in
   :height: 3.32645in
.. |image8| image:: media/image9.png
   :width: 6.50000in
   :height: 3.50000in
.. |image9| image:: media/image10.png
   :width: 6.50000in
   :height: 3.65625in
.. |image10| image:: media/image11.png
   :width: 6.50000in
   :height: 3.28750in
.. |image11| image:: media/image12.png
   :width: 6.48750in
   :height: 3.07083in
.. |image12| image:: media/image13.png
   :width: 4.70833in
   :height: 1.05460in
.. |image13| image:: media/image14.png
   :width: 6.50000in
   :height: 4.94792in
.. |image14| image:: media/image15.png
   :width: 4.22917in
   :height: 2.20722in
.. |image15| image:: media/image16.png
   :width: 6.50000in
   :height: 4.43750in
.. |image16| image:: media/image17.png
   :width: 6.50000in
   :height: 1.57292in
.. |image17| image:: media/image18.png
   :width: 4.18547in
   :height: 2.20833in
