LAB 8 Application Templates & Deployment
========================================

**BIG-IP Cloud Edition:**

BIG-IP Cloud Edition provides traffic management and security
application services on a per-application basis using BIG-IP LTM and
Advanced WAF, respectively, to isolate workloads and for better
manageability.

In addition, you can auto-scale application services based on predefined
thresholds. Application auto-scaling uses the **Service Scaling Group
(SSG)** feature, which enables administrators to configure the BIG-IQ
system to scale-out or scale-in BIG-IP virtual edition instances based
on scaling rules, such as CPU and memory use or throughput.

To further enhance manageability, BIG-IP Cloud Edition enables network
and security operations teams to provide application teams with
self-service deployment of application services in public and private
clouds.

BIG-IP Cloud Edition is a solution package comprised of the BIG-IQ
system, BIG-IP Per-App Virtual Edition (VE), and other supporting
components.

A service template is a collection of objects that you define. When you
create an application from the template, you can omit or include these
objects. Once you specify the objects and settings you need, you save
the application and it creates those objects on the device you targeted
it to, or on the devices in the SSG you identified.

Note: You can currently auto-scale per-app virtual edition instances on
Amazon Web Services (AWS) and VMWare.

**Goal:**

In this lab, we will cover the creation of Application Template and
deployment of Applications. For Auto-scaling of applications via SSG,
please refer to addition documentation.

To create an application using the BIG-IQ user interface you choose a
service template that defines the objects in that application and then
decide whether you want to deploy that application to a service scaling
group (SSG) or to a managed device.

Tasks:

8.1: Create personas in BIG-IQ for Application Deployment

8.2: Application Templates & Deployment

8.3: Application Deployment via API

Task 8.1: Create personas in BIG-IQ for Application Deployment
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

We will be using 4 main personas for this lab:

1. **Marco**: Full Administrator

2. **David**: Super-NetOps

3. **Larry**: Application Security Manager

4. **Paula**: Application Manager

**Marco** will have full access to BIG-IQ. He knows a lot about F5
products (BIG-IQ/BIG-IP). He will provide the access to David, Larry and
Paula. He will also manage the Service Scaline Group (SSG) and
application templates.

**Larry** will manage the Web Application Firewall (WAF) policies. He
will work with Paula’s team to define the necessary security policies
for each applications. Ensure teams comply with security policies,
industry rules and regulations, and best practices. Keeping up to date
on threats, determining their potential impact, and mitigating the
risks.

**Paula** will manage the application deployments, monitor levels of app
incidents, building solutions to address identified, prioritized
business problems in a timely manner. Maximizing value of app through
capabilities design, adoption, and usage. Ensuring that the app fits
within the rest of the organization’s app portfolio strategy.

**David** will try automating whenever possible, to enable efficiency
and ability to solve problems at scale. Automate common network patterns
that the other teams can consume. Automate existing environment
management and troubleshooting tasks.

Note

    Marco, Paula and Larry are already created in the blueprint,so only
    the \ **david** user needs to be created.

Connect to your BIG-IQ as \ **admin** and go to : *System* > *Users
Management* > *Users* and verify each user & role below and change where
needed.

**1. Marco: Full Administrator**

-  *Auth Provider* = Radius

-  *User Name* = marco

-  *Full Name* = Full Administrator

-  (*Password stored in Radius server* = marco)

-  *Role* = Administrator Role

**2. Larry: Application Security Manager**

-  *Auth Provider* = Radius

-  *User Name* = larry

-  *Full Name* = Application Security Manager

-  (*Password stored in Radius server* = larry)

-  *Role* = Security Manager

**3. Paula: Application Manager**

-  *Auth Provider* = Radius

-  *User Name* = paula

-  *Full Name* = Application Manager

-  (*Password stored in Radius server* = paula)

-  *Role* = Application Editor

**4. David: Super-NetOps**

Click on \ *Add*

-  *Auth Provider* = local

-  *User Name* = david

-  *Full Name* = Super-NetOps

-  *Password* = david

-  *Role* = Administrator Role

