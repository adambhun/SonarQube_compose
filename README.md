# Prerequisites

A v3.7 compose file needs Docker Engine 19.03 and docker-compose 1.25.5, though the content of the
 docker-compose.yml file is probably backward compatible with older versions.


The Sonarqube and PostgreSQL containers need directories with write permissions - hence the need for the "gitkeep" 
files. These are:

* sonarqube_conf
* sonarqube_data
* sonarqube_extensions
* postgresql


Set the maximum VM map count with this command:

`sysctl -w vm.max_map_count=262144`


Notes:

* This persists for the current session only (modifies `/proc/sys/vm/max_map_count`)

* To modify this kernel parameter permanently, append: `vm.max_map_count=262144` to `/etc/sysctl.conf`
		<br> Note: if this fails the setting has probably been overridden by a file in `/etc/sysctl.d`
		that has the same setting defined in it.

Setting the maximum VM map count should be enough on most systems. If not, the following instructions might help you:

* Increase the maximum number of open files:
		`sysctl -w fs.file-max=65536`

* Set the maximum file descriptors for a process:
		`ulimit -n 65536`

* Set the maximum number of user processes:
		`ulimit -u 4096`

# Usage

* First start - creates network and named volumes (in background)

	`docker-compose up -d`
* Stop - containers can be restarted

	`docker-compose stop`
* Restart containers

	`docker-compose start`
* Stop and remove containers and the network, but leave the volumes.

	`docker-compose down`
* Start scanning - from this folder. Excluded xmls from every folder to provide an example.
 "sonar.sources" is the name of the folder that should contain the files to analyze


```
docker run --net sonarnet \
	--user="$(id -u):$(id -g)" \
	-v "$PWD:/usr/src" \
	-e shellcheck=/usr/bin/shellcheck \
	-it adambhun/sh-sonarscanner-4.0.0-full \
	-Dsonar.projectKey="myproject" \
	-Dsonar.projectName="myproject" \
	-Dsonar.projectVersion="1" \
	-Dsonar.exclusions="**/*.xml" \
	-Dsonar.sources="to_analyze"
```


# Configuration

* Quality profiles can be added manually via SonarQube's webapp. Let me know if you know where SonarQube stores them.
* [SonarScanner configuration](https://docs.sonarqube.org/latest/analysis/analysis-parameters/)


# Other notes

* Send a pull request if you manage to get the alpine-based SonarScanner to work with Shellcheck.
* The Dockerfile is for SonarScanner. It's the same as Sonarsource's but has Shellcheck installed.
* sonarproject.properties file does not work - except the basedir property
