LAB 6 Local Traffic Management – Virtual Servers
================================================

Goal:

In this lab, we will demonstrate how to manage Virtual Servers on the
managed BIG-IP devices.

BIG-IQ is able to create nodes, monitors, pools, profiles, and virtual
servers, so a user can create and stage a new application directly on
the BIG-IQ, then deploy the change to the Managed BIG-IP device.

You will need to understand the basic workflow such as creating a new
Virtual Server on a Managed BIG-IP device.

For example, the following figure illustrates the basic workflow you
perform to manage the objects on BIG-IP® devices.

|image0|

Tasks:

6.1: Stage a new application on BIG-IQ for deployment

6.2: Create a new virtual server by cloning an existing virtual server

6.3: Create iRule and attach to multiple virtual servers

6.4: Deploy Staged Changes

6.5: Decommission a virtual server

Task 6.1: Stage a new application on BIG-IQ for deployment
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1.  Navigate to the **Configuration** tab on the top menu bar.

2.  | We will build our application starting at the nodes and making our
      way to the virtual servers. Navigate to **LOCAL TRAFFIC** >
      **Nodes**
    | |image1|

3.  | Click the Create button to create a node
    | |image2|

4.  | Fill out the configuration properties for the node
    | Name: **BIQAppNode1**
    | Device: **BOS-vBIGIP01.termmarc.com**
    | Address: **10.1.20.110**

    |image3|

5.  Click the **Save & Close** button in the lower right

6.  | Repeat steps 4 and 5 for the second node
    | Name: **BIQAppNode2**
    | Device: **BOS-vBIGIP01.termmarc.com**
    | Address: **10.1.20.121**

7.  | Verify that the MyApp nodes are created by typing MyApp in the
      filter box in the upper right and pressing return.
    | |image4|

8.  You should now see an entry for each of the MyApp nodes on BIG-IP01
    and BIG-IP02. \*\*\ ***When you create an object on a clustered
    device, BIG-IQ automatically replicates that configuration to the
    peer node in the staged configuration.***

    |image5|

9.  | Now we will create a pool with these nodes as pool members.
      Navigate to **LOCAL TRAFFIC > Pools**
    | |image6|

10. | Click the Create button to start creating your pool
    | |image7|

11. | Fill out the Pool Properties
    | Name: **BIQAppPool**
    | Device: **BOS-vBIGIP01.termmarc.com**
    | Health Monitors: **/Common/tcp**
    | Load Balancing Method: **Round Robin**

    |image8|

12. Click the **Save & Close** button in the lower right

13. | Click on the MyAppPool name in the list of pools to add pool
      members
    | |image9|

14. | Click on the New Member button under Resources to add pool members
    | |image10|

15. | Complete the Pool Member Properties for the first pool member
    | Node Type: Existing Node
    | Node: **BIQAppNode1**
    | Port: **80**

    |image11|

16. Click the **Save** button in the lower right to save the pool
    member.

17. Repeat steps 15 and 16 for the second pool member MyAppNode2 port
    80.

18. Click the **Save** **& Close** button in the lower right to save
    your pool.

19. | Now we will create a custom profile for our Virtual Server.
      Navigate to **LOCAL TRAFFIC > Profiles**
    | |image12|

20. | Click the Create button to create our custom profile
    | |image13|

21. | Fill out the Profile Properties
    | Name: **Source\_Addr\_Timeout\_75**
    | Type: **Persistence Source Address**
    | Parent Profile: **Source\_addr**
    | Timeout: **Specify 75 Seconds**
    | |image14|

22. Click **Save & Close** in the lower right.

23. | Now we will create our Virtual Server. Navigate to **LOCAL TRAFFIC
      > Virtual Servers**
    | |image15|

24. | Click the Create button to create the Virtual Server
    | |image16|

25. | Fill out the Virtual Server Properties
    | Name: **BIQAppVS**
    | Device: **BOS-vBIGIP01.termmarc.com**
    | Destination Address: **10.1.10.120**
    | Service Port **8088
      **\ HTTP Profile: **/Common/http**

    |image17|

26. | Scroll down and fill out the Resources
    | Default Pool: **BIQAppPool**
    | Default Persistence Profile: **Source\_Addr\_Timeout\_75
      **\ Leave all other options at their default settings.

    |image18|

27. Click **Save & Close** in the lower right

