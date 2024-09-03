lsblk
sudo dd if=~/Downloads/archlinux-2023.08.01-x86_64.iso of=/dev/sdc bs=4M status=progress oflag=sync
If the ISO file is on the flash memory itself (/dev/sdc), you can't use dd to overwrite the same device where the ISO is stored. You'll need to copy the ISO file to another location first, then burn it to the flash memory. Here's how to do it:
    Mount the flash memory (if not already mounted):
        First, create a mount point and mount the flash memory:
    sudo mkdir /mnt/flash
    sudo mount /dev/sdc /mnt/flash
Copy the ISO file to your local drive:
    Copy the ISO file from the flash memory to your home directory (or any other location):
    cp /mnt/flash/path_to_iso_file.iso ~/
    Replace path_to_iso_file.iso with the actual name of your ISO file on the flash memory.
Unmount the flash memory:
    Unmount the flash memory to prepare it for the burning process:
    sudo umount /mnt/flash
Burn the ISO to the flash memory:
    Now that the ISO is on your local drive, you can use dd to burn it to the flash memory:
sudo dd if=~/path_to_iso_file.iso of=/dev/sdc bs=4M status=progress oflag=sync
sudo eject /dev/sdX

To set up Arch Linux with the partitioning scheme you've outlined, follow these steps:
1. Boot from the USB Drive
    Boot your laptop from the USB drive with the Arch Linux ISO.
2. Prepare the SSD (for /boot/efi, swap, and /)
First, identify your SSD and HDD using lsblk or fdisk -l. Assuming your SSD is /dev/nvme0n1 (common for NVMe drives) and your HDD is /dev/sda, here’s how to partition the SSD:
    Partition the SSD:
cfdisk /dev/nvme0n1
Create the following partitions:
    1 GB for /boot/efi:
        Type: EFI System
    10 GB for swap:
        Type: Linux swap
    Remaining space for / (root):
        Type: Linux filesystem
Format the partitions:
mkfs.fat -F32 /dev/nvme0n1p1   # Format /boot/efi as FAT32
mkswap /dev/nvme0n1p2           # Initialize the swap partition
mkfs.ext4 /dev/nvme0n1p3        # Format the root partition as ext4
Enable the swap:
    swapon /dev/nvme0n1p2
3. Prepare the HDD (for /home)
Assuming your HDD is /dev/sda:
    Partition the HDD:
cfdisk /dev/sda
Create a single partition using the entire disk, and set the type to Linux filesystem.
Format the partition:
    mkfs.ext4 /dev/sda1   # Format the /home partition as ext4
4. Mount the Filesystems
    Mount the root partition:
mount /dev/nvme0n1p3 /mnt
Create the /boot/efi directory and mount it:
mkdir -p /mnt/boot/efi
mount /dev/nvme0n1p1 /mnt/boot/efi
Create the /home directory and mount the HDD:
    mkdir /mnt/home
    mount /dev/sda1 /mnt/home
iwctl
device list               # Confirm wlan0 is listed
device wlan0 set-property Powered on
lspci -k | grep -A 3 -i network
Manually Bring Up the Interface
    If the above steps don't work, try manually bringing up the interface:
ip link set wlan0 up
Then retry scanning for networks:
    iwctl station wlan0 scan
5. Use nmtui or wifi-menu as an Alternative
    If iwctl still doesn’t work, you can try using nmtui (NetworkManager Text User Interface) or wifi-menu (if available) to connect to Wi-Fi:
        Using nmtui:
nmtui
Navigate through the interface to connect to your Wi-Fi network.
Using wifi-menu:
wifi-menu
5. Install Arch Linux Base System
    Install the base system, including essential packages:
    pacstrap /mnt base linux linux-firmware
6. Generate the Filesystem Table (/etc/fstab)
    Generate the fstab file:
    genfstab -U /mnt >> /mnt/etc/fstab
    Verify the /etc/fstab to ensure correct entries for /, /boot/efi, /home, and swap.
7. Chroot into the New System
    Enter the new system’s root:
    arch-chroot /mnt
8. Set Up the System
    Set the Timezone:
Set the Timezone:
ln -sf /usr/share/zoneinfo/Region/City /etc/localtime
hwclock --systohc
Localization:
Edit /etc/locale.gen and uncomment en_US.UTF-8 UTF-8, then generate the locales:
locale-gen
echo "LANG=en_US.UTF-8" > /etc/locale.conf
Set the Hostname:
echo "myhostname" > /etc/hostname
Add matching entries to /etc/hosts:
echo "127.0.0.1   localhost" >> /etc/hosts
echo "::1         localhost" >> /etc/hosts
echo "127.0.1.1   myhostname.localdomain myhostname" >> /etc/hosts
Install the Bootloader:
Install grub and efibootmgr:
pacman -S grub efibootmgr
Install GRUB:
grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=GRUB
grub-mkconfig -o /boot/grub/grub.cfg
passwd
useradd -m -g users -G wheel omar
passwd
su -
EDITOR=nano visudo
uncomment %wheel ALL=(ALL)ALL
save and exit
usermod -aG wheel omar
sudo systemctl enable NetworkManager

sudo pacman -S zathura zathura-pdf-mupdf zathura-ps zathura-djvu zathura-cb


