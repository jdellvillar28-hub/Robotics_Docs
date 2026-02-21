# Activity - ROS 2 Custom Interfaces

## Description

In this activity, you will implement the battery + LED panel example to understand Services in ROS 2. The task is to turn on an LED when the battery is empty and turn it off when the battery is full.

### Steps to implement:
1. **Battery Node:** Simulate the battery state using a variable representing its status.
2. **LED Panel Node:** Manage the LED panel with an integer array to track the LEDs' states.
3. **Custom Messages and Services:** Create a custom message for the LED panel state and a custom service to control the LEDs.
4. **Simulation:** Simulate the battery state change, alternating between empty and full every 4 and 6 seconds, respectively, and turn the LEDs on/off via service calls.

### Suggested flow:
- **Step 1:** Create the LED panel node and define the LED state.
- **Step 2:** Add a service server within the LED panel node to control the LEDs.
- **Step 3:** Create the battery node, simulate the battery life, and call the "set_led" service using a service client.

This process will loop indefinitely, toggling between the two states until CTRL+C is pressed.

## Requirements:
- Create 1 node for the battery.
- Create 1 node for the LED panel.
- Define a custom message for the “led_panel_state” topic.
- Define a custom service for “set_led”.

---

**Hint:**  
- **Step 1:** Create the LED panel node and publish the LED state (with a custom message).
- **Step 2:** Add a service server inside the LED panel node (with a custom service definition).
- **Step 3:** Create the battery node, simulate the battery cycle, and call the "set_led" service using a service client.

# Explanation of Code: LED Server Node for Battery Status

## Code 1 - LED Server Node

This Python code defines a ROS 2 service server that listens for battery status requests and controls the LED behavior based on the battery status. The service uses the custom `BateryStatus` service definition from the `my_robot_interfaces` package.

### 1. Importing Libraries

```python
import rclpy
from rclpy.node import Node
from my_robot_interfaces.srv import BateryStatus  # Import the service
````

- rclpy: The ROS 2 Python library, which provides the necessary classes to create ROS nodes and handle communication between nodes.

- Node: A base class from rclpy.node to create a node in ROS 2.

- BateryStatus: The custom service message for battery status, imported from the my_robot_interfaces.srv module.

### 2. Defining the LedsServer Node

```python
class LedsServer(Node):
    def __init__(self):
        super().__init__('leds_server')
        self.srv = self.create_service(BateryStatus, 'battery_status_service', self.handle_service)
        self.get_logger().info('LEDs is ready')
````
- LedsServer(Node): The class inherits from Node, allowing it to function as a ROS node.

- create_service: This function creates a service server, which listens for service requests. The service type is BateryStatus, the service name is 'battery_status_service', and the callback function for handling requests is handle_service.

- get_logger().info: Logs that the server is ready to handle requests.