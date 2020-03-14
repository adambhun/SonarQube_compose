# Prerequisites
## Linux

Set the maximum VM map count with this command:

`sysctl -w vm.max_map_count=262144`


### Notes:

*This persists for the current session only (modifies `/proc/sys/vm/max_map_count`)

*To modify this kernel parameter permanently, append: `vm.max_map_count=262144` to `/etc/sysctl.conf`

	Note: if this fails the setting has probably been overridden by a file in `/etc/sysctl.d` that has the
 	same setting defined in it.


# Usage

* First start - creates network

	`dokcer-compose up`
* Stop - containers can be restarted

	`docker-compose stop`
* Start stopped containers

	`docker-compose start`
* Stop and remove everything

	`docker-compose down`
* Start scanning - from this folder. Excluded xmls from every folder to provide an example. "sonar.sources" is the name of the folder that should contain the files to analyze

<!-- TODO: volume -->
```
docker run --net sonarnet \
	--user="$(id -u):$(id -g)" \
	-v "$PWD:/usr/src" \
	-e shellcheck=/usr/bin/shellcheck \
	-it adambhun/sh-sonarscanner-4.0.0-full:latest \
	-Dsonar.projectKey="faszkosar" \
	-Dsonar.projectName="faszkosar" \
	-Dsonar.projectVersion="1" \
	-Dsonar.exclusions="**/*.xml" \
	-Dsonar.sources="to_analyze"
```


# Other notes
* Quality profiles can be added manually via SonarQube's webapp. Let me know if you know where SonarQube stores them.
* Send a pull request if you manage to get the alpine-based SonarScanner to work with Shellcheck.
* The Dockerfile is for SonarScanner. It's the same as Sonarsource's but has Shellcheck installed.
* sonarproject.properties file does not work - except the basedir property
* https://docs.sonarqube.org/latest/analysis/analysis-parameters/
