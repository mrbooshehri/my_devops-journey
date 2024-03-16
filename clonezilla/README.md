# What is Clonezilla

Clonezilla is a free and open-source disk cloning, disk imaging, and
deployment tool. It is designed to save and restore the exact state of a
computer's hard drive, including the operating system, applications, and
data. Clonezilla is particularly useful for system administrators, IT
professionals, and users who need to clone or image their hard drives
for backup, recovery, or deployment purposes.

## Key Features of Clonezilla:

1. **Disk Cloning**: Clonezilla can clone an entire hard drive,
	 including the operating system, applications, and data, to another
	 hard drive. This is useful for duplicating a system setup or
	 migrating data to a new drive.

2. **Disk Imaging**: Clonezilla can create an image of a hard drive,
	 which can then be restored onto another drive. This is useful for
	 creating backups of a system or for deploying the same system
	 configuration across multiple machines.


> # Cloning vs Imaging
>
> Cloning and imaging are two methods used in data management and system
> administration to create copies of data or entire systems. While they
> share the goal of duplicating data or system configurations, they
> differ in their approach, scope, and the level of detail they capture.
> Here's a comparison of the two:
> 
> ## Cloning
> 
> **Definition**: Cloning refers to the process of creating an exact
> copy of a hard drive, including the operating system, applications,
> and data. The cloned drive is a complete replica of the original,
> capable of booting up and functioning independently.
> 
> **Scope**: Cloning focuses on duplicating the entire hard drive,
> including all partitions and the boot sector. This means that the
> cloned drive will have the same file system structure, partition
> layout, and boot configuration as the original.
> 
> **Use Cases**:
> - **System Backup**: Cloning is useful for creating a complete backup
>   of a system, allowing for easy recovery in case of data loss or
>   system failure.
> - **System Migration**: When moving to a new hard drive or computer,
>   cloning ensures that all data and system configurations are
>   preserved.
> - **Disk Replacement**: In cases where a hard drive needs to be
>   replaced, cloning allows for an easy transfer of all data and system
>   configurations to the new drive.
> 
> **Pros**:
> - **Simplicity**: Cloning is straightforward and does not require
>   detailed knowledge of the system's structure.
> - **Complete Replication**: It creates a perfect copy of the original
>   drive, including the operating system and all data.
> 
> **Cons**:
> - **Size Limitation**: The cloned drive must be at least as large as
>   the original drive, which can be a limitation if the original drive
>   is large.
> - **Less Flexibility**: Since it duplicates the entire drive, it's
>   less flexible than imaging in terms of customization or selective
>   restoration.
> 
> ## Imaging
> 
> **Definition**: Imaging involves creating a digital copy of the data
> on a hard drive, which can then be saved to an external storage device
> or transferred over a network. This copy, known as an image, can be
> restored onto a hard drive, effectively duplicating the original data
> and system configuration.
> 
> **Scope**: Imaging focuses on the data and system configuration rather
> than the physical structure of the hard drive. This means that the
> image can be restored onto a different drive with a different size or
> file system, as long as the destination drive is large enough to
> accommodate the image.
> 
> **Use Cases**:
> - **System Backup**: Imaging allows for the creation of a backup that
>   can be stored off-site or on a different drive, providing an
>   additional layer of data protection.
> - **System Deployment**: Imaging is useful for deploying the same
>   system configuration across multiple machines, ensuring consistency
>   and reducing setup time.
> - **Data Migration**: Users can use imaging to move their data to a
>   new hard drive or computer, preserving their applications and
>   settings.
> 
> **Pros**:
> - **Flexibility**: Images can be restored onto different types of
>   drives and file systems, providing more flexibility than cloning.
> - **Size Efficiency**: Images are often smaller than the original
>   data, as they do not include empty space on the drive.
> 
> **Cons**:
> - **Complexity**: Imaging requires a more detailed understanding of
>   the system's structure and the process of creating and restoring
>   images.
> - **Potential for Data Loss**: If the image is not properly created or
>   restored, there is a risk of data loss or system failure.
> 
> ## Conclusion
> 
> Both cloning and imaging serve the purpose of duplicating data or
> system configurations, but they do so in different ways. Cloning is
> more straightforward and creates a complete copy of the entire hard
> drive, making it ideal for situations where the exact replication of
> the original drive is required. Imaging, on the other hand, offers
> more flexibility and efficiency, allowing for the duplication of data
> and system configurations without the need for a drive of the same
> size and structure.

