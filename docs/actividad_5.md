# Activity - ROS 2 Services

## Objective

Let's practice with ROS 2 Services! You will build on the previous Topic activity where you worked with a publisher and subscriber. 

### Quick Recap:

- The node `number_publisher` publishes a number to the `/number` topic.
- The node `number_counter` subscribes to the `/number` topic, adds the number to a counter, and publishes the updated counter to the `/number_count` topic.

### What You Will Add:

- **Service**: You will create a service to reset the counter to zero.

## Steps

### 1. Create a Service Server

- **Inside the `number_counter` node**, create a service server.
- **Service name**: `"/reset_counter"`.
- **Service type**: `example_interfaces/srv/SetBool`.
    - Use `ros2 interface show` to explore the contents of the service type.

### 2. Service Logic

- When the service is called, check the boolean data from the request.
- If the value is `True`, set the counter variable to 0.

This service will allow you to reset the counter whenever needed.

## Service Server in ROS 2

A **Service Server** in ROS 2 is a node that provides a service, managing request/response interactions. It listens for requests, performs the requested action, and sends a response back.

In ROS 2, a service is defined by two components:

1. **Request**: The data the **Service Client** sends to the **Service Server**.
2. **Response**: The data the **Service Server** sends back after processing the request.

In your case, the **Service Server** will handle requests to reset a counter and will send back a response indicating whether the action was successful or not.

### Example:

* **Service Client**: Sends a request to the **Service Server** to reset the counter.
* **Service Server**: Receives the request, processes it (resets the counter), and sends a response indicating success or failure.

## CODE 1 - Publisher Node for Activity 2

### 1. **Importing Libraries and Defining the Class**

```python
import rclpy  # ROS 2 Python library
from rclpy.node import Node  # Base class to create ROS 2 nodes
from example_interfaces.msg import Int64  # Int64 message type
```

In this part, the necessary libraries are imported to work with ROS 2 in Python. rclpy is the main ROS 2 library, Node is the base class to create nodes, and Int64 is the message type used to publish integer values.

### 2. Defining the myNode_function Class
```python
class myNode_function(Node):  # Defines the node
    def __init__(self):  # Constructor
        super().__init__("act2_number_publisher")  # Initializes the node with a name
        self.counter = 1
        self.get_logger().info('r2d2 act2 is ready')  # Log message indicating the node is ready
        self.create_timer(1.0, self.print_callback)  # Timer that calls print_callback every second
        self.publishers_ = self.create_publisher(Int64, 'number', 10)  # Publisher for messages on the 'number' topic
```
This part defines the myNode_function class, which inherits from Node. In the constructor:

- The node is initialized with the name act2_number_publisher.

- A counter variable is initialized to 1.

- A log message is created to indicate that the node is ready.

- A timer is created to call the print_callback function every 1 second.

- A publisher is set up to send Int64 messages to the number topic.

### 3. Defining the print_callback Function
```python
    def print_callback(self):  # Called by the timer
    self.get_logger().info('r2d2 act2 is sending a message')  # Log message indicating a message is being sent
    msg = Int64()  # Creates the message
    msg.data = self.counter  # Assigns the message with the counter value
    self.publishers_.publish(msg)  # Publishes the message
```
The print_callback function is called whenever the timer is triggered. In this function:

- A log message is created indicating that a message is being sent.

- An Int64 message is created.

- The value of the counter is assigned to the message.

- Finally, the message is published to the number topic.

### 4. Defining the Main Function (main)
```python
def main(args=None):  # Main function
    rclpy.init(args=args)  # Initializes ROS 2
    act2_number_publisher = myNode_function()  # Creates the node
    rclpy.spin(act2_number_publisher)  # Keeps the node running
    rclpy.shutdown()  # Shuts down ROS 2
```
### 5. Executing the Script
```python
if __name__ == "__main__":  # Executes the main function if the script is run directly
    main()
```

This block ensures that the main function is executed only when the script is run directly (not when it is imported as a module). When executed, it starts the node and keeps it running until shut down.

### 6. Full Code
This code creates a ROS 2 node that publishes integer values (Int64) to the number topic every second, starting with the value 1.

```python
#-----------PUBLSHER NODE FOR ACTIVITY 2----------------

import rclpy  # ROS 2 Python library
from rclpy.node import Node  # Base class to create ROS 2 nodes
from example_interfaces.msg import Int64  # Int64 message type

class myNode_function(Node):  # Defines the node

    def __init__(self):  # Constructor
        super().__init__("act2_number_publisher")  # Initializes the node with a name
        self.counter = 1
        self.get_logger().info('r2d2 act2 is ready')  # Log message indicating the node is ready
        self.create_timer(1.0, self.print_callback)  # Timer that calls print_callback every second
        self.publishers_ = self.create_publisher(Int64, 'number', 10)  # Publisher for messages on the topic
        
    def print_callback(self):  # Called by the timer
        self.get_logger().info('r2d2 act2 is sending a message')  # Log message indicating a message is being sent
        msg = Int64()  # Creates the message
        msg.data = self.counter  # Assigns the message with the counter value
        self.publishers_.publish(msg)  # Publishes the message

def main(args=None):  # Main function
    rclpy.init(args=args)  # Initializes ROS 2
    act2_number_publisher = myNode_function()  # Creates the node
    rclpy.spin(act2_number_publisher)  # Keeps the node running
    rclpy.shutdown()  # Shuts down ROS 2

if __name__ == "__main__":  # Executes the main function if the script is run directly
    main()
```