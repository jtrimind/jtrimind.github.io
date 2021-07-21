---

title:  "[해결법] in `bind': Address already in use - bind(2) for 127.0.0.1:4000 (Errno::EADDRINUSE)"
---

## EADDRINUSE
- `127.0.0.1:4000`을 사용하고 있는 상태에서 다시 사용하려고 해서 발생한다.
- 일반적으로 이전에 사용하고 있던 것이 정상적으로 종료되지 않아서 발생한다.
- `netstat -lntp`를 쳐서 해당 어드레스를 사용하고 있는 PID를 찾는다.
- `kill -9 (해당PID)`를 해서 강제로 종료한다.

```terminal
/usr/lib/ruby/2.5.0/socket.rb:201:in `bind': Address already in use - bind(2) for 127.0.0.1:4000 (Errno::EADDRINUSE)
$ netstat -lntp
(Not all processes could be identified, non-owned process info
 will not be shown, you would have to be root to see it all.)
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
tcp        0      0 127.0.0.1:4000          0.0.0.0:*               LISTEN      28275/ruby2.5
$ kill -9 28275
```
