# Docker image for SonarQube Runner

[SonarQube Runner](https://github.com/Sonarsource/sonar-runner) is recommended as the default launcher to analyse a project with [SonarQube](http://www.sonarqube.org/).

### Contents

 - Usage
	 - Option 1 – Run SonarQube Runner against dockerised SonarQube
	 - Option 2 – Run SonarQube Runner against regular SonarQube
 - Build
	 - Build with Docker
	 - Build with Docker Compose
 - About

## Usage

Create a `sonar-project.properties` file in the directory containing the project to be analysed, as described on the [Analyzing with SonarQube Runner](http://docs.sonarqube.org/display/SONAR/Analyzing+with+SonarQube+Runner) page of SonarQube's documentation site.

Here's an sample file for a project named *My project*, with code to be analysed in the `app` subdirectory of the project:
	
	sonar.projectKey=my:project
	sonar.projectName=My project
	sonar.projectVersion=1.0
	sonar.sources=app
	sonar.sourceEncoding=UTF-8

Then `cd` to the project's directory and run SonarQube Runner using one of the two options presented below.

### Option 1 – Run SonarQube Runner against dockerised SonarQube

**Prerequisite** – Start a SonarQube container (e.g. `sonarqube`) with ports 9000 and 9092 exposed, and get its name (e.g. `sonarqubedocker_sonarqube_1` in the example below).

In the command below, adapt:

- the `--link` option to use the actual name of the SonarQube container,    

- the `-Dsonar.* options` as needed.

Here it goes...

	docker run --link sonarqubedocker_sonarqube_1:sonarqube \
	  --entrypoint /opt/sonar-runner-2.4/bin/sonar-runner \
	  -e SONAR_USER_HOME=/data/.sonar-cache \
	  -v $(pwd):/data -u $(id -u) sebp/sonar-runner \
	    -Dsonar.host.url=http://sonarqube:9000 \
	    -Dsonar.jdbc.url=jdbc:h2:tcp://sonarqube/sonar \
	    -Dsonar.jdbc.username=sonar \
	    -Dsonar.jdbc.password=sonar \
	    -Dsonar.jdbc.driverClassName=org.h2.Driver \
	    -Dsonar.embeddedDatabase.port=9092

### Option 2 – Run SonarQube Runner against regular SonarQube

Run the command below, adapting the `-Dsonar.* options` as needed, especially the `-Dsonar.host.url` and `-Dsonar.jdbc.url` options, where the example value `sonarqube` has to be replaced with the hostname of your SonarQube server.   

	docker run --entrypoint /opt/sonar-runner-2.4/bin/sonar-runner \
	  -e SONAR_USER_HOME=/data/.sonar-cache \
	  -v $(pwd):/data -u $(id -u) sebp/sonar-runner \
	    -Dsonar.host.url=http://sonarqube:9000 \
	    -Dsonar.jdbc.url=jdbc:h2:tcp://sonarqube/sonar \
	    -Dsonar.jdbc.username=sonar \
	    -Dsonar.jdbc.password=sonar \
	    -Dsonar.jdbc.driverClassName=org.h2.Driver \
	    -Dsonar.embeddedDatabase.port=9092

## Build

First clone or download the [spujadas/sonar-runner-docker](https://github.com/spujadas/sonar-runner-docker) GitHub repository, open a shell in the newly created `sonar-runner-docker` directory, then build the image and run a container using Docker or Docker Compose, as explained below.

### Build with Docker

This command will build the image:

	$ sudo docker build .

### Build with Docker Compose

Build the image with this command:

	$ sudo docker-compose build


## About

Written by [Sébastien Pujadas](http://pujadas.net), released under the [under the GNU Lesser GPL License, Version 3](http://www.gnu.org/licenses/lgpl.txt).
