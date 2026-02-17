
# ROS2 Node Creation Task

The task outlined requires creating two nodes in ROS2:

# Activity Information

The activity involves creating two nodes:

1. **number_publisher Node:**
   - Publishes a constant number to the `/number` topic using the `example_interfaces/msg/Int64` message type.
   - The published value will always be the same number.

2. **number_counter Node:**
   - Subscribes to the `/number` topic and maintains a counter.
   - Each time a new number is received, it adds it to the counter.
   - It also has a publisher that publishes the new counter value to the `/number_count` topic.


## number_publisher Node:

This node is responsible for publishing a constant number on the `/number` topic using the `example_interfaces/msg/Int64` message type. After creating this publisher, you should test it using the command `ros2 topic echo /number` to ensure it's working correctly.

## number_counter Node:

This node has both a subscriber and a publisher. It subscribes to the `/number` topic, which receives the number published by the `number_publisher`. The node keeps track of the received numbers using a counter variable. Each time a new number is received, it adds that number to the counter. After updating the counter, the node publishes the new counter value on the `/number_count` topic.

# Publisher Node Explanation

## 1. Imports and Libraries

```python 
import rclpy  # ROS 2 Python library
from rclpy.node import Node  # Base class to create ROS 2 nodes
from example_interfaces.msg import String  # String message type
```

- `rclpy`: This is the main library used to interact with ROS 2 in Python. It allows the creation of nodes, message publishing and subscribing, and event handling.

- `Node`: This is the base class for creating a node in ROS 2. Nodes are fundamental processing units in ROS 2.

- `String`: This imports the String message type from example_interfaces, which enables sending and receiving text-based (string) data through a topic.



## 2. Class Definition `myNode_fuction`

`class myNode_fuction(Node):`

```python 
class myNode_fuction(Node):  # Defines the node
```

- A class `myNode_fuction` is defined, inheriting from `Node`. This means that `myNode_fuction` is a ROS 2 node that can perform tasks within the ROS system. 
- The node will interact with the ROS system by publishing messages to a topic every second.

## 3. The Constructor `__init__`

```python
def __init__(self):  # Constructor
    super().__init__("my_second_mode")  # Initializes the node with a name
    self.counter = 0  # Counter

    self.create_timer(1.0, self.print_callback)  # Timer that calls print_callback every second
    self.publishers_ = self.create_publisher(String, 'Robot_speaking', 10)  # Publisher for messages on the topic
```
- `_init_(self)`: The constructor of the node. It initializes the node with the name `"my_second_mode"`, defines a  `counter ` variable to count the messages, and sets up two key elements:

create_timer(1.0, self.print_callback): Creates a timer that calls the print_callback function every 1 second.

create_publisher(String, 'Robot_speaking', 10): Creates a publisher that will send messages of type String to the Robot_speaking topic, with a queue size of 10 (the number of messages it can hold in the buffer).



![TEXT](recursos/imgs/act1/a1%20(7).png){ width="600" }



