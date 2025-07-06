# 🌊 Torrentio - Mini BitTorrent Implementation

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![C++](https://img.shields.io/badge/C++-17-blue.svg)](https://isocpp.org/)
[![Platform](https://img.shields.io/badge/Platform-Linux-green.svg)](https://www.linux.org/)
[![OpenSSL](https://img.shields.io/badge/OpenSSL-Required-orange.svg)](https://www.openssl.org/)

A lightweight, educational implementation of the BitTorrent protocol in C++. This project demonstrates peer-to-peer file sharing concepts with a tracker-based architecture.

## 📋 Table of Contents

- [Features](#-features)
- [Architecture](#-architecture)
- [Prerequisites](#-prerequisites)
- [Installation](#-installation)
- [Usage](#-usage)
- [API Reference](#-api-reference)
- [Project Structure](#-project-structure)
- [Contributing](#-contributing)
- [License](#-license)

## ✨ Features

- 🔄 **Peer-to-Peer File Sharing**: Share and download files across the network
- 📡 **Tracker-Based Architecture**: Centralized tracker for peer discovery
- 🔐 **SHA-1 Hashing**: Secure file integrity verification
- 🧵 **Multi-threaded**: Concurrent handling of multiple connections
- 📁 **Torrent File Generation**: Create `.mtorrent` metadata files
- 🔍 **Download Status Tracking**: Monitor ongoing downloads
- 🗑️ **Seeder Management**: Add/remove files from the seeder list

## 🏗️ Architecture

```
┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│   Client 1  │    │   Client 2  │    │   Client N  │
│             │    │             │    │             │
│ • Share     │    │ • Download  │    │ • Seed      │
│ • Download  │    │ • Upload    │    │ • Remove    │
└─────────────┘    └─────────────┘    └─────────────┘
       │                   │                   │
       └───────────────────┼───────────────────┘
                           │
                    ┌─────────────┐
                    │   Tracker   │
                    │             │
                    │ • Peer List │
                    │ • Metadata  │
                    │ • Discovery │
                    └─────────────┘
```

## 📦 Prerequisites

### System Requirements
- **Operating System**: Linux (Ubuntu/Debian recommended)
- **Compiler**: GCC with C++17 support
- **Libraries**: OpenSSL development libraries

### Dependencies Installation

```bash
# Ubuntu/Debian
sudo apt update
sudo apt install build-essential openssl libssl-dev

# CentOS/RHEL/Fedora
sudo yum install gcc-c++ openssl-devel
# or
sudo dnf install gcc-c++ openssl-devel
```

## 🚀 Installation

### 1. Clone the Repository
```bash
git clone <repository-url>
cd Torrentio
```

### 2. Compile the Project

#### Compile Tracker
```bash
g++ -o tracker tracker.cpp -lpthread
```

#### Compile Client
```bash
g++ -o client client.cpp -lpthread -lssl -lcrypto
```

### 3. Verify Installation
```bash
# Check if executables were created
ls -la tracker client
```

## 💻 Usage

### Starting the Tracker Server

The tracker coordinates peer discovery and file sharing across the network.

```bash
./tracker <tracker_ip>:<tracker_port> <backup_tracker_ip>:<backup_tracker_port> <seederlist_file>
```

**Example:**
```bash
./tracker 127.0.0.1:8080 127.0.0.1:8081 seederlist
```

### Starting the Client

The client handles file sharing, downloading, and peer connections.

```bash
./client <client_ip>:<upload_port> <tracker_ip_1>:<tracker_port_1> <tracker_ip_2>:<tracker_port_2>
```

**Example:**
```bash
./client 127.0.0.1:9000 127.0.0.1:8080 127.0.0.1:8081
```

## 🔧 API Reference

### Client Commands

Once the client is running, you can use these interactive commands:

#### 📤 Share a File
Share a local file and create a torrent metadata file.

```bash
share <local_file_path> <filename>.<extension>.mtorrent
```

**Example:**
```bash
share /path/to/image.jpg image.jpg.mtorrent
```

#### 📥 Download a File
Download a file using its torrent metadata.

```bash
get <path_to_mtorrent_file> <destination_path>
```

**Example:**
```bash
get image.jpg.mtorrent /downloads/image.jpg
```

#### 📊 Show Downloads
Display the status of all downloads.

```bash
show_downloads
```

#### 🗑️ Remove from Seeder List
Remove a file from the seeder list (stop sharing).

```bash
remove <filename.mtorrent>
```

**Example:**
```bash
remove image.jpg.mtorrent
```

#### 🔚 Close Client
Gracefully close the client connection.

```bash
close
```

## 📁 Project Structure

```
Torrentio/
├── client.cpp          # Main client implementation
├── tracker.cpp         # Tracker server implementation
├── socket.cpp          # Socket utility functions
├── trackerheader.h     # Header file for tracker
├── client              # Compiled client executable
├── tracker             # Compiled tracker executable
├── seederlist          # File containing seeder information
├── *.mtorrent          # Torrent metadata files
├── logclient*          # Client log files
├── logtracker          # Tracker log file
└── README.md           # This file
```

## 🔍 File Formats

### .mtorrent File Structure
```
<tracker_ip_1>:<tracker_port_1>
<tracker_ip_2>:<tracker_port_2>
<original_file_path>
<file_size_in_bytes>
<file_hash>
```

### Seeder List Format
```
<file_hash> <client_ip>:<client_port> <file_path>
```

## 🛠️ Development

### Building from Source
```bash
# Clean build
make clean
make all

# Or compile individually
make tracker
make client
```

### Debug Mode
```bash
g++ -g -o tracker tracker.cpp -lpthread
g++ -g -o client client.cpp -lpthread -lssl -lcrypto
```

## 🐛 Troubleshooting

### Common Issues

1. **Port Already in Use**
   ```bash
   # Find process using port
   sudo netstat -tulpn | grep :8080
   # Kill process
   sudo kill -9 <PID>
   ```

2. **Permission Denied**
   ```bash
   # Make executables
   chmod +x tracker client
   ```

3. **OpenSSL Not Found**
   ```bash
   # Install OpenSSL development libraries
   sudo apt install libssl-dev
   ```

### Log Files
- `logtracker`: Tracker server logs
- `logclient1`, `logclient2`: Client operation logs

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

### Development Guidelines
- Follow C++ best practices
- Add comments for complex logic
- Test thoroughly before submitting
- Update documentation for new features

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

```
MIT License

Copyright (c) 2024 Dikshant Rajput

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

## 🙏 Acknowledgments

- Original BitTorrent protocol creators
- OpenSSL project for cryptographic functions
- The open-source community for inspiration and tools

---

⭐ **Star this repository if you found it helpful!**

📧 **Questions?** Open an issue or reach out to the maintainers.
