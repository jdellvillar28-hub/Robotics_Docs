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
```

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
```
- LedsServer(Node): The class inherits from Node, allowing it to function as a ROS node.

- create_service: This function creates a service server, which listens for service requests. The service type is BateryStatus, the service name is 'battery_status_service', and the callback function for handling requests is handle_service.

- get_logger().info: Logs that the server is ready to handle requests.

### 3. Service Request Handling

```python
    def handle_service(self, request, response):
        # Convertimos los valores bool a 1 y 0
        value1 = 1 if request.value1 else 0
        value2 = 1 if request.value2 else 0
        value3 = 1 if request.value3 else 0
        
        # Imprimimos los valores de la batería convertidos a 1 y 0
        self.get_logger().info(f"Received battery status: {value1}, {value2}, {value3}")
        
        # Lógica para imprimir el estado de la batería basado en el tercer valor (value3)
        if request.value3:
            self.get_logger().info("Low Battery")
        else:
            self.get_logger().info("Full Battery")
        
        # Devolvemos los valores tal cual
        response.value1 = request.value1
        response.value2 = request.value2
        response.value3 = request.value3
        return response
   ``` 

   - handle_service: This function is called whenever a service request is received. It processes the request and sends back a response.

- The battery values (value1, value2, value3) are boolean (True/False), which are converted to integers (1 or 0).

- The logger prints the battery status after converting the values to integers.

- If the third value (value3) indicates the battery is low, it logs "Low Battery". Otherwise, it logs "Full Battery".

- Finally, the function returns the received values in the response.

### 4. Main Function to Spin the Node

```python
def main(args=None):
    rclpy.init(args=args)
    server = LedsServer()
    rclpy.spin(server)
    rclpy.shutdown()
```

- rclpy.init: Initializes the ROS 2 client library.

- LedsServer(): An instance of the LedsServer node is created.

- rclpy.spin: This function keeps the node running and listening for incoming requests.

- rclpy.shutdown: Shuts down the ROS 2 client library once the node is stopped.

### 5. Running the Node
if __name__ == '__main__':
    main()

- This ensures that the main function is called when the script is executed directly (as opposed to being imported as a module).

### Full Code

```python
import rclpy
from rclpy.node import Node
from my_robot_interfaces.srv import BateryStatus  # Importa el servicio

class LedsServer(Node):
    def __init__(self):
        super().__init__('leds_server')
        self.srv = self.create_service(BateryStatus, 'battery_status_service', self.handle_service)
        self.get_logger().info('LEDs is ready')

    def handle_service(self, request, response):
        # Convertimos los valores bool a 1 y 0
        value1 = 1 if request.value1 else 0
        value2 = 1 if request.value2 else 0
        value3 = 1 if request.value3 else 0
        
        # Imprimimos los valores de la batería convertidos a 1 y 0
        self.get_logger().info(f"Received battery status: {value1}, {value2}, {value3}")
        
        # Lógica para imprimir el estado de la batería basado en el tercer valor (value3)
        if request.value3:
            self.get_logger().info("Low Battery")
        else:
            self.get_logger().info("Full Battery")
        
        # Devolvemos los valores tal cual
        response.value1 = request.value1
        response.value2 = request.value2
        response.value3 = request.value3
        return response

def main(args=None):
    rclpy.init(args=args)
    server = LedsServer()
    rclpy.spin(server)
    rclpy.shutdown()

if __name__ == '__main__':
    main()
```

## Code - Battery Client Node

This code defines a **ROS 2 client node** that interacts with the **battery status service**. The client sends requests to the service, toggles battery states (from "full" to "empty"), and processes the service's response asynchronously.

### Code Breakdown

#### 1. Importing Libraries

```python
import rclpy
from rclpy.node import Node
from my_robot_interfaces.srv import BateryStatus  # Import the service

```

- rclpy: ROS 2 Python library, used to handle nodes and communication.

