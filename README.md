# JFrog as a local Maven proxy building OpenNMS

This repository is used to setup a local JFrog instance to cache Maven artifacts which are downloaded during a build from OpenNMS.

## Installation and Usage

**Step 1**: Checkout the Docker Compose file and start the service

```shell
git clone https://github.com/opennms-forge/docker-jfrog-proxy.git
cd docker-jfrog-proxy
docker-compose up -d
```

**Step 2**: Initial setup

Navigate to http://localhost:8081 and set a password and finish initialization wizard.

**Step 3**: Configure repositories

Login with an admin account to JFrog and go to "Admin -> Advanced -> Config Descriptor" and overwrite
with the content from the file in the repository `config-descriptor.xml`.

**Step 4**: Configure your maven to use the proxy

Copy the `sample-settings.xml` to your local Maven home as `~/.m2/settings.xml`.

**Step 5**: Fill the cache with the first build of OpenNMS

```
git clone https://github.com/OpenNMS/opennms.git
cd opennms
./clean.pl && ./compile.pl -U -DskipTests && ./assemble.pl -Dopennms.home=/opt/opennms -DskipTests
```

The first run downloads all artifacts from the internet and can take a while.
Any following build will run against a populated cache which is served quickly over localhost.
