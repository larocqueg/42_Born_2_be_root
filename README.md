1 - What is a virtual machine?

A virtual machine uses part of the physical hardware of my machine to run another operating system, without the need to install it directly on the computer. This allows multiple operating systems to run simultaneously on a single machine, making it easier for users who need more than one system to use different environments.

2 - Choice of operating system

I chose Debian because, as mentioned in the PDF, it is the most recommended for people with little experience in virtual machines, due to its stability and "simplicity." However, after researching Rocky, I found that it is more focused on enterprise use.

3 - What are the benefits of VMs?

As mentioned earlier, VMs allow multiple operating systems to run on a single machine simultaneously, which reduces costs for additional machines and makes it easier for people who need different operating systems to work.

4 - Difference between aptitude and apt

APT: A more direct and simple tool, ideal for quick and efficient commands for common tasks, such as:

    Updating the package list (apt update)
    Updating installed packages (apt upgrade)
    Installing packages (apt install)
    Removing packages (apt remove)

Aptitude: Provides an interactive interface in the terminal, making it easier to search for packages and updates visually. Additionally, it has an advanced algorithm to solve dependency issues and other problems that may arise during package installation, upgrade, or removal.

    Update the package list (aptitude update)
    Update installed packages (aptitude upgrade)
    Install packages (aptitude install)
    Remove packages (aptitude remove)
    Search for packages (aptitude search)

In summary, apt is better for simple and quick tasks, while aptitude is more suitable for solving dependency issues and providing an interactive navigation experience.

5 - What is AppArmor?

AppArmor is a security tool for Linux that restricts the permissions of applications and processes on the system. Its goal is to limit the impact of security flaws by preventing malicious or compromised apps from affecting the system.

6 - Is UFW installed?

What is it? → UFW (Uncomplicated Firewall) is a firewall for Linux systems, designed to enhance security against unauthorized access by blocking unnecessary ports and protocols.

Commands:

    sudo ufw status numbered
    sudo systemctl status ufw
    sudo ufw status verbose (check active UFW rules)
    sudo ufw allow 8080 (add a new rule to UFW)
    sudo ufw delete allow 8080 (remove the created rule)

7 - What is SSH?

SSH (Secure Shell) is a protocol used for securely accessing and managing systems remotely, widely used in server environments and system administration.

Commands:

    sudo systemctl status ssh
    sudo systemctl status sshd
    sudo vim /etc/ssh/sshd_config (open the SSH configuration file to view the port 4242 configured) If any modification is made, use: sudo systemctl restart ssh
    sudo ss -tuln | grep 4242 (check if SSH is running on port 4242)
    PermitRootLogin no (ensures root cannot log in via SSH, improving security) To connect to an existing user via SSH, use the command:

ssh <username>@<server_ip> -p 4242

8 - Checking existing users and groups

To check the users, use the command: cat /etc/passwd (shows the content of the file containing the user names); To check the groups, use: cat /etc/group (shows the content of the file containing the group names).

9 - Creating users and groups

To create a new user, use the command: sudo adduser <username>; To create a group, use the command: sudo groupadd <group_name>.

10 - Associating users to new groups

To add a user to a new group, use: sudo usermod -aG <group_name> <username>; Flags:

    -aG: a → adds the user to a new group without removing them from existing groups; G → specifies the additional groups to which the user will be added.

11 - Checking the hostname

Use the command: hostnamectl (shows the hostname and other details about the machine).

12 - Changing the hostname

To change the hostname, use the command: sudo hostnamectl set-hostname <new_hostname>; Then, change the name in the /etc/hostname file with: sudo vim /etc/hostname; Also, change the /etc/hosts file with: sudo vim /etc/hosts; Then, reboot the system with: sudo reboot. After rebooting, use the hostname command to verify if the change was successful.

13 - Partitions

Use the command: lsblk. This command will display the following information:

    NAME (device or partition name)
    SIZE (size of the device or partition)
    TYPE (type [disk for disks and part for partitions])
    MOUNTPOINT (where the partition is mounted in the system)

14 - LVM (Logical Volume Manager)

LVM is a logical volume management technology that offers greater flexibility and dynamism compared to traditional partitions. It creates an abstraction layer between the physical hardware (disks) and the filesystem, making it easier to expand, reduce, or reorganize storage.