- Node: A base class to define a ROS 2 node.

- BateryStatus: Custom service type for battery status, imported from the my_robot_interfaces.srv module.

### 2. Defining the BatteryClient Node

```python
class BatteryClient(Node):
    def __init__(self):
        super().__init__('battery_client')
        self.client = self.create_client(BateryStatus, 'battery_status_service')
```

- BatteryClient(Node): Inherits from Node, creating a custom ROS 2 node.

- create_client: This function creates a client that will interact with the battery_status_service using the BateryStatus service type.

3. Waiting for the Service to Be Available

```python
        # Esperar a que el servicio esté disponible
        while not self.client.wait_for_service(timeout_sec=1.0):
            self.get_logger().info('Service not available, waiting again...')
 ```           

- wait_for_service: This function blocks until the service is available. It waits up to 1 second before checking again.

### 4. Setting Initial Battery Values and Timer

```phython
        self.request = BateryStatus.Request()
        self.request.value1 = False
        self.request.value2 = False
        self.request.value3 = True

        self.time_counter = 0  # Contador de tiempo
        self.state_duration = 6  # Duración en segundos para mantener el valor en True

        self.timer = self.create_timer(1.0, self.update_request)  # Llamamos update_request cada 1 segundo
```

- self.request: Initializes the request object for the service with value1, value2, and value3.

- value3 is initially set to True (indicating a full battery).

- self.time_counter: Counter to track the time elapsed.

- self.state_duration: Defines how long to keep the battery in the "True" (full) state.

- self.create_timer: Sets a timer to call update_request() every 1 second.

### 5. Updating the Request and Changing States

```phython
    def update_request(self):
        self.time_counter += 1  # Aumentamos el contador de tiempo

        if self.time_counter >= self.state_duration:
            self.request.value3 = False  # Cambio a estado de batería vacía
            self.state_duration = 4  # Cambiar la duración a 4 segundos para False
            self.time_counter = 0  # Reiniciar el contador

            self.send_request()  # Enviar la solicitud al servicio

        elif self.time_counter == 1:
            self.request.value3 = True  # Cambio a estado de batería llena
            self.send_request()  # Enviar la solicitud al servicio
```

- update_request(): This function is called every second by the timer.

- It increments the time counter and checks if the time duration for the current state has elapsed.

- If the duration is met, it switches the state of value3 (battery) to False (empty) and sets a new duration for the next state (4 seconds for empty).

- If the timer is still within the "full" state duration (6 seconds), it sends the request to update the state.

### 6. Sending the Request to the Service

```python
    def send_request(self):
        # Enviamos la solicitud al servicio
        future = self.client.call_async(self.request)  # Enviar la solicitud
        future.add_done_callback(self.handle_response)  # Usamos un callback para manejar la respuesta sin bloquear
```

- call_async(self.request): Sends the request to the service asynchronously, allowing the node to continue executing without blocking.

- add_done_callback(self.handle_response): Registers the handle_response function to process the service response when it's available.

### 7. Handling the Service Response

```phython
    def handle_response(self, future):
        try:
            # Obtenemos la respuesta
            response = future.result()

            # Convertimos los valores bool en 1 y 0
            value1 = 1 if response.value1 else 0
            value2 = 1 if response.value2 else 0
            value3 = 1 if response.value3 else 0

            # Imprimimos los valores de la respuesta como 1 y 0 en lugar de True/False
            self.get_logger().info(f"Response received: {value1}, {value2}, {value3}")

        except Exception as e:
            self.get_logger().error(f"Service call failed: {e}")
```

- handle_response(): This function processes the response received from the service.

- The boolean values (True/False) are converted to 1/0 for easier handling.

- Logs the received values.

- If the service call fails, it logs an error.

### 8. Main Function to Spin the Node

```python
def main(args=None):
    rclpy.init(args=args)
    client = BatteryClient()
    rclpy.spin(client)  # Mantener el nodo en ejecución mientras el temporizador cambia los valores
    rclpy.shutdown()
```

