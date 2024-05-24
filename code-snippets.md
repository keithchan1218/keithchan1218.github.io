---
title: CODE SNIPPETS
subtitle: by Keith
---

# CODE SNIPPETS

This is a page for my code snippet notes.

## Python

````python
```python
import paramiko
import getpass

HOST = "172.16.100.59"
PORT = 22
USERNAME = "kali"

class SSHConnection(object):

    # def __init__(self,pwd=input("Enter your password: ")):
    def __init__(self,pwd=getpass.getpass("Enter your password: ")):
        self.host = HOST
        self.port = PORT
        self.username = USERNAME
        self.pwd = pwd
        self.__k = None

    def connect(self):
        transport = paramiko.Transport((self.host,self.port))
        transport.connect(username=self.username,password=self.pwd)
        self.__transport = transport

    def close(self):
        self.__transport.close()

    def upload(self, local_path, target_path):
        try:
            sftp = paramiko.SFTPClient.from_transport(self.__transport)
            sftp.put(local_path, target_path)
            print("File uploaded successfully!")
        except Exception as e:
            print("File upload failed:", str(e))

    def download(self, remote_path, local_path):
        try:
            sftp = paramiko.SFTPClient.from_transport(self.__transport)
            sftp.get(remote_path, local_path)
            print("File downloaded successfully!")
        except Exception as e:
            print("File download failed:", str(e))

    def cmd(self, command):
        ssh = paramiko.SSHClient()
        ssh._transport = self.__transport
        stdin, stdout, stderr = ssh.exec_command(command)
        result = stdout.read()
        print (str(result,encoding='utf-8'))
        return result

try:
    ssh = SSHConnection()
    ssh.connect()
    # ssh.cmd("ls")
    ssh.upload('testing.txt','/tmp/testing.txt')
    # ssh.download('/tmp/testing.txt','testing.txt',)
    ssh.close()
except Exception as e:
    print("Exception:", str(e))
```
````
