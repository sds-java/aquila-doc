# REDIS

There is 2 step require to configure the Tomcat session to connect REDIS server and store the session. 

1. REDIS compiled jar file “rsm-redisson-with-dependencies-3.1.0.jar” in Tomcat/lib and context.xml configuration in Tomcat/conf.  

**Download REDIS JAR**  
<a href="/sdsfw_docs/downloads/redis/tomcat7/tomcat7.zip" download>REDIS for Tomcat 7.x</a>  
<a href="/sdsfw_docs/downloads/redis/tomcat7/tomcat8.zip" download>REDIS for Tomcat 8.x</a>  

2. The file “rsm-redisson-with-dependencies-3.1.0.jar” can be found in “tomcat8/libs” SVN. Notes, try to copy to “tomcat8/bin” folder if it cannot detect the “rsm-redisson-with-dependencies-3.1.0.jar” from “tomcat8/libs” folder.
From “Tomcat/Conf/context.xml” add the following to the “<context>...</context>”  
```xml
<Manager className="com.crimsonhexagon.rsm.redisson.SingleServerSessionManager"
	endpoint="redis://172.26.92.26:6379"
	connectionPoolSize="100"
	password="redis@sds"
	timeout="60000"
	retryAttempts="20"
	retryInterval="1000"
	sessionKeyPrefix="_rsm_"
	saveOnChange="true"
	forceSaveAfterRequest="false"
	dirtyOnMutation="false"
	ignorePattern=".*\\.(ico|png|gif|jpg|jpeg|swf|css|js)$"
	maxSessionAttributeSize="-1"
	maxSessionSize="-1"
	allowOversizedSessions="false"/>

```  
| Key 					| Value 									| Description 																								| 
| --------------------- | ----------------------------------------- | --------------------------------------------------------------------------------------------------------- |
| sessionKeyPrefix | String | prefix for redis keys. Useful for  situations where 1 redis cluster serves multiple application clusters  with potentially conflicting session IDs. Recommend to put your system short name do verification so you can identify the session name is your or not. |
| saveOnChange | true/false | the session will be persisted to redis immediately when any attribute is modified. When false, a modified session is persisted to redis when the request is complete. |
| forceSaveAfterRequest | true,/false | the session will  be persisted to redis when the request completes regardless of whether  the session has detected a change to its state. |

·	dirtyOnMutation: see "Notes on object mutation" at miscellaneous .
·	ignorePattern: Java Pattern String to be matched against the request URI (does not include the query string). If matched, the request will not be processed by the redis session manager.
·	maxSessionAttributeSize: if not -1 (RedisSessionManager#DO_NOT_CHECK) specifies a maximum encoded size for a session attribute value. Attributes larger than this size will be logged and will not be stored in the session.
·	maxSessionSize: if not -1 (RedisSessionManager#DO_NOT_CHECK) specifies a maximum encoded size for the entire session. Sessions larger than this size will be logged and will not be persisted to redis.
·	allowOversizedSessions: if true will allow sessions exceeding the configurations in maxSessionAttributeSize and maxSessionSize to be saved. An error will still be logged for any sessions exceeding the size. This attribute has no effect if neither maxSessionAttributeSize nor maxSessionSize are specified.
·	password: Password for REDIS  server authentication. Default is null.
·	endpoint: hostname:port of the redis server. Must be a primary endpoint (read/write) and not a read replicate (read-only).
·	connectionPoolSize: Redis connection maximum pool size. Default value: 64
·	database: Database index used for Redis connection. Default value: 0
·	timeout: Redis server response timeout. Starts to countdown when Redis command was succesfully sent. Value in milliseconds.Default value: 3000
·	retryAttempts: Error will be thrown if Redis command can't be sended to Redis server after retryAttempts. But if it sent succesfully then timeout will be started. Default value: 3
·	retryInterval: Time interval after which another one attempt to send Redis command will be executed. Value in milliseconds. Default value: 1500
