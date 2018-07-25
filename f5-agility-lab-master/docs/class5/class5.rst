Class 5 - Partial Deployment & Roll Back
=========================================

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

.. toctree::
   :maxdepth: 1
   :glob:

   module*/module*