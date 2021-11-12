# Installing Docker for Windows

Before installing Docker Desktop in Windows, you must make sure that your computer and Windows version meet the minimum requirements outlined below. Refer to the Docker installation instructions at [https://docs.docker.com/docker-for-windows/install/](https://docs.docker.com/docker-for-windows/install/) if the instructions below still leave you wondering.

Minimum requirements:

* Processor: 64-bit processor with Second Level Address Translation \(SLAT\)
* RAM: A minimum of 4GB system RAM
* Windows 10 64-bit: Pro, Enterprise, or Education \(Build 16299 or later\)
* BIOS support for virtualization
  * Hyper-V and Containers Windows features must be enabled.

### Checking requirements

You can check for most of these requirements using the Windows System information app; just enter "system info" into the Windows search box.

All modern processors support SLAT. AMD has supported this technology with what it calls Rapid Virtualization Indexing \(RVI\) technology since the introduction of its third-generation Opteron processors \(code name Barcelona\). Intel's implementation of SLAT, known as Extended Page Table \(EPT\), was first introduced in the Nehalem microarchitecture found in certain Core i7, Core i5, and Core i3 processors. If you want to check, make a web search for your processor's specifications. Alternatively, you can use a tool available at [https://www.howtogeek.com/73318/how-to-check-if-your-cpu-supports-second-level-address-translation-slat/](https://www.howtogeek.com/73318/how-to-check-if-your-cpu-supports-second-level-address-translation-slat/).

### Enabling virtualization

By default, virtualization is probably not enabled on your computer. You can check by opening Task Manager and clicking on the Performance tab. Enabling virtualization means making changes to the BIOS settings. Exactly how to do this depends on the make and model of your computer, so for instructions it is best to make a web search using that information.

### Installing Docker Desktop

Download the stable version of Docker for Windows from [https://hub.docker.com/editions/community/docker-ce-desktop-windows/](https://hub.docker.com/editions/community/docker-ce-desktop-windows/) and follow the installation instructions at [https://docs.docker.com/docker-for-windows/install/](https://docs.docker.com/docker-for-windows/install/).

### Configuring Docker Desktop

Docker Desktop consumes a significant amount of computer resources, so you likely do not want it to start up each time you boot your computer. To prevent this from happening, type "startup" in the Windows search area in the lower left of your screen and open **Startup Apps**. Turn off Docker Desktop. Close the app window.

Create a folder for storing your MiGA-Web results. This does not have to be on your C drive. MiGA-Web will run somewhat faster if it is on a solid-state drive.

Start Docker Desktop. As it starts, the whale icon will appear among the hidden icons revealed by clicking on the **show hidden icons** tab in the lower right of your screen. The whale will spout while Docker is starting, which takes a minute or two. Once Docker is running, the spouting will stop and a message that Docker is running should appear next to the icon.

By default, Docker is set to use 2 CPUs and 2 GB of memory. You may view these settings by right clicking on the whale in the hidden icon box and clicking on **Settings** and then **Resources** in the menus that open. It is not necessary to increase these settings to install and run MiGA-Web, and in fact requesting too many CPUs will adversely affect performance and make the system unstable.

You do, however, need to enable **FILE SHARING**, so left click on that text. Click on the **blue plus sign** inside the circle next to the grayed-out line **C:\path\to\export\directory**, browse to the folder you created to hold your MiGA-Web results, and select the folder. Close the settings window and click on **Apply & Restart**.

You are now ready to install MiGA.

