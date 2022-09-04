# PythonSSHLocalhost

 Start your own ssh client on localhost with python


Python 3 port of the original python 2 [script](https://gist.github.com/michaellihs/d2070d7a6d3bb65be18c) by [michaellihs](https://gist.github.com/michaellihs) based on the discussion in the comments of the original script.

## Installing the library

Assuming you have Python 3 installed on your system:

```` shell
pip install twisted
pip install pyOpenSSL
pip install service_identity
````

## RSA Keys

For convenience, you can store the RSA keys in the `keys` folder

To generate them, make a `keys` folder and run 

```` shell
ssh-keygen -t rsa -b 4096 -C "server" -f keys/id_rsa_localhost
````

If you want to test also key-based user authentication, also run 

```` shell
ssh-keygen -t rsa -b 4096 -C "client" -f keys/id_rsa_client
````

## Start the server

```` shell
python3 start_local_ssh_server.py
````

## Connect to the server

In another shell, run

```` shell
ssh user@localhost -p 22222
 
((password 'password'))
 
>>> Welcome to my test SSH server.
Commands: clear echo help quit whoami
$
````

If you generated client keys this should also work 
 
 ```` shell
ssh key_user@localhost -p 22222 -i id_rsa_client
````

## Testing commands

The above server is implemented in such a way, that it outputs all commands to STDOUT of the python process running the ssh server. This may be useful for testing the commands sent to the server.

The shell will run any command for which the do_command method is defined in  `SSHDemoProtocol `. An example is 

 ```` python
def do_echo(self, *args):
        self.terminal.write(" ".join(args))
        self.terminal.nextLine()
````

which runs as

 ```` shell
$ echo a
a
$ 
````