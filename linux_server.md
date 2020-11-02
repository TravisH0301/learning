# Linux Server

## Distributors
Linux server has many distributors such as Ubuntu, Red Hat, SUSE and even Amazon (through AWS EC2)

### Connection to Linux Server on Cloud
Linux server on cloud can be connected and controlled using SSH (secure shell). SSH clients such as PuTTY can be used to access and share files with linux server on cloud.

#### Remote Linux Server Connection via PuTTY
1. In `Session`, type in `Host Name` (IP address) & `Port`
2. In `Connection` > `Data`, type `Auto-login username` (if username is required)
3. In `Connection` > `SSH` > `Auth`, load key.ppk for authentication
4. In `Session`, save the setting by using `Saved Sessions`

#### File Transfer via PuTTY
PuTTY uses a secure copy utility is called PuTTy Secure Copy Protocol (PSCP) to transfer files.<br>
1. Download PSCP from https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html
2. Add path of PSCP. For ex. if PSCP is install in Desktop, `$ PATH = "%PATH%;C:\Users\Travis\Desktop"`
3. Send file to Linux Server on Cloud by specifying pathnames for the file and the destination.<br>
`$ pscp -P 22 C:\path\Sample_file.txt user_id@server_example.com:/home/my-instance-user-name/Sample_file.txt`<br>
For AWS, include the key pair file.<br>
`$ pscp -P 22 -i C:\path\my-key-pair.ppk C:\path\Sample_file.txt my-instance-user-name@my-instance-public-dns-name:/home/my-instance-user-name/Sample_file.txt`

Other way is using WinSCP which is a GUI-based file manager.
1. Download WinSCP from https://winscp.net/eng/download.php
2. For Host name, type user_id@server.com.
3. Click Advanced > Select SSH (left panel) > Select Authentication (left panel) > load key pair file for authentication
4. Once logged in, drag file to be transferred inbetween left(local) and right(cloud) screens.

## Software Installation
Linux distributors use different package management utilities. For example, Ubuntu uses APT (Advanced Package Tool) and Amazon Linux uses YUM (Yellowdog Updater, Modified).




