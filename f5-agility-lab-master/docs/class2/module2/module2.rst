Module 2 – Review BIG-IP cluster status and configurations
=============================================

Task 2.2: Create and Import a self-signed certificates/key to BIG-IQ
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. At the top of the screen, click Configuration.

On the left, click LOCAL TRAFFIC > Certificate Management > Certificates
& Keys.

On BOS-vBIG-IP01, create a new self-signed certificate.

Name: BIG-IQ-Test-Cert

Common Name: bigiq.testcrt.com

.. image:: image/image7.png

.. image:: image/image8.png

***Click on Export, and download the BIG-IQ-Test-Cert.crt file.***

.. image:: image/image9.png

***When importing the key, select “Normal” for Key Security Type:***

.. image:: image/image10.png

***BigIQ shows the cert/key being active (green status) and managed ***

.. image:: image/image11.png


.. |image7| image:: media/image7.png
   :width: 6.49167in
   :height: 4.52083in
.. |image8| image:: media/image8.png
   :width: 6.49167in
   :height: 4.46250in
.. |image9| image:: media/image9.png
   :width: 6.49583in
   :height: 2.90833in
.. |image10| image:: media/image10.png
   :width: 6.49583in
   :height: 3.39167in
.. |image11| image:: media/image11.png
   :width: 6.48750in
   :height: 1.76250in