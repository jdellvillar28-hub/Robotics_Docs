<<<<<<< HEAD
# ROS2 Node Creation Task

The task outlined requires creating two nodes in ROS2:

### `number_publisher` Node:
This node is responsible for publishing a constant number on the `/number` topic using the `example_interfaces/msg/Int64` message type. After creating this publisher, you should test it using the command `ros2 topic echo /number` to ensure it's working correctly.

### `number_counter` Node:
This node has both a subscriber and a publisher. It subscribes to the `/number` topic, which receives the number published by the `number_publisher`. The node keeps track of the received numbers using a counter variable. Each time a new number is received, it adds that number to the counter. After updating the counter, the node publishes the new counter value on the `/number_count` topic.

=======
# Robot Morphology

In robotics, **robot morphology** refers to the physical structure of a robot and the arrangement of its joints. This configuration determines its **degrees of freedom**, the shape of its **workspace**, its type of motion, and its applications.

A common way to classify industrial manipulators is by the **sequence of joint types** that compose them:

- **P (Prismatic):** linear motion  
- **R (Rotational or Revolute):** angular motion  

Based on this classification, the following main robot morphologies are identified.
>>>>>>> 2d2e04213ebe90482657c80e86cfacea555df96a

![TEXT](recursos/imgs/act1/a1%20(7).png){ width="600" }

---