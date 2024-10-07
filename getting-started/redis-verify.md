# REDIS Verify

You will need to install redis-cli for connecting to REDIS server to fetch your session to verify it is your session. To install redis-cli, there is various way to install it. I have use note.js to install redis-cli in your windows powershell. You can refer to 6.3.2 for REDIS-CLI installation.  
After you have redis-cli installed, issue this command to connect to REDIS server with Windows Powershell  

```shell
rdcli -h 172.26.92.26 -p 6379 -a redis@sds
```  
You will see this patern of command line when you have successful connect to REDIS server. It is IP follow up by port with colon separated. 
```shell
172.26.92.26:6379>
```
To list the available sessions in server, you can run the following command.  
```shell
key *
```  
It will list all the available sessions in server. If you have set the prefix, it will be listed with your prefix with following pattern, _prefix_sessionid.  

You can also list the session of certain pattern of your prefix. For example  
```shell
key _prefix_*
```  

