LAB 2 SSL Certificate Management
================================

Goal:

The following labs will get you familiar with the BIG-IQ for managing
the local traffic SSL certificates for the BIG-IP devices from BIG-IQ.
From one centralized location, BIG-IQ makes it easy for you to request,
import, and manage CA-signed SSL certificates, as well as import signed
SSL certificates, keys, and PKCS #12 archive files created elsewhere.
And if you want to create a self-signed certificate on BIG-IQ for your
managed devices, you can do that too.

SSL certificates will come in two flavors, managed or un-managed. When
BIG-IQ discovers a BIG-IP, it is only able to pull the metadata about a
cert from the BIG-IP. This process completes the cert and key
information on the BIG-IQ, so that BIG-IQ can fully manage the
discovered certs.

Once you've imported or created an SSL certificate and keys, you can
assign them to your managed devices by associating them with a Local
Traffic Manager clientssl or serverssl profile, and deploying it.

Tasks:

1. Move a certificate from unmanaged to managed state.

2. Create and Import a self-signed certificates/key to BIG-IQ

3. Renew expired certificates and Deploy from BIG-IQ to managed BIG-IP

Note

When you discover a BIG-IP device, BIG-IQ Centralized Management imports
its SSL certificates' properties (metadata), but not the actual SSL
certificates and key pairs. These certificates display as Unmanaged on
the BIG-IQ Certificates & Keys screen. This allows you to monitor each
SSL certificate's expiration date from BIG-IQ, without having to log on
directly to the BIG-IP device.

Convert an unmanaged SSL key certificate and key pair to managed so you
can centrally manage it from BIG-IQ Centralized Management. This saves
you time because you don't have to log on to individual BIG-IP devices
to create, monitor, or deploy certificates.

Task 2.1: Move a certificate from unmanaged to managed state
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Objective
^^^^^^^^^

1. At the top of the screen, click Configuration.

2. On the left, click LOCAL TRAFFIC > Certificate
   Management > Certificates & Keys. These are the imported from BIG-IP
   but not yet managed by BIG-IQ.

    |image0|

1. Click the name of the unmanaged certificate **f5-ca-bundle**. You
   will need to import the certificate from the managed BIG-IP.

    |image1|

    Before you can import the certificate to BIG-IQ, you will need to
    export certificate/key from the managed BIG-IP device.

1. Log onto the TMUI interface of the **BOS-vBIG-IP01.temmarc.com**
   device to export the certificate/key pair.

    Click System ›› Certificate Management ›› Traffic Certificate
    Management ›› SSL Certificate List ›› f5-ca-bundle

|image2|

    Click Export and then click on **Download f5-ca-bundle.crt** button
    to save the certificate to a file to a location that you can easily
    locate.

|image3|

After saving the certificate file, click Cancel button to go back to
previous screen.

1. Now, circle back to the BIG-IQ TMUI and go back to the Certificate
   Properties \ **State** setting, click the \ **Import** button and
   then:

   -  To upload the certificate's file, select Upload File and click
      the Choose File button to navigate to the certificate file you
      saved before.

|image4|

Since this is a bundled certificate/key pair, you can skip the next
step.

1. For the Key Properties State setting, click the Import button and
   then:

   -  To upload the key's file, select Upload File and click the Choose
      File button to navigate to the key file.

2. Click the Save & Close button. Your certificate/key pair is now
   **Managed** by BIG-IQ.

|image5|

 Task 2.2: Create and Import a self-signed certificates/key to BIG-IQ
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. At the top of the screen, click Configuration.

On the left, click LOCAL TRAFFIC > Certificate Management > Certificates
& Keys.

On BOS-vBIG-IP01, create a new self-signed certificate.

Name: BIG-IQ-Test-Cert

Common Name: bigiq.testcrt.com

|image6|

|image7|

***Click on Export, and download the BIG-IQ-Test-Cert.crt file.***

|image8|

***When importing the key, select “Normal” for Key Security Type:***

|image9|

***BigIQ shows the cert/key being active (green status) and managed ***

|image10|

Task 2.3: Renew expired certificates and Deploy from BIG-IQ to managed BIG-IP
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 
^

Renewed the expired cert on BIG-IQ

