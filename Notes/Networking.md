# 1. Network

-   A **network** is a group of interconnected nodes or computing
    devices that exchange data and resources with each other.

------------------------------------------------------------------------

## 2. IP Address

-   An **IP Address** is a unique identifier assigned to a device on a
    network.
-   Two main types:
    -   **Public IP Address**: Used to communicate over the internet.
    -   **Private IP Address**: Used within local/private networks.
        -   Common private IP ranges:
            -   `192.168.0.0 – 192.168.255.255`\
            -   `10.0.0.0 – 10.255.255.255`\
            -   `172.16.0.0 – 172.31.255.255`

⚡ **Note:** Private IPs cannot directly communicate with the internet.\
They are translated into Public IPs using **NAT (Network Address
Translation)**.

------------------------------------------------------------------------

## 3. DHCP (Dynamic Host Configuration Protocol)

-   Automatically assigns IP addresses to devices on a network.\
-   Devices get a **new IP address** each time they connect, instead of
    using a fixed one.\
-   Mostly used in **LANs** and **private networks**.

------------------------------------------------------------------------

## 4. TCP Three-Way Handshake

Used to establish a reliable TCP connection between client and server.

1.  **SYN** → Client: "Hello, I want to connect."\
2.  **SYN/ACK** → Server: "I am ready to connect."\
3.  **ACK** → Client: "Great, let's start communication."

✅ Now, the connection is established.

------------------------------------------------------------------------

## 5. TCP vs UDP

-   **TCP (Transmission Control Protocol):**
    -   Reliable, connection-oriented.
    -   Uses handshake mechanism.\
    -   Ensures data delivery in order.
-   **UDP (User Datagram Protocol):**
    -   Connectionless, fast.\
    -   Does **not** guarantee delivery/order.\
    -   Used in streaming, gaming, VoIP.

------------------------------------------------------------------------

## 6. DNS (Domain Name System)

-   Converts **domain names** (e.g., `google.com`) into **IP addresses**
    that machines can understand.\
-   Works like the "phonebook of the internet."

------------------------------------------------------------------------

## 7. Common Protocols

-   **FTP (File Transfer Protocol):**
    -   Transfers files between client and server.
-   **SMB (Server Message Block):**
    -   Allows file and resource sharing (e.g., printers) between
        computers.
-   **SMTP (Simple Mail Transfer Protocol):**
    -   Sends emails from one server to another.
-   **Telnet:**
    -   Provides remote access to another computer (less secure,
        replaced by SSH).

------------------------------------------------------------------------