Click
on \ `\* <http://f5-ce-lab.readthedocs.io/en/master/class1/module1/lab1.html#id1>`__\ Save
& Close\*\`

|image0|

Task 8.2: Application Templates & Deployment
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

In this module, we will learn how to use Application Templates and how
to deploy an \ **Application**.

The Application Templates will be created by \ **marco**, the
Administrator. \ **larry** will create the security policies and let
Marco know about the ones to associate with the templates. Once the
template is ready with all the necessary information, it will be ready
to use by the Application owner.

**paula** needs to deploy an application, she has multiple Application
servers. At this time, she needs to test the performance of her
application, she also wants to make her application secure before
staging it to production. She connects to the BIG-IQ and has access to
her Application Dashboard. \ **paula**\ uses the application template
created by Marco to deploy her Application.

After a week of testing her application (in the class ~5 min), she will
ask \ **larry** to fine tune and validate the learning done by the
Application Firewall (BIG-IP ASM).

Note

    A traffic generator located on the \ *Ubuntu Lamp Server, LDAP and
    DHCP* server, is sending good traffic every minute to the virtual
    servers.

Once the security policy is tuned and validated, \ **paula** will
enforce blocking mode in the policy.

Finally, we will simulate “bad” traffic to show the security policy
blocking it.

Note

    A traffic generator located on the \ *Ubuntu Lamp Server, LDAP and
    DHCP* server, can be launched manually to send bad traffic to the
    virtual servers.

Built-in templates

BIG-IQ v6.0 will have the default templates below built-in. These
default templates cannot be modified but they can be cloned. They can be
used to deploy various type of applications. These default templates are
only displayed after BIG-IQ is managing a BIG-IP device.

-  Default-AWS-f5-fastHTTP-lb-template: For load balancing an HTTP-based
   application, speeding up connections and reducing the number of
   connections to the back-end server. (only for AWS)

-  Default-AWS-f5-HTTPS-WAF-lb-template: For load balancing an HTTPS
   application on port 443 with a Web Application Firewall using an ASM
   Rapid Deployment policy. (only for AWS)

-  Default-f5-FastL4-TCP-lb-template: For load balancing a TCP-based
   application with a FastL4 profile.

-  Default-f5-FastL4-UDP-lb-template: For load balancing a UDP-based
   application with a FastL4 profile.

-  Default-f5-HTTP-lb-template: For load balancing an HTTP application
   on port 80.

-  Default-f5-fastHTTP-lb-template: For load balancing an HTTP-based
   application, speeding up connections and reducing the number of
   connections to the back-end server.

-  Default-f5-HTTPS-WAF-lb-template: For load balancing an HTTPS
   application on port 443 with a Web Application Firewall using an ASM
   Rapid Deployment policy.

-  Default-f5-HTTPS-offload-lb-template: For load balancing an HTTPS
   application on port 443 with SSL offloading on BIG-IP.

Warning

    Templates with virtual servers without a HTTP profiles can not be
    depoyed to a Service Scaling Group

Connect as \ **marco**, go to \ *Applications* > *SERVICE CATALOG*:

Look through the different default templates.

|image1|

**Create custom security policies & Application Service Template**

Connect as \ **larry**

1. Create the custom ASM policy, go
   to \ *Configuration* > *SECURITY* > *Web Application
   Security* > *policies*.

|image2|

Select the viol\_subviol ASM policy from the list and look through its
settings. Notice the policy is in Transparent mode.

Edit the Policy viol\_subviol, switch to Manual Learning Mode
and Make available in Application Templates, click Save.

|image3|

In addition, go to \ *POLICY BUILDING* > *Settings* and set \ *Policy
Building Mode* to Central and switch to Manual Learning Mode, click Save
& Close.

|image4|

Note

    If you want to use learning/blocking mode, you will need a dedicated
    app template per application.

2. Create the AFM Policy, go
to \ *Configuration* > *SECURITY* > *Network Security* > *Firewall
Policies*, click Create. Then enter the name of your
policy: f5-afm-policy1. Make sure the
box Make available in Application Templates is checked. Click Save.

|image5|

Create 2 Rules:

-  rule 1: set the destination ports to 443 and 80, Protocol to tcp

-  rule 2: set action to reject and log to true

Click Save & Close.

|image6|

Connect as \ **marco**

1. Create a Clone of the \ *Default-f5-HTTPS-WAF-lb-template* policy, go
to \ *Applications* > *SERVICE CATALOG*, and click on \ *Clone*. Enter
the name of your cloned template: f5-HTTPS-WAF-lb-template-custom1

`
 <http://f5-ce-lab.readthedocs.io/en/master/_images/img_module2_lab2_4b.png>`__\ |image7|

2. Then select the ASM policy viol\_subviol, the AFM
   policy f5-afm-policy1 and the Logging Profile templates-default in
   the SECURITY POLICIES section on both Virtual Servers (Standalone
   Device).

    Save & Close

|image8|

**8.2.3: Create Application**

Connect as \ **paula** to create a new application, and click
on \ *Create*, select the template previously
created f5-HTTPS-WAF-lb-template-custom1.

Type in a Name for the application you are creating.

-  Application Name: site18.example.com

To help identify this application when you want to use it later, in the
Description field, type in a brief description for the application you
are creating.

-  Description: My First Application on F5 Cloud Edition

Type the domain of your application (then the ASM policy will always be
transparent for this domain)

-  Domain Names: site18.example.com

For Device, select the name of the device you want to deploy this
application to. (if the HTTP statistics are not enabled, they can be
enabled later on after the application is deployed)

-  BIG-IP: Select SEA-vBIGIP01.termmarc.com and
   check Collect HTTP Statistics

|image9|

Determine the objects that you want to deploy in this application. To
omit any of the objects defined in this template, click the (X) icon
that corresponds to that object. To create additional copies of any of
the objects defined in this template, click the (+) icon that
corresponds to that object.