Benefits:

    Flexibility: allows resizing, adding, or removing disks and volumes without interrupting the system.
    Snapshots: allows creating instant backups of volumes.
    Stability: makes it easier to expand storage by adding more disks to the volume group.
    Simplified management: provides a unified view of all disks and volumes in the system.

15 - Sudo (Super User Do)

The sudo command is used to grant temporary administrative permissions for commands in the terminal. It always asks for the password of the user executing the command, but after the first execution, it will not ask for the password again unless the terminal is restarted.

Not all users can use sudo; only those who are part of the sudo group, ensuring greater security for the machine.

To check if sudo is installed, use: which sudo (this command will return the path where sudo is located).

16 - Cron

Cron is a Linux utility that allows you to schedule tasks to be executed automatically.

Commands:

    crontab -e (used to schedule the script);
    */10 * * * * <script_path> (*/10: means "every 10 minutes", * corresponds to any hour, day, month, and weekday);
    crontab -l (shows recently added entries);
    sudo cat /etc/crontab && sudo cat /etc/cron.d/* (to ensure no tasks are scheduled to run on boot).

17 - Script

**System Architecture (arc)**:
`arc=$(uname -a)`: Gets the system's architecture information (kernel version, hostname, OS type, etc.).

**CPU Information**:
`pcpu=$(grep "physical id" /proc/cpuinfo | sort | uniq | wc -l)`: Counts the number of physical CPUs by looking at physical id in /proc/cpuinfo.
`vcpu=$(grep "^processor" /proc/cpuinfo | wc -l)`: Counts the number of virtual CPUs (processors) in the system.

**Memory Information**:
`fram=$(free -m | awk '$1 == "Mem:" {print $2}')`: Gets the total system memory in MB.
`uram=$(free -m | awk '$1 == "Mem:" {print $3}')`: Gets the used memory in MB.
`pram=$(free | awk '$1 == "Mem:" {printf("%.2f"), $3/$2*100}')`: Calculates and prints the percentage of used memory.

**Disk Information**:
`fdisk=$(df -BG | grep '^/dev/' | grep -v '/boot$' | awk '{ft += $2} END {print ft}')`: Gets the total disk space used by all devices, excluding /boot.
`udisk=$(df -BM | grep '^/dev/' | grep -v '/boot$' | awk '{ut += $3} END {print ut}')`: Gets the used disk space in MB, excluding /boot.
`pdisk=$(df -BM | grep '^/dev/' | grep -v '/boot$' | awk '{ut += $3} {ft+= $2} END {printf("%d"), ut/ft*100}')`: Calculates the percentage of used disk space.

**CPU Load**:
`cpul=$(top -bn1 | grep '^%Cpu' | cut -c 9- | xargs | awk '{printf("%.1f%%"), $1 + $3}')`: Fetches the current CPU load (user + system).

**Last Boot**:
`lb=$(who -b | awk '$1 == "system" {print $3 " " $4}')`: Shows the date and time of the last system boot.

**LVM Usage**:
`lvmu=$(if [ $(lsblk | grep "lvm" | wc -l) -eq 0 ]; then echo no; else echo yes; fi)`: Checks if LVM (Logical Volume Manager) is being used by searching for "lvm" in the block devices.

**Network Connections**:
`ctcp=$(ss -Ht state established | wc -l)`: Counts the number of established TCP connections.

**User Logins**:
`ulog=$(users | wc -w)`: Counts the number of currently logged-in users.

**Network Information**:
`ip=$(hostname -I)`: Gets the system's IP address.
`mac=$(ip link show | grep "ether" | awk '{print $2}')`: Retrieves the MAC address of the system.

**Sudo Command Usage**:
`cmds=$(journalctl _COMM=sudo | grep COMMAND | wc -l)`: Counts the number of commands run using sudo (from the journal logs).

**Wall Command**:
The `wall` command is used to broadcast the collected system information to all logged-in users. The information includes:
- Architecture
- CPU (physical and virtual)
- Memory usage (total and used with percentage)
- Disk usage (total, used with percentage)
- CPU load
- Last boot time
- LVM usage
- Number of established TCP connections
- Logged-in users
- Network details (IP and MAC addresses)
- Number of sudo commands executed
