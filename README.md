# Network Scanner with Python

# Description
Through the use of Python, a Network Scanning tool has been created, and by utilizing various Python Modules and libraries such as Scapy, Threading, Socket, Queue, and ipaddress, ARP packets were generated and could be manipulated for reconnaissance of active hosts within a specified network. It uses multithreading for performance enhancements and gathers IP, MAC, and Hostname details of discovered devices.

# Tools and Utilities
![image 1](https://github.com/user-attachments/assets/5b4b9f8c-75a9-4a66-aad2-2aef2c0616f3)

- **import scapy.all as scapy** - Scapy library used for packet manipulation
- **import socket** - Provides low level networking interfaces
- **import threading** - Allows for the creation and management of threads
- **from queue import Queue** - Queue class used to provide thread-safe FIFO Implementation
- **import ipaddress** - Modules used to allow IPv4 manipulation and inspection

# Documentation
## Creating an ARP Request

In order to create an **ARP Request,** Scapy must be installed which is a **packet manipulation tool**

![image 2](https://github.com/user-attachments/assets/8d36bb11-42b9-4966-a740-40403a233778)

![image 3](https://github.com/user-attachments/assets/cb26d550-8355-4396-9410-7bb25594bbcd)

- **def scan(ip)** - A function is defined to specify the desired IP Address that will be scanned
    - **arp_request** - Using Scapy, an ARP Request packet is sent to the IP Address given from the function
    - **broadcast** - An Ethernet frame is created with the broadcast destination MAC Address
    - **packet** - A combination of the ARP Request and the MAC Address to create a complete packet
    - **answer** - scapy.srp will send the packet and wait for a response. In this case the response wait time is 1 second

## Extracting Info From the Answer

In this part of the code, it will be used to extract information from the response of the designated IP Address such as: IP, MAC Address, Host Name with the use of a **list** in order to **store** information of the target

![image 4](https://github.com/user-attachments/assets/e3e98874-0f1c-47a8-be67-4897faffa830)

- **clients [ ]** - An empty list is initialized to hold desired target information once the scanning/extraction is complete
- **for client in answer** - Will iterate of the answer received from the ARP Request packet
- **client_info = {’IP’: client[1].psrc, ‘MAC’: client[1].hwsrc}** - Extracts the IP and MAC Address from the response
    - **try -** Attempts to resolve the hostname by using **socket**
    - **hostname = socket.gethostbyaddr(client_info['IP'])[0] -** Retrieves the Hostname of the given IP
    - **client_info[’Hostname’] = hostname** - Adds the hostname to the client information variable
- **except socket.herror** - Handles errors that occur during the hostname resolution
- **client_info[’Hostname’] = ‘Uknown’ -** Will provide by default if the hostname cannot be resolved

![image 5](https://github.com/user-attachments/assets/c345a693-a37d-42c7-b577-2c0fe2b7b9f6)

- **def print_result(result)** - Defines a function to print scanning results
    - **print(’IP’ + “”*20 + ‘MAC’ + “”*20 + ‘Hostname’)** - Prints the header for the results table
    - **print(’-’*80) -** Prints a divider line
- **for client in result** - Iterates over the clients[ ] list in order to print target information
    - **print(client[’IP’] + ‘\t\t’ +client[’MAC’ + ‘\t\t’ + client[Hostname’])** - Prints the IP, MAC Address, and Host name of each client

## Using Threading to Speed Up Scans

![image 6](https://github.com/user-attachments/assets/0cedaf20-10b1-4f56-9ba6-f2dff9e8b59a)

![image 7](https://github.com/user-attachments/assets/19d85a7b-87d7-4f66-ad7e-7180d28b11ad)

- **def main(cidr)** - Defines the main function for network scanning of a given CIDR
    - **results_queue = Queue()** - Creates the queue to store results from each thread
    - **threads = [ ]** - Initializes an empty list to store threads
    - **network= ipaddress.ip_network(cidr, strict=False)** - Creates an ip_network object from the given CIDR allow iteration over all hosts in the network
- **for ip in network.hosts** - Iterates over all host addresses in the network
    - **thread = threading.Thread(target=scan, args=(str(ip), results_queue))** - Creates a new thread to scan the current IP address and store the result in the queue
    - **thread.start( )** - Starts the thread
    - **threads.append(thread**) - Adds the thread to the list of threads
- **for thread in threads**  - Iterates over the list of threads
    - **thread.join** - Waits for each thread to complete
- **all_clients = [ ] -** Initializes an empty list to store all client information
- **while not results_queue.empty( )** - Loops while the result queue is not empty
    - **all_clients.extend(result_queue.get( ))** - Retrieves and extends the list of clients with results from the queue
- **print_result(all_clients)** - Calls the function to print the final list of clients

## Main Function

![image 8](https://github.com/user-attachments/assets/eb59fc0c-4796-46cc-aadb-558cc1f35924)

- **if __name__ = ‘__main__’** - Ensures that the code block only runs if the script is executed directly
- **cidr= input(’Enter network IP Address: ‘) -** Prompts the user to input a CIDR network address
- **main(cidr) -** Calls the main functions with the provided CIDR address

# Output
![Screenshot 2025-04-12 121815](https://github.com/user-attachments/assets/b9a2d8c7-f1f9-4e63-87f6-bd86ec83f682)


# Code
![Screenshot (1416)](https://github.com/user-attachments/assets/f711309a-c138-4000-aac6-caec196638bb)