28. We now have staged our application and we will deploy it in a later
    workflow.

Task 6.2: Create a new virtual server by cloning an existing virtual server 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

BIG-IQ allows you to clone an existing virtual server to create a new
virtual server. This cloned virtual can be deployed to the same BIG-IP
device/cluster or to a new BIG-IP device/cluster. Cloning can be used to
migrate a virtual server or to make a virtual server that is similar to
one that already exists.

In this scenario, we will clone a virtual server from our cluster and
deploy it to our standalone device.

1. Navigate to the **Configuration** tab on the top menu bar.

2. | Navigate to **LOCAL TRAFFIC > Virtual Servers**
   | |image19|

3. | Type forward in the filter box on the right-hand side of the screen
     and press return.
   | |image20|

4. | Click the check box to the left of the first virtual server entry
   | |image21|

5. Click the **Clone** button under Virtual Servers at the top of the
   page

6. Edit the Virtual Server Properties

   | Name: **forward\_vs\_udf\_CLONED**
   | Device: **SEA-vBIGIP01.termmarc.com**

   | Destination Address: **1.0.0.0/8**
   | Type: **Forwarding (IP)
     **\ Protocol: **\*All Protocols**
   | VLANs and Tunnels: **Enabled on…**
   | VLANs and Tunnels: Selected: **internal**

   |image22|

7. Click the **Save & Close** button in the lower right

8. | Close out of the filter
   | |image23|

Task 6.3: Create iRule and attach to multiple virtual servers
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

BIG-IQ allows users to create iRules and use them on the virtual servers
that are managed by BIG-IQ. The iRules can be attached to individual
virtual servers or iRules can be attached to multiple virtual servers in
the same operation.

In this scenario, we will apply an iRule to a number of our virtual
servers that presents a maintenance page if none of the pool members
supporting the virtual are online and available.

1. Steps 1-5 show that you can create an iRule on the BIG-IQ directly.
   To save time the **VS\_maintenance\_irule** has already been created
   on the BIG-IQ.

   \*\*\ **Skip to step 6 to apply the already configured iRule.**

2. Navigate to the **Configuration** tab on the top menu bar.

3. | Select **LOCAL TRAFFIC > iRules**
   | |image24|

4. | Click the Create button under iRules
   | |image25|

5. | Fill out the iRule Properties page
   | Name: **VS\_maintenance\_irule**
   | Partition: **Common**
   | Body: as below,

when RULE\_INIT {

# sets the timer to return client to host URL

set static::stime 10

}

when CLIENT\_ACCEPTED {

set default\_pool [LB::server pool]

}

when HTTP\_REQUEST {

# Check if the URI is /maintenance

switch [HTTP::uri] {

"/maintenance" {

# Send an HTTP 200 response with a Javascript meta-refresh pointing to
the host using a refresh time

HTTP::respond 200 content \\

"<html><head><title>Maintenance page</title></head><body><meta
http-equiv='REFRESH' content=$static::stime;url=[HTTP::uri]></HEAD>\\

<p><h2>Sorry! This site is down for maintenance.</h2></p></body></html>"
"Content-Type" "text/html"

return

}

}

# If the default pool is down, redirect to the maintenance page

if { [active\_members $default\_pool] < 1 } {

HTTP::redirect "/maintenance"

return

}

}

*Source:*
`*https://devcentral.f5.com/codeshare/ltm-maintenance-page-lite* <https://devcentral.f5.com/codeshare/ltm-maintenance-page-lite>`__

|image26|

1. | Navigate to **LOCAL TRAFFIC > Virtual Servers**
   | |image27|

2. | Type ITwiki in the filter box on the right hand side of the screen
     and press return.
   | |image28|

3. | Click to select all the matching virtual servers
   | |image29|

4. Click the **Attach iRules** button at the top of the screen\ **
   **\ |image30|

5. | Fill out the Attach iRules section
   | iRules: Select the **VS\_maintenance\_irule** iRule
   | |image31|

6. Click **Save & Close** in the lower right.

7. | Clear the filter from the Virtual Servers
   | |image32|

Task 6.4: Deploy Staged Changes
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Now that we have staged a number of changes on the BIG-IQ, we will
evaluate the staged changes, and then deploy them to the BIG-IPs.

1)  Navigate to **Deployment** on the top menu bar.

