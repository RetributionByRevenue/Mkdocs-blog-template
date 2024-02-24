---
slug: auto-sync-zeit-epub-to-your-tolino
description: Overcoming Challenges - Migrating 100TB Production Data from Windows Server 2008 to Microsoft Azure with Backblaze B2 CLI
comments: false
date: 2022-02-22
authors:
  - fgebhart
tags:
  - Python
  - Windows Server 2008
  - BackBlaze B2

glightbox: false
categories:
  - Python
---

## Migrating 100TB of Prod Data from Windows Server 2008 to Microsoft Azure with Backblaze B2 CLI

As businesses increasingly embrace cloud solutions, the need to migrate large volumes of production data from on-premises servers to cloud platforms becomes a critical task. This blog post will talk about the problems that came up when moving over 100TB of data from a physical server running Windows Server 2008 to Microsoft Azure, and how the Backblaze B2 CLI emerged as the only reliable and cost-effective solution when working with Windows Server 2008.

![IMAGE ALT TEXT HERE](https://www.apps4rent.com/blog/wp-content/uploads/2020/03/Migrating-Windows-Server-2008-to-Azure.png)<br>


<!-- more -->

## The Initial Struggle:

Attempting to use traditional methods such as robocopy and copy to transfer the massive dataset led to frustrating roadblocks. Hours into the transfer process, these methods would result in random RAM errors, throwing obscure error codes that hindered progress. Because of this, I had to look for another way to handle the amount of data the business needed to move without these problems.


## Discovering Backblaze B2 CLI:

During the exploration for a robust migration tool, I recalled my prior positive experience with Backblaze B2. Having previously utilized B2 for its simplicity and reliability, it stood out as a preferred choice. Surprisingly, many mainstream file transfer services lacked support for Windows Server 2008, including Azure, Amazon S3, and other storage providers, whose Windows binaries were incompatible with the aging server. It was only when I stumbled upon the Backblaze B2 CLI's GitHub page that I recognized a potential pathway to make it work on Server 2008. This unexpected finding made me want to learn more, and that's when I saw Backblaze B2's hidden but promising potential to help me deal with the unique problems that the migration brought up.<br>
![IMAGE ALT TEXT HERE](https://i0.wp.com/opendedup.org/odd/wp-content/uploads/2018/04/b2-logo-polo-e1522811818753.png?fit=240%2C252&ssl=1)

## Python 3.8 Requirement and Thonny:
To use Backblaze B2 CLI, Python 3.8 was a prerequisite. Luckily, i remembered that one of my favourite Python IDE's, Thonny, provided a simple download link to the last supported version of Python for Windows 7. (Server 2008 and windows 7 share the same architecture, so i knew it would be installable in server 2008 and i was correct). <br>
![IMAGE ALT TEXT HERE](https://thonny.org/img/screenshot.png)<br>
<a href="https://thonny.org/">Thonny Website</a>

### Thonny Shell:

One of the quality of life features about Thonny is the Thonny terminal. After opening the apllication, heading to tools -> Open system Shell provides access to a command-line shell within the Thonny environment.<br>
![IMAGE ALT TEXT HERE](https://tabreturn.github.io/img/tap5/p5-pip.png)

When you open the system shell in Thonny, it inherits the environment variables and configurations set by Thonny, including the paths to the Python interpreter and pip. This ensures that you can directly run Python commands and use pip to install packages without having to manually configure the environment.

Simply typing: <span style="background-color:darkgoldenrod">pip intall b2</span><br>backblaze b2 cli is able to be installed on your windows server 2008 machine.  


## Configure and Use B2 Cli:
[![IMAGE ALT TEXT HERE](https://img.youtube.com/vi/YvaKymlOEWM/0.jpg)](https://www.youtube.com/watch?v=YvaKymlOEWM)<br>
This guide i followed to set up my b2 cli and copy the files. It is a very simple process. 

## B2 to Azure Migration:
Now that the data has been successfully migrated into B2 cloud, we can power off the 2008 machine. To migrate data seamlessly from a Backblaze B2 bucket to Azure Blob Storage, using the AzCopy tool proves to be an efficient solution. Given that B2 is built upon the S3 protocol, compatibility with AzCopy, which supports both S3 and Azure Blob Storage, facilitates a smooth transition. Begin by configuring AzCopy with the necessary credentials for both the source B2 bucket and the destination Azure Blob Storage account. This involves obtaining the appropriate access keys or other authentication details. Once configured, employ AzCopy's copy command to initiate the migration, specifying the source B2 bucket URL and the destination Azure Blob Storage URL. AzCopy efficiently handles the transfer of files, ensuring that data integrity is maintained throughout the process. With its straightforward command-line interface and robust capabilities, AzCopy streamlines the migration from Backblaze B2 to Azure Blob Storage, offering a reliable and efficient solution for cloud data transfer.
<br>
<code>
azcopy copy "https://B2BucketName/Path" "https://AzureStorageAccountName.blob.core.windows.net/ContainerName/Path" --recursive --from-to=BlobLocal
</code>
## Why B2 CLI Succeeded:
One notable reason for the success of Backblaze B2 CLI was its avoidance of the Windows API for copying. The Windows API, known for its instability during large-scale data transfers, had been a source of frustration with earlier methods. B2 CLI, using its own robust mechanisms, proved to be a stable alternative that ensured the integrity of the data throughout the migration.

## Conclusion:
The migration of large datasets can be a daunting task, especially when faced with the limitations of legacy systems. The experience outlined here highlights the importance of choosing tools that are not only scalable but also adaptable to the unique challenges posed by the migration process. In this case, Backblaze B2 CLI emerged as a reliable solution, showcasing the value of open-source tools and the need for flexibility in the face of complex migration scenarios.