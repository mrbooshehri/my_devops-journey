# TURN Server

A TURN server, which stands for Traversal Using Relays around NAT, is a
type of server used in computer networking to facilitate communication
between devices that are behind Network Address Translation (NAT)
devices or firewalls. NAT is a technology used to map private IP
addresses to a single public IP address, allowing multiple devices
within a local network to share a common internet connection.

When two devices need to communicate over the Internet, they may
encounter issues if one or both of them are behind NAT. This can lead to
difficulties in establishing direct connections, especially for
real-time communication applications like Voice over IP (VoIP) or video
conferencing.

A TURN server helps overcome these issues by acting as a relay between
the communicating devices. Here's how it works:

1. **Device Registration:** When a device encounters a NAT or firewall
	 and is unable to establish a direct connection with another device,
	 it can connect to a TURN server.

2. **Relaying Data:** The TURN server then relays data between the
	 communicating devices. Each device sends its data to the TURN server,
	 and the server forwards it to the other device.

3. **Address Translation:** The TURN server effectively serves as an
	 intermediary, handling the translation of private IP addresses into a
	 public IP address. This allows devices behind NAT to communicate with
	 each other through the server.

TURN is particularly useful in scenarios where direct peer-to-peer
communication is challenging due to NAT traversal issues. It ensures
that devices can communicate even when they are behind restrictive
network configurations.

It's important to note that while TURN helps in establishing
connections, it may introduce additional latency and consume more
bandwidth compared to direct peer-to-peer communication. TURN is often
used in conjunction with other technologies like STUN (Session Traversal
Utilities for NAT) to optimize communication paths and minimize reliance
on relays when direct connections are possible.

# STUN server

A STUN server, which stands for Session Traversal Utilities for NAT, is
a server used in networking to assist devices in discovering and
overcoming Network Address Translation (NAT) and firewall restrictions.
The primary purpose of a STUN server is to enable devices to determine
their public IP address and the type of NAT they are behind. This
information is crucial for establishing direct communication between
devices, especially in real-time communication applications such as
Voice over IP (VoIP), video conferencing, and online gaming.

Here's how a STUN server typically works:

1. **Public IP Address Discovery:** When a device is behind a NAT or
	 firewall, it may not know its public IP address. A STUN server helps
	 the device discover this information by responding to a STUN request.
	 The response from the server includes the public IP address and port
	 number.

2. **NAT Type Detection:** Different types of NAT devices (e.g., full
	 cone, restricted cone, port-restricted cone) behave in distinct ways
	 when it comes to allowing inbound connections. The STUN server's
	 response also helps the device determine its NAT type, providing
	 insights into the network configuration.

3. **Facilitating Peer-to-Peer Connections:** Armed with information
	 about the public IP address and NAT type, the device can use this
	 data to optimize the establishment of direct peer-to-peer
	 connections. This is valuable in scenarios where direct communication
	 is preferred to reduce latency and improve the efficiency of data
	 transfer.

While STUN helps in discovering and adapting to network conditions, it
is important to note that it doesn't handle relaying of data like a TURN
server does. If direct communication between devices is not possible due
to restrictive network configurations, a combination of STUN and TURN
may be employed. STUN is used to identify the public IP address and NAT
type, and if direct communication is not feasible, TURN can be employed
to relay data through a server.

In summary, a STUN server plays a crucial role in helping devices behind
NAT devices and firewalls to understand their network environment,
facilitating optimal communication paths for peer-to-peer connections.

# TURN vs STUN

TURN (Traversal Using Relays around NAT) and STUN (Session Traversal
Utilities for NAT) are both protocols used to address the challenges
posed by Network Address Translation (NAT) in real-time communication
over the Internet. While they share the common goal of facilitating
communication between devices behind NAT, they serve different purposes
in achieving this objective. Here's a detailed comparison of TURN and
STUN:

1. **Purpose:**
	 - **TURN (Traversal Using Relays around NAT):** TURN is primarily
		 used when direct peer-to-peer communication is not possible due to
		 strict NAT or firewall configurations. It acts as a relay server
		 that facilitates the exchange of data between devices by relaying
		 traffic through the server.
	 - **STUN (Session Traversal Utilities for NAT):** STUN, on the other
		 hand, is used for discovering and communicating network
		 information, such as the public IP address and NAT type of a
		 device. It helps devices determine their network environment to
		 optimize the establishment of direct connections when possible.