2)  | Navigate to **EVALUATE & DEPLOY > Local Traffic & Network**
    | |image33|

3)  Click the **Create** button under **Evaluations**

4)  | Fill out the fields to Create Evaluation
    | Name: **DeployChgSet1
      **\ Source: **Current Changes**
    | Source Scope: **All Changes**
    | Unused Objects: **Removed Unused Objects**
    | Target Devices: Select Group “All ADC Devices” and move all
      devices to Selected

    |image34|

5)  | Click the Create button in the lower right
    | |image35|

6)  | After the evaluation completes, click the View link under
      Differences to review the changes that will be deployed.
    | |image36|

7)  | Review the staged changes for each device. Change devices with the
      selector in the upper left.
    | |image37|
    | Click on each change to review the differences.
    | |image38|

8)  | After you have reviewed all of the changes, click the Cancel
      button in the lower right
    | |image39|

9)  | Click on the name of the Evaluation to review the options
      available there
    | |image40|

10) Note that you can review the changes to be deployed on a device by
    device basis and you can choose to exclude a device from the
    deployment at this point. At the bottom of the page, you can
    schedule the deployment for a later time, or you can Deploy Now.
    Click the **Deploy Now** button to push the changes to the BIG-IPs.

|image41|

1) | Click the **Deploy** button
   | |image42|

2) | At the bottom of the screen, you can review that your changes are
     being deployed
   | |image43|

3) | Click on the name of the Deployment to review what was deployed
   | |image44|

4) Log in to BOS—vBIGIP01 using the TMUI link in UDF and confirm that
   your deployment was successful. You should now see the **MyAppVS** on
   the Network Map.

Task 6.5: Decommission a virtual server
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

BIG-IQ can be used to remove virtual servers, and other objects that are
no longer needed. The same staged change workflow applies for removal of
objects.

1. Navigate to the **Configuration** on the top menu bar.

2. | Navigate to **LOCAL TRAFFIC > Virtual Servers**
   | |image45|

3. | Select the top **HR-CLONE** virtual server
   | |image46|

4. Click the Delete button

5. Verify that you want to delete this virtual server from the BIG-IQ
   configuration.\ |image47|

6. Now we need to deploy this change. Navigate to the **Deployment**
   menu on the top menu bar.

7. | Navigate to **EVALUATE & DEPLOY > Local Traffic & Network**
   | |image48|

8. | Click the Create button under Deployments
   | |image49|

9. | Fill out the evaluation properties
   | Name: **DeleteVirtual**
   | Source: **Current Changes
     **\ Source Scope: **All Changes
     **\ Unused Objects: **Remove Unused Objects
     **\ Method: **Create Evaluation**
   | Target: Group, BostonCluster, both devices selected

|image50|

1. | Click the create button in the lower right.
   | |image51|

2. | After the evaluation completes, review the differences by clicking
     the view link under Differences.
   | |image52|

3. | Review the differences.
   | |image53|

4. | After you have reviewed all of the changes, click the Cancel button
     in the lower right
   | |image54|

5. | Click the Deploy button to push the changes to the BIG-IPs.
   | |image55|

6. | Verify that you want to deploy the changes to the selected devices.
   | |image56|

.. |image0| image:: media/image1.png
   :width: 6.25000in
   :height: 0.60417in
.. |image1| image:: media/image2.png
   :width: 2.29138in
   :height: 2.18723in
.. |image2| image:: media/image3.png
   :width: 1.91643in
   :height: 1.02071in
.. |image3| image:: media/image4.png
   :width: 4.53068in
   :height: 4.42653in
.. |image4| image:: media/image5.png
   :width: 3.44749in
   :height: 1.04154in
.. |image5| image:: media/image6.png
   :width: 6.50000in
   :height: 2.30556in
.. |image6| image:: media/image7.png
   :width: 2.28096in
   :height: 1.87477in
.. |image7| image:: media/image8.png
   :width: 1.98934in
   :height: 1.06237in
.. |image8| image:: media/image9.png
   :width: 6.50000in
   :height: 4.62014in
.. |image9| image:: media/image10.png
   :width: 6.50000in
   :height: 0.58611in
.. |image10| image:: media/image11.png
   :width: 1.36441in
   :height: 0.76032in
.. |image11| image:: media/image12.png
   :width: 5.25636in
   :height: 4.42407in
