.. _E_Troubleshooting-and-temporary-procedures:

****************************************
Troubleshooting and temporary procedures
****************************************

The following sections list the solutions to the most common problems 
encountered by users, including indications on how to deal with present bugs 
or inefficiencies in the code. The records are sorted according to the main 
operation areas in which the problems arise. 

.. note:: **Users are advised to read these “hints” at the beginning of the 
   session and before attempting (possibly unnecessary) heavy-handed operations 
   on the system.** 


Setup
=====


.. admonition:: PROBLEM: 

   * **The Scheduler panel shows a FAILURE flag.**

Due to a system bug, such flag **might** not indicate an actual problem. 
Usually, when the FAILURE status is fake, the launch of a new schedule resets 
it. If you are already running a schedule, stop it and re-launch it from the 
scan_subscan that was next due. 

If you are performing the initial setup, get to the point of launching a 
schedule and check whether the flag resets to OK. 
Pay then attention to the fact that new acquisitions are 
correctly recorded (i.e. verify the presence of new FITS files, using the
quick-look tools or checking the updates in the output data folder).  
If the FAILURE status persists and observations do not proceed correctly, call
for assistance.

 
.. admonition:: PROBLEM: 

   * **The initial setup fails, but it is difficult to assess what goes 
     wrong.**

Instead of using the overall setup commands, give in the *operatorInput* the 
**individual commands** which deal with the different sub-systems, so that it 
is easier to identify the misbehaving element.

For example, the setupKKC command can be substituted by (the actual 
receiversMode code to be used depends on the desired setup):: 

    > antennaReset
    > antennaSetup=KKC    
    > servoSetup=KKC     
    > receiversSetup=KKC
    > initialize=KKC    
    > device=0
    > calOff

For other receivers, the codes of course vary. 

Schedules
=========

.. admonition:: PROBLEM:  

    * **I've performed cross-scans in order to fine-tune the pointing, but
      the measured offsets are not actually applied to the following 
      acquisitions**
    
System offsets, such as the ones measured with a Point acquisition, sum up to 
the ones indicated inside schedules ONLY if they are expressed in the same 
coordinate frame. This means that, if you perform observations using EQ offsets, 
also the fine-pointing cross-scans must be carried out in the EQ frame. The 
same holds for HOR scans. If there is a frame mismatch, the system offsets are 
automatically rejected (bug under fixing).

.. admonition:: PROBLEM:
  
    * **I’ve stopped an XARCOS schedule and no further operation is accepted 
      by the system.**
      
**Always** stop your schedules with::

    > haltSchedule

when you are using XARCOS; otherwise it will keep a busy status and the next 
operations will become impossible to perform. 


.. admonition:: PROBLEM:  

    * **I’ve launched a surely correct schedule, but parts of the system 
      crashed.**

Pay attention to the syntax, you might have inserted unwanted characters like 
an extra “ / ”: the *startSchedule* command must be either given as::

    > startSchedule=[project]/[schedname].scd,[N]

or, if you have already given the project=code command:: 

    > startSchedule=[schedname].scd,[N]


.. admonition:: PROBLEM:  

    * **Dead time between consecutive subscans is much longer than expected**

With ESCS 0.4 it is necessary to delete from the schedules all the post-scan 
wait times, as the system now takes automatically care of computing and 
applying proper delays according to the deceleration ramps duration. 
If you specify a wait time in your post-scan procedures, it will add to the 
system-computed delays. 

