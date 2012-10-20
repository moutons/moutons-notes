# Apache Tomcat
## and Java


*a useful snippet to monitor JVM memory* [1] [1]
	// Memory status
	Runtime         runtime =       Runtime.getRuntime();
	final long      totalMem =      runtime.totalMemory();
	final long      freeMem =       runtime.freeMemory();
	if (log.isDebugEnabled()) {
		log.debug("Memory free=" + freeMem + 
		" used=" + (totalMem - freeMem) + 
		" total=" + totalMem);
	}

*a good way to modify heap size* [2] [1]
Create `%CATALINA_HOME%\bin\setenv.sh` and put environment variables there:
	export CATALINA_OPTS="-Xms512m -Xmx512m"
or
	export JAVA_OPTS="-Xmx512m"
etc.

*another setenv.sh recommendation* [3] [2]
	JAVA_OPTS="-Djava.awt.headless=true -Dfile.encoding=UTF-8 
	-server -Xms512m -Xmx1024m
	-XX:NewSize=256m -XX:MaxNewSize=256m -XX:PermSize=256m 
	-XX:MaxPermSize=256m
	-XX:+DisableExplicitGC 
	-XX:+UseConcMarkSweepGC"
Use `-XX:+UseConcMarkSweepGC` for multiple cores, apparently. The rest should be more or less self-explanatory.

*some good general tips* [4] [3]
1.  To create heap dumps on OOM errors, 1.5+ JVM:
		-XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=/home/j2ee/heapdumps
2.  Custom error pages to hide raw exception messages from users - add to `web.xml`
		<error-page>
			<error-code>404</error-code>
			<location>/error/404.html</location>
		</error-page>
3.  Split Tomcat installation for flexibility when upgrading: see the *Advanced Configuration - Multiple Tomcat Instances* section in the RUNNING.txt file of the Tomcat distribution.
4.  With multi-core/multiple CPUs, **increase the thread pool beyond the default 250**.
	An interesting problem is brought up [here] [http://stackoverflow.com/questions/2761618/why-is-my-tomcat-6-executor-thread-pool-not-being-used-by-the-connector]. Hopefully it doesn't crop up.
5.  Also, [blah] [http://blog.springsource.org/2008/08/08/optimising-and-tuning-apache-tomcat/] [blah] [http://jmeter.apache.org/index.html] [blah] [http://www.junit.org/] [blah] [http://www.lambdaprobe.org/] [blah] [http://www.springsource.org/]. There's never enough time.

[1]: http://stackoverflow.com/questions/2718786/how-to-increase-java-heap-space-for-a-tomcat-app	"Stack Overflow link 1"
[2]: http://stackoverflow.com/questions/6398053/cant-change-tomcat-7-heap-size/10950387#10950387	"Stack Overflow link 2"
[3]: http://digitalsanctum.com/2007/08/18/20-tips-for-using-tomcat-in-production/			"20 Tips for using Tomcat in Production"
