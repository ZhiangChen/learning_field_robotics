# MAVLink Image Transfer System

A Python-based system for transferring images over MAVLink protocol, commonly used in drone and robotic systems.

## Overview

This project provides a simple implementation for sending and receiving images over MAVLink communication channels. It consists of two main components:

- **Publisher**: Sends images from a local file to a MAVLink endpoint
- **Subscriber**: Receives and displays images from a MAVLink stream

The system uses MAVLink's encapsulated data protocol to break down images into smaller chunks for transmission, making it suitable for bandwidth-constrained communication channels.

## System Architecture

```
┌──────────────┐                      ┌──────────────┐
│              │                      │              │
│    Image     │                      │    Image     │
│   Publisher  │ ───MAVLink Data───►  │  Subscriber  │
│              │                      │              │
└──────────────┘                      └──────────────┘
```

## Software

- Python 3.6+
- OpenCV (`cv2`)
- PyMAVLink
- NumPy

## Operation

1. Install the required dependencies
```bash
pip install pymavlink opencv-python numpy
```

2. Publisher Code
```
import time
import sys
import argparse
import cv2
from pymavlink import mavutil


def setup_publisher(connection_string):
    try:
        connection = mavutil.mavlink_connection(connection_string, source_system=1)
        print(f"Publisher connected to {connection_string}")
        return connection
    except Exception as e:
        print(f"Error setting up publisher connection: {e}")
        sys.exit(1)


def send_image(connection, image_path):
    img = cv2.imread(image_path)
    _, img_encoded = cv2.imencode('.jpg', img)
    img_bytes = img_encoded.tobytes()
    size = len(img_bytes)
    chunk_size = 253
    packets = (size + chunk_size - 1) // chunk_size

    connection.mav.data_transmission_handshake_send(
        1,  # type: JPEG
        size,
        img.shape[1],
        img.shape[0],
        packets,
        chunk_size,
        100
    )

    for i in range(packets):
        chunk = img_bytes[i*chunk_size:(i+1)*chunk_size]
        connection.mav.encapsulated_data_send(i, chunk.ljust(253, b'\0'))
        print(f"Sent packet {i+1}/{packets}")

    print("Image sent")


def main():
    parser = argparse.ArgumentParser(description='MAVLink Image Publisher')
    parser.add_argument('--connect', default='udpout:localhost:14550', help='Connection string')
    parser.add_argument('--image', required=True, help='Path to image file')
    args = parser.parse_args()

    connection = setup_publisher(args.connect)
    connection.mav.heartbeat_send(
        mavutil.mavlink.MAV_TYPE_GCS,
        mavutil.mavlink.MAV_AUTOPILOT_INVALID,
        0, 0, 0
    )

    send_image(connection, args.image)

if __name__ == "__main__":
    main()

```

3. Subscriber Code
```
import time
import sys
import argparse
from pymavlink import mavutil
import numpy as np
import cv2


def setup_subscriber(connection_string, wait_heartbeat=True):
    try:
        connection = mavutil.mavlink_connection(connection_string)
        if wait_heartbeat:
            print(f"Waiting for heartbeat from {connection_string}")
            connection.wait_heartbeat()
            print("Heartbeat received!")
        print(f"Subscriber connected to {connection_string}")
        return connection
    except Exception as e:
        print(f"Error setting up subscriber connection: {e}")
        sys.exit(1)


def receive_image(connection):
    print("Waiting for DATA_TRANSMISSION_HANDSHAKE...")
    while True:
        msg = connection.recv_match(type='DATA_TRANSMISSION_HANDSHAKE', blocking=True, timeout=10)
        if msg:
            break

    size = msg.size
    packets = msg.packets
    payload = b''

    print(f"Receiving {packets} packets...")
    for _ in range(packets):
        packet = connection.recv_match(type='ENCAPSULATED_DATA', blocking=True, timeout=10)
        if packet:
            payload += bytes(packet.data)

    img_array = np.frombuffer(payload[:size], dtype=np.uint8)
    img = cv2.imdecode(img_array, cv2.IMREAD_COLOR)

    if img is not None:
        cv2.imshow("Received Image", img)
        cv2.waitKey(0)
        cv2.destroyAllWindows()
        print("Image displayed.")
    else:
        print("Failed to decode image")


def main():
    parser = argparse.ArgumentParser(description='MAVLink Image Subscriber')
    parser.add_argument('--connect', default='udpin:localhost:14550', help='Connection string')
    args = parser.parse_args()

    connection = setup_subscriber(args.connect)
    receive_image(connection)

if __name__ == "__main__":
    main()

```

## Usage

### Image Publisher

The publisher script takes an image file and sends it over a MAVLink connection:

```bash
python image_publisher.py --connect udpout:localhost:14550 --image path/to/your/image.jpg
```

Parameters:
- `--connect`: MAVLink connection string (default: `udpout:localhost:14550`)
- `--image`: Path to the image file to be sent (required)

### Image Subscriber

The subscriber script listens for incoming images on a MAVLink connection:

```bash
python image_subscriber.py --connect udpin:localhost:14550
```

Parameters:
- `--connect`: MAVLink connection string (default: `udpin:localhost:14550`)

When an image is received, it will be displayed in a window.

## How It Works

### Publisher Operation

1. Connects to the specified MAVLink endpoint
2. Reads the input image using OpenCV
3. Encodes the image as JPEG
4. Sends a `DATA_TRANSMISSION_HANDSHAKE` message to initialize the transfer
5. Breaks the image into chunks and sends each as an `ENCAPSULATED_DATA` message

### Subscriber Operation

1. Connects to the specified MAVLink endpoint
2. Waits for a `DATA_TRANSMISSION_HANDSHAKE` message
3. Receives all expected `ENCAPSULATED_DATA` packets
4. Reassembles the complete image from the received chunks
5. Decodes and displays the image using OpenCV

## Example Use Cases

- Live camera feed from drones to ground control stations
- Transmission of inspection images from remote robots
- Visual data sharing between autonomous vehicles

## Advanced Configuration

### Connection Strings

MAVLink supports various connection types:

- UDP: `udpin:` or `udpout:` (e.g., `udpout:192.168.1.1:14550`)
- TCP: `tcp:` (e.g., `tcp:localhost:5760`)
- Serial: `serial:` (e.g., `serial:/dev/ttyUSB0:57600`)

### Performance Considerations

- The current implementation uses a fixed chunk size of 253 bytes
- For larger images or video streams, consider adjusting compression settings
- Transmission speed depends on the bandwidth of your MAVLink connection (Working on optimizing bandwidth use)

## Troubleshooting

Common issues:

1. **No connection**: Ensure both publisher and subscriber have correct connection strings
2. **Missing packets**: Check for interference or weak signal in wireless connections
3. **Image decoding errors**: Verify the image format is supported by OpenCV


