# Demonstration of using Payara Server with the Maven Cargo plugin

## Quick start

Start Payara server at the standard admin port 4848 and HTTP listener port 8080 and run:

```
mvn install -Ppayara4x-remote
```

This will do:

1. build the test application WAR
2. deploy it to the running Payara Server
3. perform an integration test to check it's deployed
4. undeploy the application
5. verify the results of the previous tests - build will fail if the application couldn't be deployed

## Using the Cargo plugin

The Cargo plugin (`cargo-maven2-plugin`) is preconfigured in the `pluginManagement` element.

Executions:
 - `start-cargo` - runs goal start to start a local server in background (not possible with remote deployment type)
 - `stop-cargo` - runs goal stop to stop the server running in background (not possible with remote deployment type)
 - `redeploy-cargo` - runs goal redeploy to deploy the built application (will undeploy if needed)
 - `undeploy-cargo` - runs goal undeploy to undeploy the built application

The property `cargo.servlet.port` is configured to the value of `servlet.port` maven property.

In order to run one of the goals during the build lifecycle, it's only necessary to redefine one of the executions. For example, to bind the redeploy goal to the install phase, add the following execution to the `cargo-maven2-plugin` configuration:

```
<executions>
  <execution>
    <id>redeploy-cargo</id>
    <phase>install</phase>
  </execution>
...
```

Additionally, one of the maven profiles must be turned on to configure server-specific cargo plugin configurtion, so that it knows how and where to deploy the application.

## Payara4x-remote profile

This profile configures the cargo plugin to work with GlassFish/Payara Server. This profile operates in remote mode, which means that the server needs to be manually started or already running and accessible via the asadmin port.