In the example, fill out the Server’s IP addresses/ports (nodes) and
virtual servers names, IPs and ports.

-  Servers (Pool Member): 10.1.20.118 and 10.1.20.119

-  Service Port: 80

-  Name WAF & LB (Virtual Server): vs\_site18.example.com\_https

-  Destination Address: 10.1.10.118

-  Destination Network Mask: /32

-  Service Port: 443

-  Name HTTP Redirect (Virtual Server): vs\_site18.example.com\_redirect

-  Destination Address: 10.1.10.118

-  Destination Network Mask: /32

-  Service Port: 80

It is good practice to type the Prefix that you want the system to use
to make certain that all of the objects created when you deploy an
application are uniquely named.

|image10|

Then Click on Create (bottom right of the window). The Application is
deployed.

|image11|

Note

    In case the Application fails, connect as \ **Marco** and go to
    Applications > Application Deployments to have more details on the
    failure. You try retry in case of failure.

Note

    You can tail the logs: /var/log/restjavad.0.log

In Paula’s Dashboard, she can see her Application.

|image12|

Click on the Application and check the details (alarms, security
enabled, configuration, …)

|image13|

Click on Traffic Management > Configuration

|image14|

Paula can update Application Health Alert Rules by clicking on the
Health Icon on the top left of the Application Dashboard.

|image15|

|image16|

**8.2.4: Security workflows**

Connect as \ **larry**

1. Larry check the Firewall policy.

Go to Monitoring > REPORTS > Security > Network Security > Rule
statistics and
select \ *vs\_site18.example.com\_https* SEA-vBIGIP01.termmarc.com

|image17|

2. Larry check the Web Application Security for viol\_subviol ASM
   Policy.

Go to Configuration > SECURITY > Web Application Security > Policies

Click on Suggestions, then Accept the Learning.

|image18|

3. Go to Deployment > EVALUATE & DEPLOY > Web Application Security

Under Deployments, click on \ **Create**. Name your Deployment, select
SEA-vBIGIP01.termmarc.com, choose method \ **Deplot immediatly**, then
click on \ **Create**.

|image19|

4. Go back to Configuration > SECURITY > Web Application Security >
   Policies

Update the Enforcement Mode to Blocking.

|image20|

Connect as \ **paula**

Select site18.example.com

1. Paula enforce the policy APPLICATION SERVICES > Security >
   CONFIGURATION tab > click on Start Blocking

|image21|

2. Let’s generate some bad traffic, connect on the \ *Ubuntu Lamp
   Server* server and launch the following script:

# /home/f5/scripts/generate\_bad\_traffic.sh

3. In Application Dashboard, navigate to the Security Statistics and
   notice the Malicious Transactions.

Connect as \ **larry**

1. Check ASM type of attacks

Monitoring > EVENTS > Web Application Security > Event Logs > Events

|image22|

.. |image0| image:: media/image1.png
   :width: 6.50000in
   :height: 3.56291in
.. |image1| image:: media/image2.png
   :width: 6.50000in
   :height: 2.36611in
.. |image2| image:: media/image3.png
   :width: 6.50000in
   :height: 3.24322in
.. |image3| image:: media/image4.png
   :width: 6.50000in
   :height: 4.68704in
.. |image4| image:: media/image5.png
   :width: 6.50000in
   :height: 4.49151in
.. |image5| image:: media/image6.png
   :width: 6.50000in
   :height: 2.94218in
.. |image6| image:: media/image7.png
   :width: 6.50000in
   :height: 2.19608in
.. |image7| image:: media/image8.png
   :width: 6.50000in
   :height: 2.80884in
.. |image8| image:: media/image9.png
   :width: 6.50000in
   :height: 3.85489in
.. |image9| image:: media/image10.png
   :width: 6.50000in
   :height: 3.86486in
.. |image10| image:: media/image11.png
   :width: 6.50000in
   :height: 3.85870in
.. |image11| image:: media/image12.png
   :width: 6.50000in
   :height: 3.33198in
.. |image12| image:: media/image13.png
   :width: 6.50000in
   :height: 3.22292in
.. |image13| image:: media/image14.png
   :width: 6.50000in
   :height: 3.19947in
.. |image14| image:: media/image15.png
   :width: 6.50000in
   :height: 3.33004in
.. |image15| image:: media/image16.png
   :width: 6.50000in
   :height: 4.78448in
.. |image16| image:: media/image17.png
   :width: 6.50000in
   :height: 5.01914in
.. |image17| image:: media/image18.png
   :width: 6.50000in
   :height: 2.98245in
.. |image18| image:: media/image19.png
   :width: 6.50000in
   :height: 4.02289in
.. |image19| image:: media/image20.png
   :width: 6.50000in
   :height: 3.21818in
.. |image20| image:: media/image21.png
   :width: 6.50000in
   :height: 7.08950in
.. |image21| image:: media/image22.png
   :width: 6.50000in
   :height: 3.48878in
.. |image22| image:: media/image23.png
   :width: 6.50000in
   :height: 3.62230in
