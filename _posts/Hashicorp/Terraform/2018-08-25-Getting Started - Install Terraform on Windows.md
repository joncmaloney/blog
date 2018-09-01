---
layout: post
title: Getting Started - Installing Terraform on windows
author: jon_maloney
summary: TBD
---
Source: https://www.vasos-koupparis.com/terraform-getting-started-install/

## Download Terraform

You can  [download a version of Terraform](https://releases.hashicorp.com/terraform/?_ga=2.17493689.1489308342.1524687882-1109174367.1520536284) from the releases service.

## Install Terraform – Windows

1. Download terraform for windows 
   - Note: Terraform is packaged as a zip archive, so after downloading Terraform, unzip the package. Terraform runs as a single binary named terraform. Any other files in the package can be safely removed and Terraform will still function
2. Copy files from the zip to “c:\terraform” for example. That’s our terraform **PATH.**
3. The final step is to make sure that the terraform binary is available on the **PATH**.



#### General Information

- The **PATH** is the system variable that your operating system uses to locate needed executables from the command line or Terminal window.
- The **PATH** system variable can be set using **System Utility** in control panel on Windows, or in your shell’s startup file on Linux.

### Windows 10 and Windows 8

1. In Search, search for and then select: System (Control Panel)
2. Click the **System and Security** link.
3. Click the **System** link.
4. Click the **Advanced system settings** link.
5. Click **Environment Variables**. In the section **System Variables**, find the **PATH** environment variable and select it. Click **Edit**. If the **PATH** environment variable does not exist, click **New**.
6. In the **Edit System Variable** (or **New System Variable**) window, append at the end of the **PATH** environment variable the value of terraform path ex.”c:\terraform;” . Click **OK**. Close all remaining windows by clicking **OK**.
7. Reopen Command prompt window, and run terraform.