2. **Functionality:**
	 - **TURN:** TURN servers relay data between devices that cannot
		 establish direct connections. They act as intermediaries, receiving
		 data from one device and forwarding it to another. This relaying
		 process allows communication to occur despite NAT or firewall
		 restrictions.
	 - **STUN:** STUN servers assist devices in learning their public IP
		 addresses and NAT types. This information is crucial for devices to
		 determine how they can establish direct connections with each
		 other.

3. **Data Handling:**
	 - **TURN:** TURN servers actively handle and relay the data being
		 exchanged between devices. They introduce an additional layer in
		 the communication path, which may result in increased latency and
		 bandwidth usage.
	 - **STUN:** STUN servers primarily provide information about the
		 network environment. They do not relay data themselves; instead,
		 they help devices optimize their communication paths.

4. **Latency and Efficiency:**
	 - **TURN:** Introducing a relay server can add latency to the
		 communication process, as data has to travel through an
		 intermediary. This may not be ideal for latency-sensitive
		 applications.
	 - **STUN:** STUN aims to optimize communication paths by helping
		 devices discover their network information, potentially allowing
		 for direct peer-to-peer connections and reducing latency.

5. **Use Cases:**
	 - **TURN:** Ideal for scenarios where direct peer-to-peer
		 communication is not possible due to strict NAT or firewall
		 restrictions. Commonly used in applications like VoIP, video
		 conferencing, and online gaming.
	 - **STUN:** Used to optimize communication paths and enable direct
		 connections when feasible. Particularly useful for improving
		 efficiency in real-time communication applications.

In many cases, both TURN and STUN are used together. STUN helps devices
determine whether direct communication is possible, and if not, TURN can
be employed to ensure reliable communication by relaying data through a
server.

**Comparison of TURN and STUN presented in a table format:**

| Feature                       | TURN (Traversal Using Relays around NAT)          | STUN (Session Traversal Utilities for NAT)            |
|-------------------------------|---------------------------------------------------|------------------------------------------------------|
| **Purpose**                   | Facilitates communication when direct connections are not possible. | Aids devices in discovering network information for optimal direct connections. |
| **Functionality**             | Relays data between devices to overcome NAT/firewall restrictions. | Provides public IP address and NAT type information for devices. |
| **Data Handling**             | Actively relays data between devices through the TURN server. | Provides information but does not actively relay data.    |
| **Latency and Efficiency**    | May introduce additional latency due to relaying through an intermediary. | Aims to optimize communication paths for reduced latency. |
| **Use Cases**                 | Ideal for scenarios with strict NAT/firewall configurations where direct communication is challenging. | Used to optimize communication paths, particularly in real-time applications. |
| **Combination Usage**         | Often used in conjunction with STUN to determine if direct communication is possible, and if not, to relay data through the TURN server. | Can be used independently but often combined with TURN for comprehensive NAT traversal. |


# Real world usecases


1. **Real-Time Communication Apps:**
	 - **Use of STUN:** In a video conferencing application, STUN can be
		 used to help participants behind NAT devices and firewalls discover
		 their public IP addresses and NAT types. This information is then
		 utilized to attempt direct peer-to-peer connections for low-latency
		 communication.
   - **Use of TURN:** If direct connections are not possible due to restrictive network conditions, TURN can be employed to relay video and audio data between participants, ensuring smooth communication even in challenging network environments.

2. **VoIP (Voice over IP) Services:**
	 - **Use of STUN:** STUN servers are commonly used in VoIP services to
		 assist devices in determining their public IP addresses and NAT
		 types. This allows the VoIP application to establish direct
		 connections between users whenever possible.
   - **Use of TURN:** In cases where direct connections are not feasible due to strict firewalls or symmetric NAT, TURN servers can relay voice data between users, ensuring continuous communication.

3. **Online Gaming:**
	 - **Use of STUN:** STUN can be employed in online gaming to optimize
		 the peer-to-peer connection setup between players. It helps in
		 identifying the network conditions, allowing for direct
		 communication whenever feasible.
   - **Use of TURN:** When direct connections are hindered by NAT or firewall restrictions, TURN servers can be used to relay game data, ensuring that players can interact seamlessly.

4. **WebRTC (Web Real-Time Communication):**
	 - **Use of STUN:** WebRTC applications often leverage STUN to gather
		 ICE (Interactive Connectivity Establishment) candidates, including
		 public IP addresses and port information, to establish direct
		 connections between browsers.
   - **Use of TURN:** TURN is a crucial component in WebRTC when direct connections are not possible. It helps in relaying audio, video, and data between browsers, maintaining communication across different network configurations.

