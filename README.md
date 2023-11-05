# Canoa Webapp Runner

Command line tool for starting a Jakarta EE web application programmatically.
It currently supports Tomcat 10.0.

## Table of Contents

- [Usage](#usage)
    * [Maven](#maven)
- [Using behind a reverse proxy server](#using-behind-a-reverse-proxy-server)
- [Options](#options)
- [Development](#development)

## Usage

Canoa Webapp Runner is a standalone CLI tool that runs a WAR file. The simplest usage looks like this:

```
$ java -jar webapp-runner.jar path/to/project.war
```

### Maven

You can use the Maven dependency plugin to download Webapp Runner as part of your build. This will eliminate the need 
for any external dependencies other than those specified in your build to run your application.

```xml
<plugins>
  <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-dependency-plugin</artifactId>
      <version>3.3.0</version>
      <executions>
          <execution>
              <phase>package</phase>
              <goals>
                  <goal>copy</goal>
              </goals>
              <configuration>
                  <artifactItems>
                      <artifactItem>
                          <groupId>com.talenteca</groupId>
                          <artifactId>canoa-webapp-runner</artifactId>
                          <version>${canoa-webapp-runner.version}</version>
                          <destFileName>canoa-webapp-runner.jar</destFileName>
                      </artifactItem>
                  </artifactItems>
              </configuration>
          </execution>
      </executions>
  </plugin>
</plugins>
```

Now when you run `mvn package`, Webapp Runner will be downloaded for you. You can then launch your application with:

```
$ java -jar target/dependency/canoa-webapp-runner.jar target/<appname>.war
```

## Using behind a reverse proxy server

If you are using canoa-webapp-runner behind a proxy server, you can set the proxy base url within tomcat:

```
$ java -jar canoa-webapp-runner.jar --proxy-base-url https://example.com target/<appname>.war
```

If you pass an HTTPS base url, e.g. https://example.com, secure flag will be automatically added to session cookies. This indicates to the browser that cookies should only be sent over a secure protocol.

## Options

```
Usage: <main class> [options]
  Options:
    --access-log
      Enables AccessLogValue to STDOUT
      Default: false
    --access-log-pattern
      If --access-log is enabled, sets the logging pattern
      Default: common
    --basic-auth-pw
      Password to be used with basic auth. Defaults to BASIC_AUTH_PW env
      variable.
    --basic-auth-user
      Username to be used with basic auth. Defaults to BASIC_AUTH_USER env
      variable.
    --bind-on-init
      Controls when the socket used by the connector is bound. By default it
      is bound when the connector is initiated and unbound when the connector
      is destroyed., default value: true
      Default: true
    --compressable-mime-types
      Comma delimited list of mime types that will be compressed when using
      GZIP compression.
      Default: text/html,text/xml,text/plain,text/css,application/json,application/xml,text/javascript,application/javascript
    --context-xml
      The path to the context xml to use.
    --enable-basic-auth
      Secure the app with basic auth. Use with --basic-auth-user and
      --basic-auth-pw or --tomcat-users-location
      Default: false
    --enable-client-auth
      Specify -Djavax.net.ssl.keyStore and -Djavax.net.ssl.keyStorePassword in
      JAVA_OPTS
      Default: false
    --enable-compression
      Enable GZIP compression on responses
      Default: false
    --enable-naming
      Enables JNDI naming
      Default: false
    --enable-ssl
      Specify -Djavax.net.ssl.keyStore, -Djavax.net.ssl.keystoreStorePassword,
      -Djavax.net.ssl.trustStore and -Djavax.net.ssl.trustStorePassword in
      JAVA_OPTS. Note: should not be used if a reverse proxy is terminating
      SSL for you (such as on Heroku)
      Default: false
    --expand-war-file
      Expand the war file and set it as source
      Default: true
    --expanded-dir-name
      The name of the directory the WAR file will be expanded into.
      Default: expanded
    --help

    --max-threads
      Set the maximum number of worker threads
      Default: 0
    --memcached-transcoder-factory-class
      The class name of the factory that creates the transcoder to use for
      serializing/deserializing sessions to/from memcached.
    --path
      The context path
      Default: <empty string>
    --port
      The port that the server will accept http requests on.
      Default: 8080
    --proxy-base-url
      Set proxy URL if tomcat is running behind reverse proxy
      Default: <empty string>
    --scanBootstrapClassPath
      Set jar scanner scan bootstrap classpath.
      Default: false
    --session-store
      Session store to use (default is none)
    --session-store-ignore-pattern
      Request pattern to not track sessions for. Valid only with
      session store. (default is '.*\.(png|gif|jpg|css|js)$')
      Default: .*\.(png|gif|jpg|css|js)$
    --session-store-locking-mode
      Session locking mode for use with session store. (default is
      all)
      Default: all
    --session-store-operation-timeout
      Operation timeout for the session store. (default is 5000ms)
      Default: 5000
    --session-store-pool-size
      Pool size of the session store connections (default is 10.)
      Default: 10
    --session-store-ssl-endpoint-identification
      Enables or disables SSL endpoint identification for the session
      store. (default is true)
      Default: true
    --session-timeout
      The number of minutes of inactivity before a user's session is timed
      out.
    --shutdown-override
      Overrides the default behavior and casues Tomcat to ignore lifecycle
      failure events rather than shutting down when they occur.
      Default: false
    --temp-directory
      Define the temp directory, default value: ./target/tomcat.PORT
    --tomcat-users-location
      Location of the tomcat-users.xml file. (relative to the location of the
      webapp-runner jar file)
    --uri-encoding
      Set the URI encoding to be used for the Connector.
    --use-body-encoding-for-uri
      Set if the entity body encoding should be used for the URI.
      Default: false
    --secure-error-report-valve
      Set true to set ErrorReportValve properties showReport and showServerInfo to false. This protects from Apache stacktrace logging of malicious http requests. 
      Default: false
    -A
      Allows setting HTTP connector attributes. For example: -Acompression=on
      Syntax: -Akey=value
      Default: {}
```

See the Tomcat documentation for a [complete list of HTTP connector attributes](https://tomcat.apache.org/tomcat-10.0-doc/config/http.html).