- rclpy.init: Initializes the ROS 2 client library.

- BatteryClient(): Instantiates the client node.

- rclpy.spin(client): Keeps the node running and processing requests asynchronously.

- rclpy.shutdown: Shuts down the ROS 2 client library when the node stops.

### 9. Running the Node

```phython
if __name__ == '__main__':
    main()
```

- This ensures that the main function is executed when the script is run directly.

### Full Code

This code defines a ROS 2 client node that periodically requests the battery status from a service. It toggles the battery state between "full" and "empty" every 6 and 4 seconds, respectively. The client sends asynchronous requests and handles responses using a callback, allowing non-blocking operation while interacting with the service.

```python
import rclpy
from rclpy.node import Node
from my_robot_interfaces.srv import BateryStatus  # Importa el servicio

class BatteryClient(Node):
    def __init__(self):
        super().__init__('battery_client')
        self.client = self.create_client(BateryStatus, 'battery_status_service')

        # Esperar a que el servicio esté disponible
        while not self.client.wait_for_service(timeout_sec=1.0):
            self.get_logger().info('Service not available, waiting again...')
        
        self.request = BateryStatus.Request()
        # Inicialmente definimos los valores para la batería
        self.request.value1 = False
        self.request.value2 = False
        self.request.value3 = True

        # Controlador de tiempo para alternar entre True (6s) y False (4s)
        self.time_counter = 0  # Contador de tiempo
        self.state_duration = 6  # Duración en segundos para mantener el valor en True

        # Creamos el temporizador para cambiar el estado cada 1 segundo
        self.timer = self.create_timer(1.0, self.update_request)  # Llamamos update_request cada 1 segundo

    def update_request(self):
        self.time_counter += 1  # Aumentamos el contador de tiempo

        # Cambiar el estado según el contador
        if self.time_counter >= self.state_duration:
            # Cuando el tiempo es mayor o igual a la duración, cambiamos el estado a False
            self.request.value3 = False
            self.state_duration = 4  # Cambiamos la duración a 4 segundos para False
            self.time_counter = 0  # Reiniciamos el contador

            # Solo enviamos la solicitud una vez cuando el estado cambia
            self.send_request()

        elif self.time_counter == 1:
            # Cuando el contador es 1 y estamos en el estado de True, enviamos la solicitud
            self.request.value3 = True
            self.send_request()

    def send_request(self):
        # Enviamos la solicitud al servicio
        future = self.client.call_async(self.request)  # Enviar la solicitud
        future.add_done_callback(self.handle_response)  # Usamos un callback para manejar la respuesta sin bloquear

    def handle_response(self, future):
        try:
            # Obtenemos la respuesta
            response = future.result()

            # Convertimos los valores bool en 1 y 0
            value1 = 1 if response.value1 else 0
            value2 = 1 if response.value2 else 0
            value3 = 1 if response.value3 else 0

            # Imprimimos los valores de la respuesta como 1 y 0 en lugar de True/False
            self.get_logger().info(f"Response received: {value1}, {value2}, {value3}")

        except Exception as e:
            self.get_logger().error(f"Service call failed: {e}")

def main(args=None):
    rclpy.init(args=args)
    client = BatteryClient()
    rclpy.spin(client)  # Mantener el nodo en ejecución mientras el temporizador cambia los valores
    rclpy.shutdown()

if __name__ == '__main__':
    main()
```

# Results

When you run both the LED server and the battery client nodes, you should see logs in the terminal indicating the battery status changes and the corresponding responses from the service. The LED server will log whether the battery is "Low" or "Full" based on the client's requests, while the client will log the responses received from the server.
![TEXT](recursos/imgs/act6/a%20(1).png){width=700px}
![TEXT](recursos/imgs/act6/a%20(2).png){width=700px}
![TEXT](recursos/imgs/act6/a%20(3).png){width=700px}