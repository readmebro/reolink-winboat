# Running Reolink App on Arch Linux with WinBoat

A guide to running the Reolink Windows client on Arch Linux using the dockur/windows Docker container.

## Overview

If you are like me you prefer to use Linux. However, Reolink refuses to make a native Linux client (Boo). This guide helps you run the Reolink camera management software on Arch Linux (or any Linux distro) by using a full Windows environment in WinBoat/Docker. This is particularly useful for Reolink cameras that don't support RTSP or browser-based access.

## Prerequisites

- Arch Linux (Other Distros will work as well)
- Docker installed and running
- 4GB of available RAM
- 32GB+ of free disk space for the Windows container

## Installation

### 1. Install Docker (if not already installed)

See [Docker Documentation](https://docs.docker.com/)

```bash
sudo pacman -S docker docker-compose freerdp
sudo systemctl enable --now docker.service
sudo docker info
sudo gpasswd -a $USER docker
```

**Reboot for group changes to take effect.**

### 2. Download the latest AppImage from WinBoat

Visit [https://www.winboat.app/](https://www.winboat.app/) and download the latest AppImage.

### 3. Install WinBoat

Right click the AppImage file, go to properties and give it execute permissions.

After confirming all the green check boxes are ok, install Windows.

**Note:** This will take a while as it has to pull Windows and install it.

### 4. Connect to Windows Container

Once installation is complete, connect to the Windows container through the WinBoat interface.

## Setting Up Reolink App

### 1. Install Visual C++ Redistributables (REQUIRED)

Inside the Windows container:

1. Open Edge or Firefox browser
2. Download Visual C++ 2015-2022 Redistributable (x64): [Visual C++](https://aka.ms/vs/17/release/vc_redist.x64.exe)
3. Run the installer
4. Restart if prompted

**This step is critical** - without it, you'll get "Dynamic Linking Error: Win32 error 126"

### 2. Download and Install Reolink Client

1. Visit [Reolink Downloads](https://reolink.com/software-and-manual/)
2. Download the Windows client
3. Run the installer as Administrator (right-click → Run as administrator)
4. Complete the installation wizard

### 3. Configure Reolink App

1. Launch the Reolink Client
2. Add your camera(s) using IP address or UID
3. Enter your camera credentials
4. Profit!

## Network Configuration

### Accessing Local Network Cameras

If your cameras aren't detected:

1. Check your camera IP is accessible from the host:

```bash
ping <camera-ip>
```

2. Try adding cameras manually by IP address if auto-discovery fails

## Troubleshooting

### Error: "A JavaScript error occurred in the main process"

<img width="619" height="120" alt="image" src="https://github.com/user-attachments/assets/a7dadcc1-c9dd-4474-9722-206b40d0520d" />



**Solution:** Install Visual C++ Redistributables (see step 1 above)

### Camera not detected on network

- Ensure camera and host are on same subnet
- Check firewall rules aren't blocking discovery
- Try adding camera manually by IP address instead of auto-discovery

### View Container Status

```bash
docker ps
```

## Performance Tips

- **Allocate adequate RAM:** Minimum 4GB, recommended 8GB
- **Use SSD storage:** Place the storage volume on an SSD
- **Close unnecessary programs:** Windows running in Docker is resource-intensive, debloat as needed

## Known Limitations

- Container requires significant disk space (32GB)
- High quality setting for video did not load sound, I had to switch to low

## Resources

- [docker/windows GitHub](https://github.com/dockur/windows)
- [Reolink Support](https://support.reolink.com/)
- [Docker Documentation](https://docs.docker.com/)

## Contributing

Found a better solution or improvement? Please submit a PR or open an issue!

## License

MIT License - Feel free to use and modify as needed.
