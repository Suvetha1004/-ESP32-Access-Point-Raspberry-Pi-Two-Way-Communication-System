ESP32 Access Point & Raspberry Pi Two-Way Communication System

 Project Overview
This project demonstrates a real-time two-way communication system using an ESP32 as a WiFi Access Point and Raspberry Pi as a server. A laptop acts as a client to exchange messages using TCP socket programming without internet connection.

 Objectives
- Implement a WiFi Access Point using ESP32
- Establish real-time two-way communication between Laptop and Raspberry Pi
- Demonstrate message sending and receiving
- Prepare for IoT communication demonstration

 Hardware Used
- ESP32 (WiFi Access Point)
- Raspberry Pi
- Laptop
- USB Cable / Power Supply

 Software Used
- Arduino IDE
- Python 3.x
- VS Code / Notepad
- Serial Monitor

 Network Architecture
Laptop and Raspberry Pi connect to the ESP32 Access Point and communicate through TCP sockets using a specific port.

ESP32 Access Point Code

#include <WiFi.h>

const char* ssid = "ESP32_Network";
const char* password = "12345678";

void setup() {
  Serial.begin(115200);
  WiFi.softAP(ssid, password);

  Serial.println("ESP32 Access Point Started");
  Serial.print("IP Address: ");
  Serial.println(WiFi.softAPIP());
}

void loop() {}
Raspberry Pi Server (Python):

import socket

host = "0.0.0.0"
port = 5000

server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server.bind((host, port))
server.listen(1)

print("Waiting for connection...")

conn, addr = server.accept()
print("Connected by", addr)

while True:
    data = conn.recv(1024)
    if not data:
        break
    print("Received:", data.decode())
Laptop Client Code:
import socket
import threading

host = "192.168.4.3"
port = 5000

client = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
client.connect((host, port))

print("Connected to Raspberry Pi")

def receive():
    while True:
        data = client.recv(1024).decode()
        if data.lower() == "exit":
            break
        print("Raspberry Pi:", data)

def send():
    while True:
        msg = input("You: ")
        client.send(msg.encode())
        if msg.lower() == "exit":
            break

threading.Thread(target=receive).start()
threading.Thread(target=send).start()

Working Procedure:

Power ON ESP32, Raspberry Pi and Laptop.

ESP32 creates a WiFi Access Point.

Connect Raspberry Pi and Laptop to the ESP32 network.

Run the server program on Raspberry Pi.

Run the client program on the laptop.

Messages can now be sent and received between devices.

Expected Output:

Laptop sends message

Raspberry Pi receives message

Two-way communication established

Result:

The ESP32 successfully created a WiFi Access Point and both Raspberry Pi and laptop connected to the network. TCP socket communication was established enabling real-time message exchange between devices.
