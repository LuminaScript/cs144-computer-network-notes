
## I. (APP Layer) Bidirectional Reliable Byte Stream Connection
- A key concept enabling communication in various network applications.
- Provides reliability and order, ensuring that data is delivered accurately between endpoints.

#### **1. World Wide Web (HTTP)**
- **Protocol:** HTTP (`http://`) used for web communication.
- **Client-Server Interaction:**
  1. **Client Action:** Opens a connection and sends a request (`GET / HTTP/1.1`).
  2. **Server Response:** Returns a response (`HTTP/1.1 200 OK`).


#### **2. BitTorrent**
- **Peer-to-Peer File Sharing:**
  1. **Torrent File:** Client acquires a `.torrent` file containing metadata.
  2. **Tracker Info:** The file provides details about the tracker (coordinates peers).
  3. **Peer Interaction:** Client contacts the tracker to find peers and requests pieces of the file from other clients.


#### **3. Skype**
For 2 clients to establish connections and override NAT complication
- **NAT Complications:** A client behind a NAT (Network Address Translation) can establish outbound connections but struggles with inbound connections.
- **Rendezvous Service:** Facilitates connections when one client (e.g., Client B) is behind NAT:
    - Client A calls Rendezvous Service.
    - Rendezvous Service contacts Client B.
    - Client B opens a reverse connection to Client A.
- **Relay Service:** Used when **both clients are behind NAT**, relaying traffic through a server.

## II. The 4 Layer Internet Model

The Internet can be understood through a simplified **4-layer model**, which abstracts the complexity of network communications into manageable layers. Each layer has specific responsibilities and interacts only with the layers directly above and below it.

Each Layer provide a service to the service below

#### 1. **Application Layer**
- **Purpose:** Provides network services directly to user applications.
- **Functionality:**
  - Facilitates interactions such as retrieving URLs.
  - Examples of applications: Web browsers (e.g., Skyes), BitTorrent.
  - Requirements:
    - Often require a bidirectional, reliable byte stream between two endpoints.
    - Applications decide whether they need reliable data delivery (e.g., TCP) or can tolerate some loss (e.g., UDP for video streaming).

#### 2. **Transport Layer**
- **Purpose:** Ensures reliable data transfer between hosts.
- **Protocols:**
  - **TCP (Transmission Control Protocol):**
    - Provides a reliable, ordered, and error-checked delivery of a stream of data.
    - Handles retransmissions, flow control, and congestion control.
    - Guarantees accurate and reliable delivery service to applications.
  - **UDP (User Datagram Protocol):**
    - Offers a connectionless datagram service.
    - No guarantee of delivery, order, or duplicate protection.
    - Suitable for applications where speed is critical and some data loss is acceptable (e.g., video streaming).

#### 3. **Network Layer**
- **Purpose:** Manages the delivery of packets (datagrams) from the source to the destination across multiple networks.
- **Key Components:**
  - **IP (Internet Protocol):**
    - The fundamental protocol for routing packets across networks.
    - Characteristics:
      - **Thin Waist:** Acts as a universal interface, allowing diverse technologies to interconnect.
  - **Datagrams:**
    - Structure: `[Data | Header | From | To]`
    - **Out-of-Order Delivery:** IP does not guarantee the order of packet delivery.
    - **Unreliable Delivery:** Packets may be lost, duplicated, or delivered out of sequence.
  - **Routers:**
    - Devices that forward packets from the link layer to the network layer.
    - Determine the best path for data transmission across interconnected networks.

#### 4. **Link Layer**
- **Purpose:** Handles the physical transmission of data over a specific medium.
- **Responsibilities:**
  - Transmits raw bitstreams over the physical medium (e.g., Ethernet, Wi-Fi).
  - Ensures proper framing, error detection, and handling at the data link level.
  - Interfaces directly with the physical hardware (e.g., network interface cards).
  
### **Interaction Between Layers**
- **Layer Communication:** Each layer communicates only with its corresponding layer on the adjacent devices, abstracting the complexities of other layers.
  - **Example:** The Application layer on one device communicates with the Application layer on another device, regardless of how the Transport, Network, and Link layers are implemented or interact beneath them.


## III. The IP Service
![image](https://hackmd-prod-images.s3-ap-northeast-1.amazonaws.com/uploads/upload_c318a9256d978ef920ffd176f9dbba22.png?AWSAccessKeyId=AKIA3XSAAW6AWSKNINWO&Expires=1737394069&Signature=Yd8%2BFBW%2Fn64JAjDLaQzLEGsuFQk%3D)

### IP Service Characteristics
- **Datagram:** Individually routed packets with hop-by-hop routing (source and destination info in each packet); routers use forwarding tables.
- **Unreliable:** Packets may be dropped, duplicated, or delayed.
- **Best Effort:** Attempts delivery; drops packets only when necessary (e.g., congestion).
- **Connectionless:** No per-flow state; packets may arrive out of sequence.

### Why is IP Service Simple?
- **Efficiency:** Enables faster, cost-effective packet delivery.
- **End-to-End Principle:** Delegates features like reliability to end hosts where possible.
- **Thin Waist Design:** 
  - Supports a variety of services (reliable/unreliable) on top.
  - Operates over diverse link-layer technologies.

### IP Services in Detail
- **Loop Prevention:** Uses a hop count; packets are dropped when count reaches zero.
- **Fragmentation:** Splits oversized packets to meet link layer size restrictions.
- **Header Checksum:** Minimizes errors in delivering packets to incorrect destinations.
- **IP Versions:**
  - **IPv4:** 32-bit addresses.
  - **IPv6:** 128-bit addresses.
- **Extensibility:** Allows for adding new fields in the datagram header.

### IPv4 Datagram 
![image]([https://hackmd.io/_uploads/rJ7CZ2lNJg.png](https://hackmd-prod-images.s3-ap-northeast-1.amazonaws.com/uploads/upload_7c5c2db4ca42f66745eb20caf57fa95c.png?AWSAccessKeyId=AKIA3XSAAW6AWSKNINWO&Expires=1737394267&Signature=z9Wcq0clMWYqoGPAwVC02LNnmmc%3D))
* **Version (4 bits):** Indicates IP version (IPv4 or IPv6).
* **Header Length (4 bits):** Specifies the length of the header in 32-bit words.
* **Type of Service (8 bits):** Provides routing hints about packet priority.
* **Total Packet Length (16 bits):** Overall size of the packet (header + data).
* **Packet ID (16 bits):** Identifies fragments of a single packet.
* **Flags (3 bits) and Fragment Offset (13 bits):** Used for packet fragmentation and reassembly.
* **Time to Live (TTL, 8 bits):** Prevents looping; decremented by each router, dropped if it reaches zero.
* **Protocol ID (8 bits):** Indicates encapsulated protocol (e.g., 6 for TCP).
* **Header Checksum (16 bits):** Verifies header integrity.
* **Source IP Address (32 bits):** Origin of the packet.
* **Destination IP Address (32 bits):** Target destination of the packet.
* **Options (variable, optional):** Additional header fields if required.
