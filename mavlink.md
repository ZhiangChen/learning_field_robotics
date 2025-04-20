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
│   Publisher  │ ───MAVLink Data───► │  Subscriber  │
│              │                      │              │
└──────────────┘                      └──────────────┘
```

## Prerequisites

- Python 3.6+
- OpenCV (`cv2`)
- PyMAVLink
- NumPy

## Installation

1. Clone this repository
```bash
git clone https://github.com/yourusername/mavlink-image-transfer.git
cd mavlink-image-transfer
```

2. Install the required dependencies
```bash
pip install pymavlink opencv-python numpy
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
- Transmission speed depends on the bandwidth of your MAVLink connection

## Troubleshooting

Common issues:

1. **No connection**: Ensure both publisher and subscriber have correct connection strings
2. **Missing packets**: Check for interference or weak signal in wireless connections
3. **Image decoding errors**: Verify the image format is supported by OpenCV

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

This project is licensed under the MIT License - see the LICENSE file for details.
