# MiGA for Amazon Web Services

## Introduction
For those who have large MiGA jobs to run and no access to a computer cluster at their institution, Amazon Web Services (AWS) provides an answer. While one could install Docker for Linux in an AWS EC2 instance (think of an instance as your computer in the cloud)  and then the Docker version of MiGA, we offer a more efficient MiGA Amazon Machine Image (AMI) which provides both command line and web-based (MiGA-Web) functionality. We have also configured it differently so it is easier to transfer files to and from MiGA AWS and so data are stored on a separate volume than the program itself.

Having the data on a separate volume means that you can attach it to different MiGA instances. There are two reasons that you might want to do so: 
- An updated version of MiGA becomes available.
- You can create a large project from the command line with a more expensive instance type with lots of resources and browse the results with MiGA-Web using a cheaper instance type with fewer resources.

