# MECHA470_Mobile_Sanitation_Robot
### MECHA 470 Robotics Project

-------------------------------------------------------------------------------------

Mobile Sanitation Robot

<p align = "center">
Project Members:
TaylorAnne Brown - 
Maryam Nassar -
Juan Ruiz -
Daniel Sousa 
      </p> 

-------------------------------------------------------------------------------------


<p align = "center">
  <img src = "photos/HeaderPhoto.PNG" height = "320px" style="margin:10px 10px">
</p>



<center>
   <h4> California State University Chico</h4>
   <h4> College of Engineering, Computer Science, and Construction Management</h4> 
   <h4> MECA 570 Introduction to Robotics</h4> 
   <h4> Mobile Sanitation Robot</h4> 
</center>

#### Table of Contents
- [1. Introduction](#1-Introduction)
- [2. Control Theory Modeling](#2-Control-Theory-Modeling)
- [3. Dynamic Modeling](#3-Dynamic-Modeling) 
- [4. Design and Simulation](#4-Design-and-Simulation)
- [5. Appendix](#5-Appendix)
- [6. References](#6-References)

## 1. Introduction 
The actively damped treadmill system consists of a treadmill and suspension system which reduces the peak reaction force felt by an object impacting the treadmill. The control is achieved by tuning the damping and spring coefficients in the suspension system, taking into account the spring and damping coefficients of the frame itself. The Project Team’s goal is to develop a comprehensive solution (explained in more details in the deliverables section) to reduce the impact for any weight within the limits.

## 2. Control Theory Modeling
Sample High-Level Architecture:

The project is most completely represented as a series of mass-spring-damper systems stacked upon each other, as illustrated here:
<p align = "center">
  <img src = "photos/Treadmill%20System%20ModelResize.png" height = "260px" style="margin:10px 10px">
</p>
This diagram illustrates the spring and damping properties of the runner's body (K_f, D_f) as they interact with the spring and damping properties of the treadmil plate (K_p, D_p). The plate in turn acts upon the frame, which has its own spring constant (K_frame), and the whole system is supported by the active damping system (K_s, D_s).

## 3. Dynamic Modeling 

In order to better approach the problem, it is necessary to simplify the complete model into a single mass-spring-damper system.
<p align = "center">
  <img src = "photos/MKD_fbd.PNG" "width="504" height="351" style="margin:10px 10px">
</p>

Here, the dynamic properties of the runner are reduced to a single input (F_t), and the dynamic properties of the plate, the frame, and the active damping system are merged into a single mass-spring-damper representation. The resulting equation of motion is given below.                                                                        
 <p align = "center">
  <img src = "photos/MKD_eqn_ft.PNG" width="301" height="71" style="margin:10px 10px">
</p>    

<!--
<p align = "center">
// <img src = "photos/MKD_eqn_u.PNG" width="281" height="72" style="margin:10px 10px">
</p>
-->
By applying LaPlace transformations, we can obtain a system transfer function in s-domain.
<p align = "center">
  <img src = "photos/MKD_eqn_Gs.PNG" height = "width="679" height="165" style="margin:10px 10px">
</p>
This transfer function allows us to model the system in Simulink, and enables us to better visualize the system response.
<p align = "center">
  <img src = "photos/MKD_Smlnk_Mdl.PNG" "width="721" height="381" style="margin:10px 10px">
</p>


## 4. Design and Simulation

System simulation was done using Coppelia-Sim. Treadmill spring dampeners were siumulated using prismaic joints with an accompanying 
Lua script in order to simulate a realistic spring with a spring constant value of 566,440 N/M reacting to a downward force of 25 kg
The resulting Coppelia sumulation is shown below

<p align = "center">
<iframe src="https://drive.google.com/file/d/1JGDH5E4Qt0_5jSQPU98jLqbb_XijHtdo/preview" width="640" height="480"></iframe>
</p>

        *** function sysCall_threadmain()
            sim.setThreadAutomaticSwitch(false)
            cyhandle=sim.getObjectHandle('Cylinder')
            cyhandle0=sim.getObjectHandle('Cylinder1')
            initPosition=sim.getObjectPosition(cyhandle,cyhandle0)
            k=566440
            while sim.getSimulationState()~=sim.simulation_advancing_abouttostop do
            tempPosition=sim.getObjectPosition(cyhandle,cyhandle0)
            distance=tempPosition[3]-initPosition[3]
            print(distance)
            lastforce=distance*k
           sim.addForceAndTorque(cyhandle0,{0,0,distance*k},{0,0,0})
           sim.switchThread() -- resume in next simulation step
          end
      end
      
In addition we also simulated a mass-spring system using a Visual Python extension. This allowed for a more accurate representation of the model's spring system. The same values were used to represent with spring with the constant at 566,440 N/M and a downward forcer of 25 kg. In addition to this we were able to model the spring radius, number of coils, and thickness. These values are 1.25, 10, and .625 respectively. Below is the Visual Python simulation of the spring system. 

<p align = "center">
<iframe src="https://drive.google.com/file/d/1UyK8NhcIz9nqFU8-gtub95kVcnHaFl65/preview" width="640" height="480"></iframe>
</p>

        *** from vpython import *
            #GlowScript 2.9 VPython
             display(width=700,height=700,center=vector(7,0,0),background=color.white)
             wall=box(pos=vector(0,1,0),size=vector(0.2,12,12),color=color.white)
             floor=box(pos=vector(6,-1.75,-1),size=vector(18,0.2,10),color=color.white)
             Mass=box(pos=vector(12,0,0),velocity=vector(0,0,0),size=vector(1,1,1),mass=10.0,color=color.green)
              pivot=vector(0,0,0)
              spring=helix(pos=pivot,axis=Mass.pos-pivot,radius=1.25,constant=566440,thickness=0.625,coils=10,color=color.orange)
              eq=vector(9,0,0)
              #spring constant is in units of N/M
              #spring and block size values are in units of mm
              t=0
              dt=0.001
              while (t<50):
              rate(100)
              acc=(eq-Mass.pos)*(spring.constant/Mass.mass)
              Mass.velocity=Mass.velocity+acc*dt
              Mass.pos=Mass.pos+Mass.velocity*dt
              spring.axis=Mass.pos-spring.pos
              t=t+dt
              


  <p align = "center">
  <img src = "photos/Masons%20Rule2.png" height = "100px" style="margin:7px 7px">
</p>
  The open loop transfer function of the entire treadmill system, including all of the damping coefficients of each component were modeled into one system above, which is seemingly unsolvable at this point. This equation is not currently being used in this project, but it is included to provide context as to how complicated this problem can become if left unsimplified.
  
  --------------------------------------------------------------------------------------------------------------
  
 Here is a summary of the simulation results with the requirements as shown:
  <p align = "center">
  <img src = "photos/Output%20Values.png" height = "260px" style="margin:10px 10px">
</p>


              

## 5. Appendix

Basis for shoe damping estimates:
https://www.google.com/url?q=https://www.researchgate.net/publication/316526467_The_Effects_of_shoe_stiffness_and_damping_on_the_Gastrocnemius_vibration_during_walking&sa=D&ust=1589745572663000&usg=AFQjCNEl91jeMZW9CHBB52Nol5StPYOswQ

Basis for frame damping estimate:
https://www.google.com/url?q=https://iopscience.iop.org/article/10.1088/1742-6596/268/1/012022/pdf&sa=D&ust=1589745572662000&usg=AFQjCNGAvX2rAkUiJHnXtbrUmGIrKLEIRA

## 6. References

Kenneth P. Clark, Laurence J. Ryan, Peter G. Weyand
Journal of Experimental Biology 2017 220: 247-258; doi: 10.1242/jeb.138057
https://jeb.biologists.org/content/220/2/247

Surface effects on ground reaction forces and lower extremity kinematics in running
DIXON, SHARON J.; COLLOP, ANDREW C.; BATT, MARK E.
https://journals.lww.com/acsm-msse/Fulltext/2000/11000/Surface_effects_on_ground_reaction_forces_and.16.aspx


<a href="https://github.com/MECA-482-Project/Main"> Click here to go to our project repository </a>
