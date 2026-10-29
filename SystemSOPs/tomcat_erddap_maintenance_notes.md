# Maintenance Log: [Tomcat for ERDDAPS with EcoFOCI]

**Current Version:** v1.0.0
**Last Updated:** 2025-01-25
**Status:** [Active / Maintenance Mode / Deprecated]

---
## Info
*tomcat with ecofoci is used predominatly to run erddap instances.  We currently run three (ports: 8080/8005 {tomcat}, 8081/8006 {tomcat_prod}, 8082/8007 {tomcat_dev})

Software impacted
* tomcat instance
* erddap.war
* JRE

systemctl daemons can be / are established to autostart and manage processes 
eg. `systemctl start tomcat`

---

## 📋 Update Procedure (SOP)
*Steps to take when applying an update to tomcat, this is written for the `tomcat` instance but the other two instances `tomcat_prod` and `tomcat_dev` are functionally equivalent*

1.  **Backup:** copy tomcat folder tomcat_bkup.
2.  **Obtain software:** download relevant tomcat `wget https://dlcdn.apache.org/tomcat/tomcat-11/v11.0.15/bin/apache-tomcat-11.0.15.tar.gz`
3.  **Unpack and modify configurations:** ports need to be changed in `conf/server.xml`, CorsFilter needs to be modified in `conf/web.xml`, and `bin/setenv.sh` needs to be modified ***see below***,
4.  **Migrate erddap:** copy `webapps/erddap.war` or download it again from github [https://github.com/ERDDAP/erddap](https://github.com/ERDDAP/erddap) and copy the entire `{tomcat}/content` directory from your backup to the new installation
5.  **Deploy:** Restart the service using `systemctl restart tomcat`.
6.  **Test:** Test by going to erddap instance
7.  **Cleanup:** Remove backup folder and proceed to next erddap instance

> **Note: server.xml**
***port and shutdown port***
tomcat: 8080:8005 
tomcat_prod: 8081:8006 
tomcat_dev: 8082:8007

```xml
    <Connector port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" />
```
```xml
<Server port="8005" shutdown="SHUTDOWN">
```

> **Note: CorsFilter** make sure this setting in apache is existant
```xml
  <!-- ================== Built In Filter Definitions ===================== -->
<filter>
    <filter-name>CorsFilter</filter-name>
    <filter-class>org.apache.catalina.filters.CorsFilter</filter-class>
  <init-param>
    <param-name>cors.allowed.origins</param-name>
    <param-value>*</param-value>
  </init-param>
</filter>
<filter-mapping>
    <filter-name>CorsFilter</filter-name>
    <url-pattern> /* </url-pattern>
</filter-mapping>
```
> **Note: setupenv.sh** make sure this file is in the `tomcat/bin` directory
```shell
export JAVA_OPTS="-server -Xmx8G -Xms1G -Djava.awt.headless=true"
export JAVA_HOME=/opt/current_jdk/
export TOMCAT_HOME=/opt/tomcat
export CATALINA_HOME=/opt/tomcat
export CATALINA_PID=/opt/tomcat/bin/catalina.pid
export SERVER_PORT=8080
```

> **Note: FINAL NOTE** UPGRADE each tomcat instance independently.  Don't try to get too smart and shortcut steps by taking down the instances all at once or copying from one instance to another.  setting regarding each unique port are setup with erddaps `setup.xml` and this could come back to bite you.

---

## 📝 Change Log
### [v1.0.0] - 2025-01-25
*Initial log development*

**Added**

**Changed**
* updated tomcat to tomcat 11.0.15 on all three instances
* current erddap version: `2.29.0`
* current java JDK version: `jdk-25.0.1+8-jre`

**Fixed**

---

## 🚀 Roadmap / Backlog
* [ ] **High Priority:** implement https
* [ ] **Low Priority:** 
* [ ] **Idea:** 

---

## 🐛 Known Issues / Troubleshooting
* **Issue:** Inusfficient resources to heavily use all three erddaps.
    * **Workaround:** Restart failed tomcat instance.
    * **Solutin:** More memory or auto restart instance