5. **Remote Desktop Applications:**
	 - **Use of STUN:** In remote desktop applications, STUN can assist in
		 determining the public IP addresses and NAT types of devices to
		 optimize the setup of direct connections for efficient screen
		 sharing.
   - **Use of TURN:** TURN can be used as a fallback mechanism when direct connections are not achievable, ensuring reliable remote desktop access even in complex network setups.


**The following table summarizing real-world use cases for
TURN and STUN:**

| Use Case                                      | TURN (Traversal Using Relays around NAT)                                                                                                                                               | STUN (Session Traversal Utilities for NAT)                                                                                                                                       |
|-----------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Video Conferencing Apps**                   | Relays video and audio data between participants when direct peer-to-peer connections are not possible due to NAT or firewall restrictions.                                        | Assists participants in discovering their public IP addresses and NAT types, optimizing the establishment of direct connections for low-latency communication.                      |
| **VoIP Services**                             | Relays voice data between users in cases where direct connections are hindered by NAT or firewall restrictions.                                                                       | Helps devices determine public IP addresses and NAT types, facilitating direct peer-to-peer connections for real-time voice communication.                                         |
| **Online Gaming**                             | Relays game data when direct peer-to-peer connections are not feasible due to NAT or firewall restrictions.                                                                            | Optimizes peer-to-peer connection setup by assisting in identifying network conditions and allowing for direct communication between players.                                       |
| **WebRTC Applications**                       | Relays audio, video, and data between browsers when direct connections are not possible.                                                                                            | Gathers ICE candidates, including public IP addresses and port information, to optimize the setup of direct connections between browsers.                                       |
| **Remote Desktop Applications**               | Relays screen sharing data between devices when direct connections cannot be established due to NAT or firewall configurations.                                                       | Assists in determining public IP addresses and NAT types of devices to optimize the setup of direct connections for efficient remote desktop access.                                  |
| **Peer-to-Peer File Sharing Applications**     | Facilitates the transfer of files between users by relaying data through the TURN server when direct connections are challenging.                                                     | Aids in determining network conditions to enable direct peer-to-peer connections, optimizing the efficiency of file transfers between devices.                                       |
| **Instant Messaging and Chat Applications**   | Ensures reliable communication by relaying text and multimedia messages through the TURN server when direct connections are restricted by NAT or firewalls.                           | Helps devices discover network information to establish direct connections, reducing latency in instant messaging and chat applications.                                            |


# TURN and STUN working together

TURN (Traversal Using Relays around NAT) and STUN (Session Traversal
Utilities for NAT) are often used together to address the challenges
posed by Network Address Translation (NAT) in real-time communication
applications. While they serve distinct purposes, their combination
enhances the overall effectiveness of NAT traversal.

Here's how TURN and STUN can work together:

1. **STUN for Network Information:**
	 - STUN is typically used first in the communication setup. Devices
		 behind NAT or firewalls send STUN requests to a STUN server to
		 discover information about their network environment.
	 - The STUN server responds with details such as the public IP address
		 and NAT type of the device.

2. **Attempt Direct Connections with STUN Information:**
	 - With the information obtained from the STUN server, devices attempt
		 to establish direct peer-to-peer connections whenever possible.
		 This is based on the public IP address and NAT type information
		 provided by STUN.

3. **Fallback to TURN when Direct Connections Fail:**
	 - If direct connections are not possible due to strict NAT
		 configurations or firewall restrictions, devices can then fall back
		 to using TURN.
	 - TURN servers act as relays, facilitating the exchange of data
		 between devices that cannot establish direct connections.

4. **Dynamic Adaptation:**
	 - The combination of STUN and TURN allows for dynamic adaptation to
		 varying network conditions. When direct connections are feasible,
		 the communication can occur directly, reducing latency and
		 improving efficiency. In cases where direct connections are not
		 possible, TURN ensures reliable communication through relaying.

5. **Comprehensive NAT Traversal:**
	 - By combining STUN and TURN, applications can achieve comprehensive
		 NAT traversal. This approach optimizes communication paths by
		 attempting direct connections first and resorting to relaying
		 through a TURN server when necessary.

**The following diagram shows how TURN and STUN works together**

```
  +------------+                 +------------+                +------------+
  |  Client A  |                 |  STUN      |                |  TURN      |
  |  Behind    |  STUN Request   |  Server    |                |  Server    |
  |  NAT/FW    +---------------->+            +---------------->            |
  +------------+    Public IP    |            |    Public IP   |            |
                                 |            |                |            |
  +------------+                 |            |                |            |
  |  Client B  |                 |            |                |            |
  |  Behind    |                 |            |                |            |
  |  NAT/FW    |<----------------+            |<---------------+            |
  +------------+    Public IP    +------------+    Public IP   +------------+
```

