# Introduction to Advanced Linux Commands
### File Permissions and Access Rights
In Linux, file permissions and access rights are fundamental to system security and user management. They determine who can read, write, or execute files and directories, ensuring proper control over sensitive data and system resources.

Linux assigns permissions to three categories of users:

1. Owner (user) – The creator of the file/directory.

2. Group – Users who are part of a specific group.

3. Others – All other users on the system.

Each file and directory has three basic permission types:

- Read (r) – Allows viewing file contents or listing directory contents.

- Write (w) – Permits modifying a file or adding/removing files in a directory.

- Execute (x) – Grants the ability to run a file as a program or traverse a directory.

These permissions can be viewed and modified using commands like ls -l, chmod, chown, and chgrp. Understanding and managing file permissions is essential for maintaining security, preventing unauthorized access, and ensuring smooth system operations.

## File Permission Commands
### chmod (Change Mode) Command
The chmod command is used to change the permissions of a file or directory.

## 1. Connecting to AWS Ubuntu Instance
![Connecting to AWS Ubuntu Instance](./img/1.%20Connecting%20to%20AWS%20Ubuntu%20Instance.jpg)

**Steps:**
1. Use SSH with your PEM key:  
   ```bash
   ssh -i "keytest.pem" ubuntu@ec2-54-234-218-213.compute-1.amazonaws.com
   ```
2. System information is displayed upon successful connection.

**Key Details:**
- Ubuntu 24.04.2 LTS
- System load, disk usage, and memory stats shown
- Security recommendations provided

---

## 2. Creating and Modifying File Permissions
![Creating an empty file](./img/2.%20Creating%20an%20empty%20file%20using%20touch%20command.jpg)

**Practical Steps:**
1. Create a file:
   ```bash
   touch script.sh
   ```
2. View permissions:
   ```bash
   ls -l
   ```
3. Remove group write permission:
   ```bash
   chmod g-w script.sh
   ```

**Observation:**
- Initial permissions: `-rw-rw-r--`
- After modification: `-rw-r--r--`

---

## 3. Understanding Permission Symbols
![Checking file permissions](./img/3.%20Checking%20the%20permission%20of%20the%20file.jpg)

**Permission Breakdown:**
```
-rw-r--r--
```
- `-`: Regular file
- `rw-`: Owner can read/write
- `r--`: Group can read
- `r--`: Others can read

**Command Used:**
```bash
ls -latr script.sh
```

---

## 4. Visualizing Permission Groups
![Permission group analysis](./img/4.%20What%20I%20think%20About%20the%20permission.jpg)

**Three Permission Groups:**
1. **Owner (ubuntu)**: `rw-` → Read/Write
2. **Group**: `r--` → Read only
3. **Others**: `r--` → Read only

**Command:**
```bash
ls -latr script.sh
```

---

## 5. Adding Execute Permissions
![Adding execute permission](./img/5.%20Granting%20Execute%20permission%20to%20the%20file%20script%20dot%20sh.jpg)

**Action:**
```bash
chmod +x script.sh
```

**Effect:**
- Grants execute permission to all users
- Changes to: `-rwxr-xr-x`

---

## 6. Verifying New Permissions
![Updated permissions](./img/6.%20Checking%20the%20new%20permissions.jpg)

**New Permission Breakdown:**
```
-rwxr-xr-x
```
- Owner: `rwx` (Read/Write/Execute)
- Group: `r-x` (Read/Execute)
- Others: `r-x` (Read/Execute)

---

## 7. Setting Permissions Numerically
![Numeric permissions](./img/7.%20Assigning%20the%20same%20permission%20using%20755.jpg)

**Using Octal Notation:**
```bash
chmod 755 script.sh
```

**Explanation:**
- `7` (Owner): 4(r) + 2(w) + 1(x)
- `5` (Group/Others): 4(r) + 1(x)
- Same as `-rwxr-xr-x`

---

## 8. Creating New Files with Default Permissions
![New file creation](./img/8.%20New%20file%20created.jpg)

**Process:**
1. Create new file:
   ```bash
   touch note.txt
   ```
2. Check default permissions:
   ```bash
   ls -latr note.txt
   ```

**Default Permissions:**
- `-rw-rw-r--` (664)
- Different from script.sh's 755 permissions

## 9. Assigning Full Permissions (777)
![Full permissions](./img/9.%20Assigning%20full%20permission.jpg)

**Commands:**
```bash
chmod 777 note.txt
ls -latr note.txt
```

**Output:**
```
-rwxrwxrwx 1 ubuntu ubuntu 0 May 18 11:44 note.txt
```

**Explanation:**
- `777` grants read/write/execute to owner, group, and others
- File color changes to green in many terminals (indicates executable)
- **Security Note**: Avoid 777 permissions in production systems

