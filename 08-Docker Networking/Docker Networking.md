# Docker Networking

Docker networking enables communication between containers, allowing them to share data and services. 
It provides different network types like bridge, overlay, and macvlan for connectivity, isolation, and 
scalability. Docker networking simplifies containerized application deployment and enables seamless 
interaction between containers within and across hosts.

> **Note:** Docker networking allows you to attach a container to as many networks as you like.

## Container Network Model (CNM)

- **IPAM (IP Address Management)** plugin APIs are used to create/delete address pools and allocate/deallocate container IP addresses.
- **Network plugin** APIs are used to create/delete networks and add/remove containers from networks.

## Network Drivers/Types

There are 7 network drivers/types used in Docker networking:

1. **Default Bridge**
2. **User-defined Bridge**
3. **MACVLAN**
4. **IPVLAN (L2)**
5. **IPVLAN (L3)**
6. **Overlay Network**
7. **None**

### 1.Default Bridge

The default bridge network is automatically created when Docker is installed. It allows containers on the same Docker host to communicate with each other using IP addresses. It provides basic networking functionality but lacks features like multi-host communication.

### 2.User-defined Bridge

User-defined bridge networks allow you to create custom networks with specific configurations, such as IP address ranges and subnet settings. Containers connected to the same user-defined bridge network can communicate with each other. This network type provides more control and isolation compared to the default bridge network.

### 3.MACVLAN

MACVLAN networks allow containers to have their own MAC addresses and appear as separate devices on the physical network. This enables direct connectivity with external systems, and containers can be accessed from other devices on the network.

#### MACVLAN 802.1q

MACVLAN 802.1q is an extension of MACVLAN that supports trunking and VLAN tagging. It allows containers to connect to VLAN tagged networks and provides network segmentation within the container environment.

### 4.IPVLAN (L2)

IPVLAN networks provide each container with a unique MAC and IP address on the physical network. Containers in an L2 (Layer 2) IPVLAN network can communicate with other devices on the same Layer 2 network, but they are isolated from each other within the container environment.

### 5.IPVLAN (L3)

IPVLAN networks operate at Layer 3 and provide containers with their own IP addresses on the physical network. Containers in an L3 IPVLAN network can communicate with each other and external systems using IP routing.

### 6.Overlay Network

Overlay networks are used for multi-host communication and container orchestration. They allow containers to communicate transparently across different Docker hosts or nodes in a Docker Swarm cluster. Overlay networks use encapsulation to enable communication between containers running on different hosts.

### 7.None

The "none" network is a special network type where containers are not connected to any network. Containers in this network type are isolated and cannot communicate with other containers or external systems.

These network types offer different functionalities and features to cater to various deployment scenarios 
and requirements. Choosing the appropriate network type depends on factors such as network isolation needs, 
performance, communication across multiple hosts, and direct connectivity with external systems.
