# gradle-build

Generic Gradle project, for building Docker images, using Packer, Ansible, and Serverspec.
The intent is to clone this as a submodule project into other projects

## Requirements

- [Gradle](https://gradle.org/install/#manually)
- [Packer](https://packer.io/)
- [Ansible](https://www.ansible.com/)
- [Ruby](https://www.ruby-lang.org/en/documentation/installation/)
- [Serverspec](https://serverspec.org/): gem install serverspec
- [docker-api](https://github.com/swipely/docker-api/releases): gem install docker-api

## Install

```shell
git submodule add https://github.com/apolloclark/gradle-build

# create the project specific Gradle config files, using "packer-nodejs" as an example:
[settings.gradle]
rootProject.name = 'packer-nodejs'
include ':gradle-build:centos7.6', ':gradle-build:ubuntu18.04', ':gradle-build:ubuntu16.04'

[gradle.properties]
package_name=nodejs
package_version=12.6.0
docker_username=apolloclark
docker_email=apolloclark@gmail.com

# set your Docker hub username
export DOCKER_USERNAME="apolloclark" # $(whoami)
export DOCKER_PASSWORD=""



# Gradle, build all images, in parallel
gradle test --parallel --project-dir gradle-build

# Gradle, build all images, in parallel, forced rebuild
gradle test --parallel --rerun-tasks --project-dir gradle-build

# Gradle, build only specific OS images
gradle ubuntu18.04:test --project-dir gradle-build
gradle ubuntu16.04:test --project-dir gradle-build
gradle centos7:test --project-dir gradle-build

# Gradle, build only specific OS images, forced rebuild
gradle ubuntu18.04:test --rerun-tasks --project-dir gradle-build
gradle ubuntu16.04:test --rerun-tasks --project-dir gradle-build
gradle centos7:test --rerun-tasks --project-dir gradle-build

# Gradle, clean up previous builds, from today
gradle clean --parallel --project-dir gradle-build

# clean up ALL previous builds
./clean_packer_docker.sh



# Gradle, list tasks, and dependency graph
gradle tasks --project-dir gradle-build
gradle tasks --all --project-dir gradle-build
gradle test taskTree --project-dir gradle-build
gradle buildParallel taskTree --project-dir gradle-build

# Gradle, debug
gradle properties

gradle ubuntu16.04:info --project-dir gradle-build

gradle ubuntu16.04:test --project-dir gradle-build --info --rerun-tasks

rm -rf ~/.gradle
```
ejs.org/en/download/releases/
```