---

## 10. Verifying Universal Access
![Permission confirmation](./img/10.%20Confirming%20all%20permission%20for%20note%20dot%20txt.jpg)

**Command:**
```bash
ls -latr note.txt
```

**Breakdown:**
```
-rwxrwxrwx
```
- Owner: `rwx` (Full access)
- Group: `rwx` (Full access)
- Others: `rwx` (Full access)

**Warning:**  
777 permissions allow any user to modify/delete the file - use sparingly!

---

## 11. Creating a New User
![User creation](./img/11.%20Adding%20new%20user.jpg)

**Steps:**
```bash
sudo adduser john  # Corrected from "adducer" typo
```
**Process:**
1. Sets password (hidden input)
2. Adds user info (Name, Phone, etc.)
3. Creates home directory `/home/john`

---

## 12. Verifying User Identity
![User verification](./img/12.%20Confriming%20the%20user.jpg)

**Command:**
```bash
id john
```

**Output:**
```
uid=1001(john) gid=1001(john) groups=1001(john),100(users)
```

**Key Fields:**
- UID: User ID
- GID: Primary Group ID
- Groups: Supplementary memberships

---

## 13. Creating Developer Group
![Group creation](./img/13.%20group%20developer%20created%20with%20id%20confirm.jpg)

**Commands:**
```bash
sudo groupadd developer
getent group developer
```

**Output:**
```
developer:x:1002:
```

**Explanation:**
- Creates system group with GID 1002
- `getent` confirms group existence in `/etc/group`

---

## 14. Changing File Ownership
![Ownership change](./img/14.%20Changed%20the%20owner%20to%20john%20in%20developer%20group.jpg)

**Process:**
1. Attempt without sudo fails:
   ```bash
   chown john:developer note.txt  # Permission denied
   ```
2. Success with sudo:
   ```bash
   sudo chown john:developer note.txt
   ```

**Verification:**
```
-rwxrwxrwx 1 john developer 0 May 18 11:44 note.txt
```
---

## 15. Switching to Root Privileges
![Root access](./img/15.%20Switched%20to%20sudo%20mode.jpg)

**Command:**
```bash
sudo -i
```

**Result:**
- Prompt changes from `$` to `#`
- Full root privileges
- Dangerous: Avoid prolonged root sessions

**Exit Root:**
```bash
exit
```
## 16. Exiting Root Privileges
![Switching to user mode](./img/16.%20Switched%20to%20user%20mode.jpg)

**Process:**
```bash
sudo -i       # Enter root mode
exit          # Return to regular user
```

**Key Points:**
- Root prompt: `#` (Danger Zone!)
- Regular user prompt: `$`
- Always minimize root session duration

---

## 17. Changing User Password
![Password update](./img/17.%20Password%20sucssesfully%20changed.jpg)

**Command:**
```bash
sudo passwd john
```

**Process:**
1. Verify current user privileges
2. Enter current password for authentication
3. Set new password (hidden input)
4. Confirm password change

**Security Tip:**  
Use complex passwords: mix uppercase, numbers, and special characters!

---

## 18. User Session Verification
![User login](./img/18.%20Logged%20in%20as%20john.jpg)

**Verification Steps:**
1. Switch to john:
   ```bash
   su john
   ```
2. Test privileges:
   ```bash
   ping 172.31.23.249  # Basic network test
   ```

**Output Analysis:**
```
64 bytes from 172.31.23.249: icmp_seq=1 ttl=64 time=0.013 ms
```
- Successful pings confirm network connectivity
- Low latency (<0.03ms) indicates localhost communication

---

## 19. Process Termination
![Process termination](./img/19.%20kill%20the%20process%20owned%20by%20john.jpg)

**Scenario:**  
Cannot delete active user:
```bash
userdel john  # Fails - user has active processes
```

**Solution:**
```bash
pkill -u john  # Terminate all john's processes
userdel john    # Now succeeds
```

**Alternative:**
```bash
killall -u john  # More forceful termination
```

---

## 20. DevOps Group Creation
![Group creation](./img/20.%20Group%20devops%20create%20with%20id%20number.jpg)

**Commands:**
```bash
sudo groupadd devops
getent group devops
```

**Output:**
```
devops:x:1004:
```

**Explanation:**
- Group ID (GID): 1004
- `x` indicates shadow password system in use
- No members initially

---

## 21. Bulk User Creation
![User creation](./img/21.%20Five%20users%20created%20and%20assigned%20password.jpg)

**Automation Script:**
```bash
sudo useradd -m -G devops mary && sudo passwd mary
sudo useradd -m -G devops mohammed && sudo passwd mohammed
# Repeated for ravi, tunji, sofia
```