1. **Client A and Client B:** Two clients are behind Network Address
	 Translation (NAT) or firewalls, and they initiate communication.

2. **STUN Server:** Both Client A and Client B send STUN requests to a
	 STUN server to discover their public IP addresses and NAT types.

3. **Direct Connection Attempt:** Clients use the information obtained
	 from the STUN server to attempt a direct peer-to-peer connection. If
	 successful, data is exchanged directly between the clients.

4. **Fallback to TURN:** If direct connections are not possible due to
	 restrictive network conditions, clients fall back to using a TURN
	 server.

5. **TURN Server Relaying Data:** The TURN server acts as a relay,
	 receiving data from one client and forwarding it to the other,
	 ensuring communication even in cases where direct connections are not
	 feasible.


> **Note:** 
>
> WebRTC, which stands for Web Real-Time Communication, is an
> open-source project that enables real-time communication directly
> between web browsers and applications. It provides a set of APIs
> (Application Programming Interfaces) and protocols that allow
> developers to integrate voice calling, video chat, and file sharing
> functionalities directly into web applications without the need for
> third-party plugins or additional software installations. WebRTC is
> supported by major web browsers, including Google Chrome, Mozilla
> Firefox, Microsoft Edge, and Safari.
> 
> Here are the key components and aspects of WebRTC:
> 
> 1. **MediaStream API:**
>    - WebRTC introduces the `MediaStream` API, which allows access to
>      the user's camera and microphone. This API enables the capture of
>      audio and video streams for real-time communication.
> 
> 2. **RTCPeerConnection:**
>    - The `RTCPeerConnection` API is a core component of WebRTC and is
>      responsible for managing the connection between peers (users or
>      devices). It handles the setup, negotiation, and maintenance of
>      the real-time communication session.
> 
> 3. **getUserMedia:**
>    - The `getUserMedia` API is used to request access to the user's
>      camera and microphone. It provides a way for web applications to
>      capture audio and video streams from the user's device.
> 
> 4. **RTCDataChannel:**
>    - Besides audio and video communication, WebRTC supports data
>      channels through the `RTCDataChannel` API. This allows
>      applications to establish peer-to-peer data connections for
>      exchanging arbitrary data between clients.
> 
> 5. **Signaling:**
>    - WebRTC does not define a signaling protocol for communication
>      negotiation (exchange of session information). Developers need to
>      implement their signaling mechanism for this purpose. Signaling
>      is crucial for coordinating the establishment and termination of
>      connections, exchanging session descriptions, and handling other
>      metadata.
> 
> 6. **ICE (Interactive Connectivity Establishment):**
>    - WebRTC uses ICE to address NAT traversal challenges. It enables
>      the discovery of the most efficient communication path between
>      peers, taking into account firewalls and NAT devices. ICE uses
>      STUN (Session Traversal Utilities for NAT) and TURN (Traversal
>      Using Relays around NAT) servers for this purpose.
> 
> 7. **WebRTC APIs in JavaScript:**
>    - WebRTC APIs are accessible through JavaScript in the web browser.
>      Developers use these APIs to set up and manage real-time
>      communication sessions. Key methods include `getUserMedia` for
>      accessing the user's media devices, `RTCPeerConnection` for
>      managing the connection, and `RTCDataChannel` for setting up data
>      channels.
> 
> 8. **Security:**
>    - WebRTC prioritizes security and privacy. It encrypts media
>      streams using protocols like SRTP (Secure Real-time Transport
>      Protocol) for audio and video encryption and DTLS (Datagram
>      Transport Layer Security) for key exchange.
> 
> 9. **Cross-Browser Compatibility:**
>    - WebRTC is supported by multiple web browsers, ensuring
>      cross-browser compatibility. This allows developers to create web
>      applications that can offer real-time communication experiences
>      to a broad user base.
> 
> 10. **Use Cases:**
>     - WebRTC is widely used in various applications, including video
>       conferencing platforms, online gaming with real-time
>       communication features, remote collaboration tools, live
>       streaming, and more.
> 
> WebRTC has significantly contributed to the democratization of
> real-time communication on the web by providing a standardized and
> accessible set of tools for developers. Its ease of use and
> cross-browser compatibility make it a popular choice for building
> applications that require audio, video, and data communication
> directly within the web browser.
