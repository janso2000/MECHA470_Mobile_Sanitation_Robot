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
   <h4> MECA 470 Introduction to Robotics</h4> 
   <h4> Mobile Sanitation Robot</h4> 
</center>

#### Table of Contents
- [1. Introduction](#1-Introduction)
- [2. Mobile Robot Degrees of Freedom](#2-Mobile-Robot-Degrees-of-Freedom)
- [3. Mapping and Path Planning with ROS](#3-Mapping-and-Path-Planning-with-ROS) 
- [4. Simulation](#4-Simulation)
- [5. Appendix](#5-Appendix)
- [6. References](#6-References)

## 1. Introduction 
What is this project about and what are we doing 

## 2. Mobile Robot Degrees of Freedom
Insert photo of calcs for the mobile robot - 3 DOF 

## 3. Mapping and Path Planning with ROS 

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
  
    function sysCall_init()
            left_wheel=sim.getObjectHandle('Magni_LeftMotor')
            right_wheel=sim.getObjectHandle('Magni_RightMotor')
    xml = [[
    <ui title="Speed Control" closeable="true" resizable="false" activate="false">
    <group layout="form" flat="true">
        <label text="Left Wheel (rad/s): 0.00" id="1"/>
        <hslider tick-position="above" tick-interval="1" minimum="-10" maximum="10" on-change="actuateLeft" id="2"/>
        <label text="Right Wheel (rad/s): 0.00" id="3"/>
        <hslider tick-position="above" tick-interval="1" minimum="-10" maximum="10" on-change="actuateRight" id="4"/>
    </group>
    <label text="" style="* {margin-left: 400px;}"/>
      </ui>
      ]]
        ui=simUI.create(xml)
    end
    
    function actuateLeft(ui,id,newVal)
            local val = 0.5*newVal
            sim.setJointTargetVelocity(left_wheel,val)
            simUI.setLabelText(ui,1,string.format("Left Wheel (rad/s): %.2f",val))
      end

     function actuateRight(ui,id,newVal)
            local val = 0.5*newVal
            sim.setJointTargetVelocity(right_wheel,val)
            simUI.setLabelText(ui,3,string.format("Right Wheel (rad/s): %.2f",val))
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
