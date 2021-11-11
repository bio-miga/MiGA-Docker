# Installing Docker for Mac OS

Before installing Docker Desktop in your Mac computer, you must make sure that your system meets the minimum requirements outlined below. These requiremnets depend on the CPU installed in yor computer. Refer to the Docker installation instructions at https://docs.docker.com/desktop/mac/install/ if the instructions below still leave you wondering.

## Mac with Intel processor requirements:

* **OS Version:** Your MacOS version must be 10.14 or newer (that is, Mojave, Catalina, or MacOS Big Sur). Upgrading to the latest version of MacOS is recommended. If you experience any issues after upgrading your MacOS to version 10.15, you must install the latest version of Docker Desktop to be compatible with this version of MacOS.
* **RAM:** At least 4 GB of RAM.

**NOTE:** VirtualBox prior to version 4.3.30 must not be installed as it is not compatible with Docker Desktop.

## Mac with M1(Apple silicon chip) processor requirements:
* Rosetta 2 must be installed as a prerequisite. To install Rosetta 2 manually from the command line, run the following command from the terminal:

```
software update --install-rosetta
```

**NOTE:** Please refer to https://docs.docker.com/desktop/mac/apple-silicon/ for any  "known issues."

## Installing Docker

* Download Docker Desktop from https://desktop.docker.com/mac/stable/amd64/Docker.dmg?utm_source=docker&utm_medium=webreferral&utm_campaign=docs-driven-download-mac-amd64
* Double-click Docker.dmg to open the installer.
* Drag the Docker icon to the Applications folder.
* Double-click Docker in the Applications folder to start Docker.
* If a Docker Subscription Service window appears, click th checkbox to indicate that you accept the updated terms and then click Accept to continue.
 
## Starting Docker Desktop

* Launch Docker from the launchpad by selecting Docker. You should see a small Docker icon on the top right of the Apple menu. It will make a Building animation as Docker starts up. 
* If you select the Docker icon now, you can see that Docker Desktop is running in the first place of the menu bar.

Congratulations! You are now successfully running Docker Desktop. Click the Docker menu to see Preferences and other options. To run the Quick Start Guide on demand, select the Docker menu and then choose Quick Start Guide.

## Updates

* When an update is available, Docker Desktop displays an icon to indicate the availability of a newer version. You can choose when to start the download and installation process.
* Click Download update when you are ready to download the update. This downloads the update in the background. 
* After downloading the update, click Update and restart from the Docker menu. This installs the latest update and restarts Docker Desktop for the changes to take effect.
* When Docker Desktop starts, it displays the Docker Subscription Service Agreement window. 
* Click the checkbox to indicate that you accept the updated terms and then click Accept to continue.
* Docker Desktop starts after you accept the terms.

## Uninstall Docker Desktop
To uninstall Docker Desktop from your Mac:
* From the Docker menu, select Troubleshoot and then select Uninstall.
* Click Uninstall to confirm your selection.
