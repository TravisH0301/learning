# SSH
- [SSH Installation](#ssh-installation)
  - [SSH on Client](#ssh-on-client)
  - [SSH on Server](#ssh-on-server)
- [Public Key Authentication](#public-key-authentication)
  - [Key Pair - Public & Private](#key-pair---public--private)
  - [Copy Public Key to Server](#copy-public-key-to-server)
  - [SSH connection using Public Key Authentication](#ssh-connection-using-public-key-authentication)
  - [Config File](#config-file)
- [SSH File Transfer Protocol](#ssh-file-transfer-protocol)
  - [Transfer Files from Server to Client](#transfer-files-from-server-to-client)
  - [Transfer Files from Client to Server](#transfer-files-from-client-to-server)
- [Remote Development on VS Code via SSH](#remote-development-on-vs-code-via-ssh)
  - [Port Forwarding on VS Code](#port-forwarding-on-vs-code)
- [Connect to Cloudera CDSW](#connect-to-cloudera-cdsw)

Secure Shell(SSH) is a network protocol to access a computer remotely over an unsecured network.
It also refers to the suite of utilities to implement the SSH protocol. 

SSH has client-server model connecting the client with the server. It supports Secure Copy (SCP) and SSH File Transfer Protocol (SFTP)
for transferring data.

## SSH Installation
To use the SSH connection, SSH has to be installed on both server and client. Some popular options are:
- PuTTY: SSH client for Windows
- OpenSSH: SSH tool for Linux distributions

This section will show installation for OpenSSH on Linux.<br>

### SSH on Client
On the client side, SSH client needs to be installed. To check if SSH is already installed, `ssh` can be entered in the terminal. <br>
If SSH is installed, a response similiar to the below will be shown.

        username@host:~$ ssh
        
        usage: ssh [-1246AaCfGgKkMNnqsTtVvXxYy] [-b bind_address] [-c cipher_spec]
        [-D [bind_address:]port] [-E log_file] [-e escape_char]
        [-F configfile] [-I pkcs11] [-i identity_file]
        [-J [user@]host[:port]] [-L address] [-l login_name] [-m mac_spec] [-O ctl_cmd] [-o option] [-p port] [-Q query_option] [-R address] [-S ctl_path] [-W host:port] [-w local_tun[:remote_tun]]
        [user@]hostname [command]

Otherwise, OpenSSH client can be installed.

        sudo apt-get install openssh-client

### SSH on Server
For the server, SSH server is required. To check if SSH server is installed on the server. `ssh localhost` can be entered in the terminal. <br>
If the following response is returned, the SSH server is not installed. 

        username@host:~$ ssh localhost
        
        ssh: connect to host localhost port 22: Connection refused username@host:~$

OpenSSH server can be installed as below.

        sudo apt-get install openssh-server ii.
        
To ensure OpenSSH server is properly installed, `ssh localhost` command can be tested again. If the following response is returned, SSH server is 
installed properly. 

        username@host:~$ ssh localhost

        The authenticity of host 'localhost (127.0.0.1)' can't be established. ECDSA key fingerprint is SHA256:9jqmhko9Yo1EQAS1QeNy9xKceHFG5F8W6kp7EX9U3Rs. Are you sure you want to continue connecting (yes/no)? yes
        Warning: Permanently added 'localhost' (ECDSA) to the list of known hosts.

## Public Key Authentication
To establish a secure client-server connection, the public key authentication can be used. It adds cryptographic strength to the
SSH security. `ssh-keygen` can be used on the client-side to generate the authentication key pairs.

### Key Pair - Public & Private
`ssh-keygen` creates a pair of private key & public key using the following command.

        ssh-keygen -f <path to store key pair (optional & default: ~/.ssh/)> -t <key algorithm (optional & default:rsa)> -b <key size (optional & default:1024)>
        # more about key algorithm: https://www.ssh.com/academy/ssh/keygen#ssh-keys-and-public-key-authentication
        
To establish a connection the public key is copied to the server, and the private key stays in the client.<br>
Public key is used to encrpyt data that can only be read by the private key. 

When the public is considered trustworthy in the server, it is marked as authorized and is called the *authorized key*.<br>
On the other hand, the private key used for user identity is called the *identity key*. 

### Copy Public Key to Server
The public key can be copied to the server by using the command below. The public key will be stored in the authorized_key file, and will
allow client to establish a connection using the private key.

        ssh-copy-id -i <path to identity key> <username>@<host>
        
### SSH connection using Public Key Authentication
After the authentication key pair is generated and the public key is copied to the server, the following command can be used to establish 
a SSH tunnel to the server.

        ssh -i <path to identity key> -d <port> <username>@<host>

The connection can be closed using `exit` command.

### Config File
Configuration file for the SSH connection can be set up for quick connection on the client. <br>
OpenSSH client-side configuration file is named `config`, and it is stored in ~/.ssh.

        # config file
        Host <define name for the connection ex.cdsw>
          HostName <remote server hostname ex.localhost>
          IdentitiesOnly <whether to use only passed secret key | ex.yes>
          IdentityFile <location of secret key | ex.c:/users/user/.ssh/ssh>
          User <remote server username | ex.cdsw>
          Port <port number | ex.2024>

The following command can be used to connect to the server using the config file.

        ssh cdsw
  
*Note that when there are multiple identity keys on computer, enable "IdentitiesOnly" parameter to use only the given secret key for the connection.*<br>
*On command line, "IdentitiesOnly" can be used using "-o" parameter alongside "-i" parameter to point to the identity key.*

        ssh -p <port> <username>@<host> -o "IdentitiesOnly=yes" -i <path to identity key>

## SSH File Transfer Protocol
SSH File Transfer Protocol (SFTP) is a file transfer protocol (FTP) implemented in SSH. It allows a user to transfer files over the SSH tunnel. <br>
Given the public key is stored in the server, the following command can be used to connect to the server using SFTP.

        sftp -P <port> -o "IdentitiesOnly=yes" -i <path to identity key> <username>@<host>
        
*Note that capital P is used for the port parameter and the \<username\>@\<host\> is given at the end of the command.*

Once the SFTP session is established, the command line will show "sftp\>" on the command line input. Now all commands entered are executed in the server.<br>
To execute the commands on the client, put "l" (for local) in front of the commands as below:
  
        # remote command
        sftp\> pwd
        Remote working directory: /home/cdsw
        # local command
        sftp\> lpwd
        Local working directory: c:\users\user\.ssh
  
### Transfer Files from Server to Client
`get` command can be used to fetch a file from server to client. The working directories of the server and the client will be used as the source and the destination.
  
        sftp\> get <source file> <destination file (if not given, same name is used)>
        sftp\> get -r <directory>

*Note that "-r" is used to fetch child files in the directory recursively.*
  
### Transfer Files from Client to Server
`put` command can be used to push a file from client to server.
  
        sftp\> put <source file> <destination file>
        sftp\> put -r <directory>

## Remote Development on VS Code via SSH
VS code can be used for remote development using SSH extension. The following steps can be taken to establish a SSH tunnel to the remote server.

1. Install remote development extension pack (ex. Remote - SSH)
2. Select "Remote-SSH: Connect to Host" from Command Palette (F1, Ctrl+Shift+P)
3. Either input ssh command or choose the configured host name (from `Config File` section)

### Port Forwarding on VS Code
Port forwarding can be done using VS Code to allow the client to open up any applications using the forwarded port.<br>
This can be used to access the server's Jupyter Notebook or Airflow webserver on the client.

Port forwarding can be done in the Remote Explorer from the activity bar.
<img width=600px src=https://user-images.githubusercontent.com/46085656/186347489-911dadd9-47e2-41b1-8e69-d28d62405aa2.png>

## Connect to Cloudera CDSW
Cloudera CDSW provides remote development via SSH. The following steps can be taken to create a new session and develop remotely.

1. Store public key in Cloudera CDSW setting.
2. Install CDSW CLI client
3. Use CDSW CLI to log in

        cdswctl login -n <username> -u http(s)://cdsw.your_domain.com

5. Create a new session on CDSW

        cdswctl ssh-endpoint -p <username>/<project name> [-c <CPU_cores>] [-m <memory in GB>] [-g <number of GPUs>] [-r <runtime ID> ]
        
*runtime refers to the session specification (ex. Python version). Available runtimes can be viewed using `cdswctl runtimes list`

7. Connect to CDSW server using development tool (ex. VS Code) for remote development. Upon creation of a new session, connection information is required as below.

        ssh -p 2024 cdsw@localhost
