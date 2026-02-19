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
## Code 2 - Subscriber and Publisher Node for Activity 2

### 1. **Importing Libraries and Defining the Class**

```python
import rclpy  # ROS 2 library
from rclpy.node import Node  # ROS 2 Node base class
from example_interfaces.msg import Int64  # Int64 message type
from example_interfaces.srv import SetBool  # SetBool message type
```
In this part, the necessary libraries are imported:

- rclpy is the main ROS 2 library for Python.

- Node is the base class for creating ROS 2 nodes.

- Int64 is the message type used to send integer values.

- SetBool is the service type for the reset counter service.

### 2. Defining the myNode_function Class
```python
class myNode_function(Node):  # Defines the node

    def __init__(self):
        super().__init__('act2_number_counter')  # Initialize the node
        
        self.get_logger().info('c3po act2 is ready')  # Log sending message
        
        self.suscriber_ = self.create_subscription(Int64, 'number', self.listener_callback, 10)
        self.publishers_ = self.create_publisher(Int64, 'number_count', 10)
        self.serve = self.create_service(SetBool, 'reset_counter', self.reset_counter_callback)

        self.accumulated = 0  # Initialize the counter variable
        self.counter = 0  # Initialize the counter variable
      
        self.create_timer(1.0, self.print_callback)
```
In this section, the myNode_function class is defined. It inherits from Node:

- The node is initialized with the name 'act2_number_counter'.

- A log message is created to indicate the node is ready.

- A subscriber is created to listen to the 'number' topic with the listener_callback function.

- A publisher is set up to publish Int64 messages to the 'number_count' topic.

- A service is created to handle the reset_counter service, calling the reset_counter_callback function when requested.

- Two counter variables (accumulated and counter) are initialized.

- A timer is created to call print_callback every second.

### 3. Service Callback for Resetting the Counter

```python
def reset_counter_callback(self, request, response):
    if request.data == True:
        self.accumulated = 0  # Reset the counter to 0
        self.get_logger().info('Counter reset requested')  # Log reset request
        response.success = True  # Indicate success
        response.message = 'Counter reset successfully'  # Response message
    else:
        self.get_logger().info('Counter reset denied')  # Log reset denial
        response.success = False  # Indicate failure
        response.message = 'Counter reset failed'  # Response message
    
    return response
```
The reset_counter_callback function handles the reset_counter service requests:

- If the request data is True, it resets the accumulated counter to 0 and sends a success response.

- If the request data is False, it denies the reset and sends a failure response.

### 4. Listener Callback for Subscription
```python
def listener_callback(self, msg: Int64):
    self.get_logger().info('Received number: %d' % msg.data)  # Log received message
    self.accumulated += msg.data  # Increment the counter by received value
    self.get_logger().info('Updated counter: %d' % self.accumulated)  # Log updated counter
```
The listener_callback function is called when a new message is received on the 'number' topic:

- It logs the received number.

- The received number is added to the accumulated counter.

- The updated counter value is logged.

### 5. Timer Callback for Publishing Messages
```python
def print_callback(self):
    self.get_logger().info('c3po act2 is sending a message')  # Log sending message
    msg = Int64()  # Create message
    msg.data = self.counter # Set message data to current accumulated value
    self.counter += 1  # Increment the counter for the next message
    self.publishers_.publish(msg)  # Publish message
```
The print_callback function is triggered by the timer every second:

- It logs that a message is being sent.

- It creates a new Int64 message and assigns it the current counter value.

- The counter is incremented for the next message.

- The message is published to the 'number_count' topic.

### 6. Main Function
```python
def main(args=None):
    rclpy.init(args=args)  # Initialize ROS 2
    act2_number_counter = myNode_function()  # Create node instance
    rclpy.spin(act2_number_counter)  # Keep node running
    rclpy.shutdown()  # Shutdown ROS 2
```
The main function initializes ROS 2, creates the node instance (myNode_function), keeps the node running with rclpy.spin(), and shuts down ROS 2 once the node is stopped.

### 7. Executing the Script
```python
if __name__ == "__main__":
    main()  # Run main function if executed directly
```
This block ensures that the main function is executed only when the script is run directly, not when it's imported as a module.

### 8. Full Code
This code creates a ROS 2 node that subscribes to the 'number' topic, accumulates the received numbers, and publishes the accumulated count to the 'number_count' topic. It also provides a service to reset the counter when requested.

```python
#-----------SUSCRIBER AND PUBLISHER NODE FOR ACTIVITY 2----------------

import rclpy  # ROS 2 library
from rclpy.node import Node  # ROS 2 Node base class
from example_interfaces.msg import Int64  # Int64 message type
from example_interfaces.srv import SetBool  # SetBool message type

class myNode_function(Node):  # Defines the node

    def __init__(self):
        super().__init__('act2_number_counter')  # Initialize the node
        
        self.get_logger().info('c3po act2 is ready')  # Log sending message
        
        self.suscriber_ = self.create_subscription(Int64, 'number', self.listener_callback, 10)
        self.publishers_ = self.create_publisher(Int64, 'number_count', 10)
        self.serve = self.create_service(SetBool, 'reset_counter', self.reset_counter_callback)

        self.accumulated = 0  # Initialize the counter variable
        self.counter = 0  # Initialize the counter variable
      
        self.create_timer(1.0, self.print_callback)

    def reset_counter_callback(self, request, response):
        if request.data == True:
            self.accumulated = 0  # Reset the counter to 0
            self.get_logger().info('Counter reset requested')  # Log reset request
            response.success = True  # Indicate success
            response.message = 'Counter reset successfully'  # Response message
        else:
            self.get_logger().info('Counter reset denied')  # Log reset denial
            response.success = False  # Indicate failure
            response.message = 'Counter reset failed'  # Response message
        
        return response
    
    def listener_callback(self, msg: Int64):
        self.get_logger().info('Received number: %d' % msg.data)  # Log received message
        self.accumulated += msg.data  # Increment the counter by received value
        self.get_logger().info('Updated counter: %d' % self.accumulated)  # Log updated counter
    
    def print_callback(self):
        self.get_logger().info('c3po act2 is sending a message')  # Log sending message
        msg = Int64()  # Create message
        msg.data = self.counter # Set message data to current accumulated value
        self.counter += 1  # Increment the counter for the next message
        self.publishers_.publish(msg)  # Publish message

def main(args=None):
    rclpy.init(args=args)  # Initialize ROS 2
    act2_number_counter = myNode_function()  # Create node instance
    rclpy.spin(act2_number_counter)  # Keep node running
    rclpy.shutdown()  # Shutdown ROS 2

if __name__ == "__main__":
    main()  # Run main function if executed directly
```
### Results

![TEXT](recursos/imgs/act5/a%20(1).png){width=700px}
![TEXT](recursos/imgs/act5/a%20(3).png){width=700px}
![TEXT](recursos/imgs/act5/a%20(6).png){width=700px}
![TEXT](recursos/imgs/act5/a%20(4).png){width=700px}
![TEXT](recursos/imgs/act5/a%20(2).png){width=700px}