3. **Deployment**: Clonezilla supports the deployment of a system image
	 to multiple computers simultaneously. This is particularly useful in
	 large organizations where the same system configuration needs to be
	 deployed across many machines.

4. **Partition and Filesystem Support**: Clonezilla supports a wide
	 range of file systems and partition types, making it versatile for
	 different types of storage devices and configurations.

5. **Compression**: Clonezilla can compress disk images during the
	 imaging process, which can save significant storage space.

6. **Encryption**: Clonezilla supports disk encryption, allowing for the
	 secure storage and transfer of disk images.

7. **Multi-Boot Support**: Clonezilla can clone and image multiple
	 bootable partitions, making it suitable for systems with multiple
	 operating systems.

8. **Network Cloning**: Clonezilla can clone a hard drive over a
	 network, which is useful for deploying systems in a networked
	 environment.

9. **Restore from Image**: Clonezilla can restore a system from an
	 image, allowing for recovery from a backup or the deployment of a new
	 system configuration.

10. **Cross-Platform**: Clonezilla can run on various operating systems,
		including Windows, Linux, and macOS, making it accessible to a wide
		range of users.

# How Clonezilla Works:

Clonezilla works by creating an exact copy of the source disk's data and
structure. It does this by reading the data from the source disk and
writing it to the destination disk. The process involves several steps:

1. **Preparation**: Before cloning, Clonezilla checks the source and
	 destination disks for errors and ensures they are compatible.

2. **Imaging**: Clonezilla reads the data from the source disk and
	 creates an image file. This image file can then be saved to an
	 external storage device or transferred over a network.

3. **Restoration**: To restore a system from an image, Clonezilla reads
	 the image file and writes the data to the destination disk. This
	 process recreates the exact state of the source disk on the
	 destination disk.

4. **Verification**: After the cloning or imaging process, Clonezilla
	 can verify the integrity of the data to ensure that the cloned or
	 imaged disk is identical to the original.

# Use Cases:

- **Backup and Recovery**: Clonezilla is widely used for backing up and
	recovering systems. It allows users to create a complete backup of
	their system, which can be restored in case of data loss or system
	failure.

- **System Deployment**: In organizations, Clonezilla can be used to
	deploy the same system configuration across multiple machines,
	ensuring consistency and reducing setup time.

- **Data Migration**: Users can use Clonezilla to migrate their data to
	a new hard drive or to another computer, preserving their applications
	and settings.

- **Disk Replacement**: When replacing a hard drive, Clonezilla can be
	used to clone the old drive to the new one, ensuring that all data and
	system configurations are preserved.


# Real-Life example

Creating a clone of a Linux machine with an ext4 filesystem involves
several steps. This process can be done using various tools, but one
of the most popular and effective methods is using Clonezilla, a free
and open-source disk cloning and imaging tool. Clonezilla supports a
wide range of filesystems, including ext4, and is capable of cloning
entire hard drives or creating disk images.

## Making clone

### Prerequisites