**Key Flags:**
- `-m`: Create home directory
- `-G devops`: Add to secondary group
- **Best Practice:** Use password managers for bulk operations

---

## 22. Bulk User Creation
![User creation](./img/22.%20Confirming%20created%20users.jpg)

**Automation Script:**
```bash
sudo useradd -m -G devops mary && sudo passwd mary
sudo useradd -m -G devops mohammed && sudo passwd mohammed
# ... repeated for ravi, tunji, sofia
```

**Flags Explained:**
- `-m`: Create home directory
- `-G devops`: Add to secondary group

**Common Error:**  
Typo in username (`nohammed` instead of `mohammed`) - always verify!

---

## 23. Group Membership Verification
![Group members](./img/23.%20Members%20of%20devops%20group.jpg)

**Verification Commands:**
```bash
id mary
getent group devops
```

**Output:**
```
devops:x:1004:mary,mohammed,ravi,tunji,sofia
```

**Breakdown:**
- Group Name: devops
- GID: 1004
- Members: 5 users
- Primary groups remain separate (1001-1008)

---

## System Administration Best Practices

1. **User Management:**
   ```bash
   sudo usermod -aG devops existing_user  # Add to group
   sudo deluser --remove-home username   # Complete removal
   ```

2. **Process Monitoring:**
   ```bash
   ps -u john       # List user processes
   lsof -u john     # Show open files
   ```

3. **Group Permissions:**
   ```bash
   chgrp devops /path/to/file  # Change group ownership
   chmod g+rwx /path/to/dir   # Add group permissions
   ```

4. **Security Audit:**
   ```bash
   sudo grep devops /etc/group      # Verify group members
   sudo ls -l /home                # Check home directories
   sudo passwd -S john             # View password status
   ```

**Critical Reminder:**  
Always test user/group changes in non-production environments first!

## 24. Verifying Home Directory Permissions
![Directory check](./img/24.%20confirming%20the%20directory.jpg)

**Command:**
```bash
ls -ld /home/mary /home/mohammed /home/ravi /home/tunji /home/sofia
```

**Output:**
```
drwxr-x--- 2 mary    mary    4096 May 18 13:45 /home/mary
drwxr-x--- 2 mohammed mohammed 4096 May 18 13:45 /home/mohammed
...

**Key Observations:**
- Default permissions: `750` (`rwxr-x---`)
- Owner: User-specific primary groups (e.g., `mary:mary`)
- Group members can only read/execute directories
- **Security Note:** Isolated by default for user privacy

---

## 25. Changing Group Ownership
![Ownership change](./img/25.%20change%20ownership%20to%20devops.jpg)

**Command:**
```bash
sudo chown :devops /home/mary /home/mohammed /home/ravi /home/tunji /home/sofia
```

**What Changed:**
- Group ownership shifted from user's primary group to `devops`
- Now shows: `mary devops` instead of `mary mary`
- **Why?** Enables collaborative access for devops team members

---

## 26. Confirming Group Ownership
![Updated ownership](./img/26.%20Ownership%20is%20devops.jpg)

**Verification Command:**
```bash
ls -ld /home/mary /home/mohammed /home/ravi /home/tunji /home/sofia
```

**New Output:**
```
drwxr-x--- 2 mary    devops 4096 May 18 13:45 /home/mary
```

**Analysis:**
- Directories now belong to `devops` group
- Permissions remain `750` - group can read/execute but not write
- **Next Step:** Need to enable collaborative editing

---

## 27. Enabling Group Write Access
![Write permissions](./img/27.%20Elevated%20thier%20permission%20to%20also%20write%20as%20group%20memebrs%20of%20devops.jpg)

**Command:**
```bash
sudo chmod g+w /home/mary /home/mohammed /home/ravi /home/tunji /home/sofia
```

**Resulting Permissions:**
```
drwxrwx--- 2 mary devops 4096 May 18 13:45 /home/mary
```

**Breakdown:**
- Group permission changed from `r-x` to `rwx`
- **Impact:** All devops members can now:
  - Create/delete files
  - Modify existing content
  - Rename items in directories

---

## Security Considerations

1. **Principle of Least Privilege**
   - Start with `750` permissions
   - Only enable write access when necessary

2. **Audit Trails**
   ```bash
   sudo grep devops /etc/group
   sudo ls -l /home
   ```

3. **SSH Configuration**
   ```bash
   sudo nano /etc/ssh/sshd_config
   # Set AllowGroups devops for restricted access
   ```

4. **Backup Strategy**
   ```bash
   sudo tar -czvf devops_homes.tar.gz /home/mary /home/mohammed ...
   ```

**Pro Tip:** Use ACLs for granular control:
```bash
sudo setfacl -m g:devops:rwx /shared_directory
```