# REDIS

There is 2 step require to configure the Tomcat session to connect REDIS server and store the session. REDIS compiled jar file “rsm-redisson-with-dependencies-3.1.0.jar” in Tomcat/lib and context.xml configuration in Tomcat/conf.  

The file “rsm-redisson-with-dependencies-3.1.0.jar” can be found in “tomcat8/libs” SVN. Notes, try to copy to “tomcat8/bin” folder if it cannot detect the “rsm-redisson-with-dependencies-3.1.0.jar” from “tomcat8/libs” folder.
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
**Download REDIS JAR**  
<a href="/sdsfw_docs/downloads/redis/tomcat7/tomcat7.zip" download>REDIS for Tomcat 7.x</a>  
<a href="/sdsfw_docs/downloads/redis/tomcat7/tomcat8.zip" download>REDIS for Tomcat 8.x</a>  