- **Clonezilla Live Media**: You'll need to download the Clonezilla Live
	Media from the official website (https://clonezilla.org/). Choose the
	version that suits your needs (e.g., Clonezilla Live, Clonezilla SE,
	or Clonezilla Live ISO).
- **Bootable Media**: You'll need a bootable USB drive or CD/DVD to boot
	the Clonezilla Live Media. Tools like Rufus (for Windows) or `dd`
	command (for Linux) can be used to create a bootable USB drive.
- **Target Storage**: Ensure you have a target storage device (e.g., an
	external hard drive) with enough space to hold the cloned data.

### Steps to Clone a Linux Machine

1. **Prepare the Clonezilla Live Media**:
	 - Download Clonezilla Live Media from the official website.
	 - Create a bootable USB drive or CD/DVD using the downloaded media.

2. **Boot from Clonezilla Live Media**:
	 - Insert the bootable media into the machine you want to clone.
	 - Restart the machine and boot from the Clonezilla Live Media. You
		 might need to adjust your BIOS/UEFI settings to boot from the USB
		 drive or CD/DVD.

3. **Start Clonezilla**:
	 - Once Clonezilla boots, you'll see the Clonezilla menu. Choose the
		 option to start Clonezilla.

4. **Select Operation Mode**:
	 - Choose "device-image" for disk imaging/cloning.

5. **Select Clonezilla Workflow Type**:
	 - Choose "Beginner mode" for a simpler process.

6. **Select Image Repository Location**:
	 - Choose where you want to save the cloned image. This could be an
		 external drive or a network location.

7. **Select Disk to Clone**:
	 - Choose the disk you want to clone. Clonezilla will list all
		 available disks.

8. **Select Clone Mode**:
	 - Choose "Clonezilla clone" to clone the entire disk.

9. **Select Compression**:
	 - You can choose to compress the image to save space. This step is
		 optional.

10. **Select Encryption**:
		- If you want to encrypt the cloned image, choose the encryption
			method. This step is optional.

11. **Start Cloning**:
		- Review your selections and start the cloning process. Clonezilla
			will begin copying the data from the source disk to the target
			storage.

12. **Verify the Clone**:
		- Once the cloning process is complete, you can verify the integrity
			of the cloned image. This step is optional but recommended.

13. **Exit Clonezilla**:
		- After verification, you can exit Clonezilla. If you're cloning to
			an external drive, you can now remove it and use it as needed.

> **Important Considerations**
> 
> - **Backup Important Data**: Before starting the cloning process,
>   ensure you have backed up any important data. Cloning can overwrite
>   existing data on the target storage.
> - **Compatibility**: Ensure the target storage device is compatible
>   with the cloned system. For example, if the source system uses an
>   ext4 filesystem, the target storage should also support ext4.
> - **System Requirements**: The target storage device must have enough
> space to hold the cloned data. The cloned image will be the same size
> as the source disk.

## Restore a clone

Restoring a clone created with Clonezilla involves a similar process to
creating the clone, but in reverse. You'll use Clonezilla to restore the
cloned image onto a new or existing hard drive. This process is
straightforward and can be done even if the original system is not
available. Here's how to restore a clone using Clonezilla:

### Prerequisites

- **Clonezilla Live Media**: You'll need the Clonezilla Live Media that
	you used to create the clone. If you don't have it, download it from
	the official website (https://clonezilla.org/).
- **Bootable Media**: Create a bootable USB drive or CD/DVD with the
	Clonezilla Live Media.
- **Cloned Image**: Ensure you have the cloned image file or the
	external storage device where the cloned image is saved.
- **Target Hard Drive**: Identify the hard drive where you want to
	restore the clone. This could be the same drive from which the clone
	was made or a new one.

### Steps to Restore a Clone

1. **Prepare the Clonezilla Live Media**:
	 - If you haven't already, create a bootable USB drive or CD/DVD with
		 the Clonezilla Live Media.

2. **Boot from Clonezilla Live Media**:
	 - Insert the bootable media into the machine where you want to
		 restore the clone.
	 - Restart the machine and boot from the Clonezilla Live Media. You
		 might need to adjust your BIOS/UEFI settings to boot from the USB
		 drive or CD/DVD.

3. **Start Clonezilla**:
	 - Once Clonezilla boots, you'll see the Clonezilla menu. Choose the
		 option to start Clonezilla.

4. **Select Operation Mode**:
	 - Choose "device-image" for disk imaging/cloning.

5. **Select Clonezilla Workflow Type**:
	 - Choose "Beginner mode" for a simpler process.

6. **Select Image Repository Location**:
	 - Choose where the cloned image is stored. This could be an external
		 drive or a network location.

7. **Select Disk to Restore**:
	 - Choose the disk you want to restore the clone to. Clonezilla will
		 list all available disks.

8. **Select Restore Mode**:
	 - Choose "Clonezilla restore" to restore the cloned image to the
		 selected disk.

9. **Start Restoring**:
	 - Review your selections and start the restoring process. Clonezilla
		 will begin copying the data from the cloned image to the target
		 disk.

10. **Verify the Restore**:
		- Once the restoring process is complete, you can verify the
			integrity of the restored system. This step is optional but
			recommended.

11. **Exit Clonezilla**:
		- After verification, you can exit Clonezilla. The restored system
			should now be bootable.


# Make a bootable Clonezilla using `dd` command

Creating a bootable disk using the `dd` command is a straightforward
process that involves writing an ISO image to a USB drive or CD/DVD.
This method is particularly useful for making Clonezilla bootable, as it
allows you to create a bootable Clonezilla Live Media on a USB drive or
CD/DVD. Here's how to do it:

## Prerequisites

- **Clonezilla ISO File**: Download the Clonezilla ISO file from the
	official Clonezilla website (https://clonezilla.org/). Choose the
	version that suits your needs (e.g., Clonezilla Live, Clonezilla SE,
	or Clonezilla Live ISO).
- **USB Drive or CD/DVD**: You'll need a USB drive or CD/DVD with enough
	space to hold the Clonezilla ISO file. For a USB drive, ensure it's
	formatted to FAT32.

### Steps to Make Clonezilla Bootable Using `dd` Command

#### For USB Drive:

1. **Identify the USB Drive**:
	 - Plug in your USB drive and use the `lsblk` command to list all
		 block devices. Identify your USB drive by its size and note the
		 device name (e.g., `/dev/sdb`).

2. **Unmount the USB Drive**:
	 - Before writing to the USB drive, ensure it's not mounted. Use the
		 `umount` command followed by the device name to unmount it. For
		 example: ``` sudo umount /dev/sdb1 ```

3. **Write the ISO to the USB Drive**:
	 - Use the `dd` command to write the Clonezilla ISO file to the USB
		 drive. Replace `/path/to/clonezilla.iso` with the actual path to
		 the ISO file and `/dev/sdb` with your USB drive's device name. ```
		 sudo dd if=/path/to/clonezilla.iso of=/dev/sdb bs=4M
		 status=progress ```
	 - This command writes the ISO file to the USB drive, with `bs=4M`
		 specifying the block size for the operation and `status=progress`
		 showing the progress of the write operation.

4. **Sync and Eject the USB Drive**:
	 - After the `dd` command completes, use the `sync` command to ensure
		 all data is written to the USB drive. ``` sync ```
	 - Then, safely eject the USB drive.

#### For CD/DVD:

1. **Insert the CD/DVD**:
	 - Insert a blank CD/DVD into your CD/DVD writer.

2. **Write the ISO to the CD/DVD**:
	 - Use the `dd` command to write the Clonezilla ISO file to the
		 CD/DVD. Replace `/path/to/clonezilla.iso` with the actual path to
		 the ISO file and `/dev/cdrom` with your CD/DVD writer's device
		 name. ``` sudo dd if=/path/to/clonezilla.iso of=/dev/cdrom bs=4M
		 status=progress ```
	 - This command writes the ISO file to the CD/DVD, with `bs=4M`
		 specifying the block size for the operation and `status=progress`
		 showing the progress of the write operation.

3. **Eject the CD/DVD**:
	 - After the `dd` command completes, use the `eject` command to safely
		 eject the CD/DVD. ``` eject /dev/cdrom ```

> **Important Considerations**
> 
> - **Be Careful with the `dd` Command**: The `dd` command is very
> 	powerful and can overwrite data if used incorrectly. Always
> 	double-check the device names (`if` and `of` parameters) before
> 	executing the command.
> - **USB Drive Formatting**: If you're using a USB drive, ensure it's
> 	formatted to FAT32. The `dd` command does not format the drive, so
> 	you'll need to do this manually beforehand if necessary.
> - **Backup Important Data**: Before writing to the USB drive or CD/DVD,
> 	ensure you have backed up any important data, as the process will
> 	erase all existing data on the drive.

By following these steps, you can create a bootable Clonezilla Live Media using the `dd` command. This bootable media can then be used to boot into Clonezilla and perform disk cloning, imaging, or deployment tasks.
