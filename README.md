# Configuring VM resource settings

Note: The commands below modify this file:
/etc/sysctl.conf

```
sysctl -w vm.max_map_count=262144
sysctl -w fs.file-max=65536
ulimit -n 65536
ulimit -u 4096
```

# Usage

* First start - creates network

	`dokcer-compose up`
* Stop - containers can be restarted

	`docker-compose stop`
* Restart containers

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
	-it adambhun/sh-sonarscanner-4.0.0-full \
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
