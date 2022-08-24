# SSH
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

        ssh-copy-id -i ~/.ssh/tatu-key-ecdsa user@host
        
### SSH connection using Public Key Authentication
After the authentication key pair is generated and the public key is copied to the server, the following command can be used to establish 
a SSH tunnel to the server.

        ssh -i <path of private key> -d <port> <username>@<host>

### Config File
Configuration file for the SSH connection can be set up for quick connection on the client.

        # config file
        Host <define name for the connection ex.cdsw>
          HostName <remote server hostname ex.localhost>
          IdentitiesOnly <whether to use only passed secret key | ex.yes>
          IdentityFile <location of secret key | ex.c:/users/hongj5/.ssh/ssh>
          User <remote server username | ex.cdsw>
          Port <port number | ex.2024>

The following command can be used to connect to the server using the config file.

        ssh cdsw
  
*Note that when there are multiple secret keys on computer, enable "IdentitiesOnly" parameter to use only the given secret key for the connection.*

## Remote Development on VS Code via SSH
VS code can be used for remote development using SSH extension. The following steps can be taken to establish a SSH tunnel to the remote server.

1. Install remote development extension pack (ex. Remote - SSH)
2. Select "Remote-SSH: Connect to Host" from Command Palette (F1, Ctrl+Shift+P)
3. Either input ssh command or choose configured host name (from `Config File` section)

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

        cdswctl ssh-endpoint -p <username>/<project_name> [-c <CPU_cores>] [-m <memory_in_GB>] [-g <number_of_GPUs>] [-r <runtime ID> ]
        
*runtime refers to the session specification (ex. Python version). Available runtimes can be viewed using `cdswctl.ext runtimes list`

7. Connect to CDSW server using development tool (ex. VS Code) for remote development. Upon creation of a new session, connection information is required as below.

        ssh -p 2024 cdsw@localhost