Push the new cert to SEA-vBIGIP01

Objective
^^^^^^^^^

Walk through deployment of Certificate changes to the BIG-IP devices.

-  Navigate to ConfigurationLocal TrafficCertificate Management
   Certificate & Keys

-  Renew expired certificate: app01.termmarc.com on SEA-vBIGIP01 device.

|image11|

Move the mouse over to the yellow triangle in front of the Managed State
of the certificate app01.termmarc.com.

|image12|

Click on the app01.termmarc.com certificate, and click on the **Renew
Certificate** button on the right upper corner.

|image13|

Accept the default Duration 365 days, and click on Renew button on the
right lower corner.

|image14|

The certificate is now renewed to 365 days of Duration.

1. At the top of the screen, click Deployment.

On the left, click EVALUATE & DEPLOY > Local Traffic & Network, then
click on Create button under Evaluations.

Enter a task name “\ **Deploy-Renewed-Cert**\ ”, in Evaluation section,
select Partial Changes next to Source Scope line item, the list of
Source Objects appears. Select “Certificate” from the dropdown list on
the left Available section, and move the certificate app01.termmarc.com
to the right side Selected box.

|image15|

Scroll down the screen and click on “Find Relevant Devices”, you will
see SEA-vBIGIP01 listed in the Available section on the left. Select and
move the device to the right side Selected box, and click on Create
button. The evaluation starts, and you will see the Status marked as
Evaluation Complete when it is done.

|image16|

Click on the View link in the middle of the screen under Differences
column.

|image17|

Review the differences between the BIG-IQ certificate and the BIG-IP
certificate for app01.termmarc.com.

|image18|

Cancel the window to return to the previous screen, select the
evaluation “Deploy-Renewed-Cert” and click on Deploy button above. Click
on Deploy again to confirm.

|image19|

Now the deployment is completed.

|image20|

Log back into SEA-vBIGIP01 device, navigate back to System ››
Certificate Management : Traffic Certificate Management : SSL
Certificate List. Verify that the certificate app01.termmarc.com has
been renewed to 365 days duration by BIG-IQ.

|image21|

.. |image0| image:: media/image1.png
   :width: 6.50000in
   :height: 1.82917in
.. |image1| image:: media/image2.png
   :width: 6.49583in
   :height: 3.38750in
.. |image2| image:: media/image3.png
   :width: 6.49583in
   :height: 4.02083in
.. |image3| image:: media/image4.png
   :width: 6.49583in
   :height: 3.14583in
.. |image4| image:: media/image5.png
   :width: 6.49167in
   :height: 3.06250in
.. |image5| image:: media/image6.png
   :width: 6.49167in
   :height: 1.82083in
.. |image6| image:: media/image7.png
   :width: 6.49167in
   :height: 4.52083in
.. |image7| image:: media/image8.png
   :width: 6.49167in
   :height: 4.46250in
.. |image8| image:: media/image9.png
   :width: 6.49583in
   :height: 2.90833in
.. |image9| image:: media/image10.png
   :width: 6.49583in
   :height: 3.39167in
.. |image10| image:: media/image11.png
   :width: 6.48750in
   :height: 1.76250in
.. |image11| image:: media/image12.png
   :width: 6.49167in
   :height: 2.13750in
.. |image12| image:: media/image13.png
   :width: 6.49167in
   :height: 1.34167in
.. |image13| image:: media/image14.png
   :width: 6.50000in
   :height: 3.06597in
.. |image14| image:: media/image15.png
   :width: 6.50000in
   :height: 3.12083in
.. |image15| image:: media/image16.png
   :width: 6.50000in
   :height: 3.65625in
.. |image16| image:: media/image17.png
   :width: 6.50000in
   :height: 3.65625in
.. |image17| image:: media/image18.png
   :width: 6.49583in
   :height: 1.47500in
.. |image18| image:: media/image19.png
   :width: 6.48750in
   :height: 3.31250in
.. |image19| image:: media/image20.png
   :width: 6.48750in
   :height: 3.09583in
.. |image20| image:: media/image21.png
   :width: 6.49167in
   :height: 2.74167in
.. |image21| image:: media/image22.png
   :width: 6.50000in
   :height: 3.65625in