.. |image12| image:: media/image13.png
   :width: 2.29138in
   :height: 1.23943in
.. |image13| image:: media/image14.png
   :width: 1.82269in
   :height: 1.31234in
.. |image14| image:: media/image15.png
   :width: 5.68125in
   :height: 4.58081in
.. |image15| image:: media/image16.png
   :width: 2.32263in
   :height: 0.78115in
.. |image16| image:: media/image17.png
   :width: 2.72883in
   :height: 1.01029in
.. |image17| image:: media/image18.png
   :width: 6.50000in
   :height: 4.10486in
.. |image18| image:: media/image19.png
   :width: 5.93676in
   :height: 3.26001in
.. |image19| image:: media/image16.png
   :width: 2.32263in
   :height: 0.78115in
.. |image20| image:: media/image20.png
   :width: 3.36416in
   :height: 0.74991in
.. |image21| image:: media/image21.png
   :width: 5.73887in
   :height: 1.56230in
.. |image22| image:: media/image22.png
   :width: 6.50000in
   :height: 2.80417in
.. |image23| image:: media/image23.png
   :width: 3.04129in
   :height: 1.16652in
.. |image24| image:: media/image24.png
   :width: 2.34346in
   :height: 1.44774in
.. |image25| image:: media/image25.png
   :width: 1.12486in
   :height: 1.02071in
.. |image26| image:: media/image26.png
   :width: 6.50000in
   :height: 2.42917in
.. |image27| image:: media/image16.png
   :width: 2.32263in
   :height: 0.78115in
.. |image28| image:: media/image27.png
   :width: 3.43707in
   :height: 0.69783in
.. |image29| image:: media/image28.png
   :width: 6.50000in
   :height: 3.04375in
.. |image30| image:: media/image29.png
   :width: 3.18125in
   :height: 0.98529in
.. |image31| image:: media/image30.png
   :width: 6.50000in
   :height: 3.36181in
.. |image32| image:: media/image31.png
   :width: 2.91630in
   :height: 1.41649in
.. |image33| image:: media/image32.png
   :width: 2.27055in
   :height: 1.28109in
.. |image34| image:: media/image33.png
   :width: 6.49167in
   :height: 3.17500in
.. |image35| image:: media/image34.png
   :width: 1.82269in
   :height: 0.55201in
.. |image36| image:: media/image35.png
   :width: 6.50000in
   :height: 0.87847in
.. |image37| image:: media/image36.png
   :width: 3.09336in
   :height: 1.36441in
.. |image38| image:: media/image37.png
   :width: 6.50000in
   :height: 3.39792in
.. |image39| image:: media/image38.png
   :width: 0.95821in
   :height: 0.51035in
.. |image40| image:: media/image39.png
   :width: 1.99975in
   :height: 1.69770in
.. |image41| image:: media/image40.png
   :width: 7.36203in
   :height: 2.07222in
.. |image42| image:: media/image41.png
   :width: 4.57234in
   :height: 2.17681in
.. |image43| image:: media/image42.png
   :width: 6.50000in
   :height: 1.15972in
.. |image44| image:: media/image43.png
   :width: 6.50000in
   :height: 1.15625in
.. |image45| image:: media/image16.png
   :width: 2.32263in
   :height: 0.78115in
.. |image46| image:: media/image44.png
   :width: 2.47886in
   :height: 0.68741in
.. |image47| image:: media/image45.png
   :width: 3.92659in
   :height: 2.43719in
.. |image48| image:: media/image32.png
   :width: 2.27055in
   :height: 1.28109in
.. |image49| image:: media/image46.png
   :width: 3.18125in
   :height: 1.03772in
.. |image50| image:: media/image47.png
   :width: 6.68264in
   :height: 4.52778in
.. |image51| image:: media/image48.png
   :width: 1.72895in
   :height: 0.52077in
.. |image52| image:: media/image49.png
   :width: 6.50000in
   :height: 1.38194in
.. |image53| image:: media/image50.png
   :width: 6.50000in
   :height: 3.40764in
.. |image54| image:: media/image38.png
   :width: 0.95821in
   :height: 0.51035in
.. |image55| image:: media/image51.png
   :width: 3.59330in
   :height: 1.24984in
.. |image56| image:: media/image52.png
   :width: 4.60359in
   :height: 2.17681in
