# Robot Morphology

In robotics, **robot morphology** refers to the physical structure of a robot and the arrangement of its joints. This configuration determines its **degrees of freedom**, the shape of its **workspace**, its type of motion, and its applications.

A common way to classify industrial manipulators is by the **sequence of joint types** that compose them:

- **P (Prismatic):** linear motion  
- **R (Rotational or Revolute):** angular motion  

Based on this classification, the following main robot morphologies are identified.

![TEXT](recursos/imgs/act1/a1%20(7).png){ width="600" }

---

### Cartesian Robots (PPP)

They are composed of **three prismatic joints**, allowing linear motion along the X, Y, and Z axes. Their workspace has a rectangular shape, and their kinematic control is simple.

**Typical applications:** 3D printers, CNC machines, linear positioning systems.

![TEXT](recursos/imgs/act1/a1%20(23).png){ width="350" }
![TEXT](recursos/imgs/act1/a1%20(11).png){ width="350" }
---

### SCARA Robots (RRP)

They consist of **two rotational joints and one prismatic joint**. They are compliant in the horizontal plane and rigid along the vertical axis, which enables high speed and precision.

**Typical applications:** high-speed assembly, electronics industry, automated production lines.

![TEXT](recursos/imgs/act1/a1%20(16).png){ width="350" }
![TEXT](recursos/imgs/act1/a1%20(10).png){ width="350" }
---

### Articulated Robots (RRR)

They are composed of **three rotational joints**, providing a high degree of motion flexibility and a geometry similar to the human arm.

**Typical applications:** welding, painting, industrial assembly, general manipulation.

![TEXT](recursos/imgs/act1/a1%20(6).png){ width="250" }
![TEXT](recursos/imgs/act1/a1%20(8).png){ width="250" }
![TEXT](recursos/imgs/act1/a1%20(12).png){ width="250" }
---

### Spherical Robots (RRP)

They combine **two rotational joints and one prismatic joint**, generating a spherical or semi-spherical workspace.

**Typical applications:** part manipulation, machine loading and unloading.

![TEXT](recursos/imgs/act1/a1%20(2).png){ width="350" }
![TEXT](recursos/imgs/act1/a1%20(24).png){ width="350" }
---

### Cylindrical Robots (RPP)

They feature **one rotational joint and two prismatic joints**. Their workspace is cylindrical, making them suitable for vertical and radial movements.

**Typical applications:** material handling, vertical assembly processes.

![TEXT](recursos/imgs/act1/a1%20(15).png){ width="350" }
![TEXT](recursos/imgs/act1/a1%20(19).png){ width="350" }
---

### Delta Robots (Parallel)

They are robots with a **parallel structure**, where multiple arms operate simultaneously on a moving platform. They are characterized by high speed and precision.

**Typical applications:** pick and place, packaging, product sorting.

![TEXT](recursos/imgs/act1/a1%20(3).png){ width="350" }
![TEXT](recursos/imgs/act1/a1%20(13).png){ width="350" }

## Other Robot Morphologies

In addition to classical industrial robots classified by their kinematic chains (PPP, RRP, RRR, etc.), there are **other robot morphologies** whose design is not focused exclusively on fixed manipulation. These robots expand the field of robotics toward **mobility, interaction with unstructured environments, task adaptability, and human collaboration**. Their morphology responds to specific needs such as autonomous locomotion, exploration, structural reconfiguration, or the safe manipulation of objects and people.

---

### Wheeled Mobile Robots

These robots are designed for **autonomous movement**, typically using wheels (differential, omnidirectional, or tracked). Unlike industrial manipulators, they are not anchored to a fixed base.

**Key characteristics:**
- High energy efficiency  
- Relatively simple kinematic control  
- High autonomy in flat environments  

**Typical applications:** service robotics, logistics, educational robots, autonomous vehicles.

![TEXT](recursos/imgs/act1/a1%20(4).png){ width="350" }
![TEXT](recursos/imgs/act1/a1%20(5).png){ width="350" }
---

### Bipedal and Humanoid Robots

They feature a morphology inspired by the **human body**, usually consisting of two legs, a torso, arms, and a head. Their main advantage is compatibility with environments designed for humans.

**Key characteristics:**
- High mechanical and control complexity  
- Critical dynamic stability  
- Advanced human–robot interaction  

**Typical applications:** research, assistance, social interaction, technological demonstrators.

![TEXT](recursos/imgs/act1/a1%20(14).png){ width="350" }
![TEXT](recursos/imgs/act1/a1%20(18).png){ width="250" }
---

### Biomimetic Robots

Their design is inspired by **living organisms**, such as snakes, insects, fish, or birds. The morphology is adapted to the type of environment in which the robot operates.

**Key characteristics:**
- High adaptability to complex terrain  
- Non-conventional motion patterns  
- Highly specialized design  

**Typical applications:** exploration, rescue, inspection in confined or hazardous spaces.

![TEXT](recursos/imgs/act1/a1%20(21).png){ width="350" }
![TEXT](recursos/imgs/act1/a1%20(9).png){ width="350" }
---

### Modular Robots

They are composed of **repetitive modules** that can be assembled in different configurations, allowing the robot to change its shape and functionality.

**Key characteristics:**
- Physical reconfiguration  
- High redundancy  
- Structural flexibility  

**Typical applications:** research, experimental robotics, adaptive systems.

![TEXT](recursos/imgs/act1/a1%20(17).png){ width="350" }
![TEXT](recursos/imgs/act1/a1%20(20).png){ width="350" }
---

### Soft Robots

They are built using **flexible materials** such as silicones or elastomers, rather than traditional rigid structures.

**Key characteristics:**
- High safety during physical interaction  
- High deformability  
- Complex non-linear control  

**Typical applications:** medical robotics, grippers for fragile objects, human–robot interaction.

![TEXT](recursos/imgs/act1/a1%20(22).png){ width="350" }
![TEXT](recursos/imgs/act1/a1%20(1).png){ width="350" }
