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
  <img src = "Images/MagniinCoppelia.png" height = "320px" style="margin:10px 10px">
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
- [4. Coppelia Simulation](#4-Coppelia Simulation)
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
  


<p align = "center">
<iframe src="https://drive.google.com/file/d/1dyAuiPnWZ_z191VjUVspRuHX62vPLFmA/view?usp=sharing" width="640" height="480"></iframe>
</p>

     


  <p align = "center">
  <img src = "photos/Masons%20Rule2.png" height = "100px" style="margin:7px 7px">
</p>
  The open loop transfer function of the entire treadmill system, including all of the damping coefficients of each component were modeled into one system above, which is seemingly unsolvable at this point. This equation is not currently being used in this project, but it is included to provide context as to how complicated this problem can become if left unsimplified.
  
  --------------------------------------------------------------------------------------------------------------
  
 Here is a summary of the simulation results with the requirements as shown:
  <p align = "center">
  <img src = "photos/Output%20Values.png" height = "260px" style="margin:10px 10px">
</p>


## 4. Coppelia Simulation 

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


## 5. Appendix

Degree of Freedom Calculation: 
https://modernrobotics.northwestern.edu/nu-gm-book-resource/2-2-degrees-of-freedom-of-a-robot/


## 6. References

Lynch, K., &amp; Park, F. C. (2019). Modern robotics: Mechanics, planning, and control. Cambridge, United Kingdom: Cambridge University Press.


<a href="https://github.com/janso2000/MECHA470_Mobile_Sanitation_Robot"> Click here to go to our project repository </a>
