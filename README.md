# LicenseScan Maven Plugin

[![](https://jitpack.io/v/carlomorelli/licensescan-maven-plugin.svg)](https://jitpack.io/#carlomorelli/licensescan-maven-plugin)

LicenseScan Maven plugin audits the dependencies and the transitive dependencies for the Runtime and Compile scopes,
and allow to fail the build if a license is detected belonging to the configured blacklist.

The plugin is exclusively composed of the `audit` goal. The goal can be linked at any stage of the Maven lifecycle with the appropriate `<executions/>` configuration.

The following configuration parameters are offered:
* `printLicenses`: prints the scanned licenses during the build (default `false`)
* `blacklistedLicenses`: list of licenses that will make the build fail if detected
* `failBuildOnBlacklisted`: if `blacklistedLicenses` are configured and at least a violation is found, makes the build fail (default `false`)

Parameter list `blacklistedLicenses` is tricky to configure as some Maven artifacts use different names (e.g. Apache 2.0, Apache Apache License, Version 2.0, Apache Version 2.0, etc...) for the same license.
My personal suggestion before compiling this list is to run the plugin with `printLicenses` set to `true`, note down all the license naming variation found, and then compile the blacklist from that.

Plugin configuration example in a project:
```xml
 <plugin>
    <groupId>com.github.carlomorelli</groupId>
    <artifactId>licensescan-maven-plugin</artifactId>
    <version>master-SNAPSHOT</version>
    <configuration>
      <printLicenses>true</printLicenses>
      <blacklistedLicenses>
        <license>GNU General Public License, v2.0</license>
        <license>GNU General Public License, v3.0</license>
        <license>GNU Affero General Public License</license>
      </blacklistedLicenses>
      <failBuildOnBlacklisted>true</failBuildOnBlacklisted>
    </configuration>
    <executions>
      <execution>
        <phase>compile</phase> <!-- use your preferred goal, for me it makes sense to do the check at compile time -->
        <goals>
          <goal>audit</goal>
        </goals>
      </execution>
    </executions>
  </plugin>
```
The plugin is released through [Jitpack](https://jitpack.io), thus the following block needs also to be enabled in your `pom.xml`:
```xml
  <pluginRepositories>
    <pluginRepository>
      <id>jitpack.io</id>
      <url>https://jitpack.io</url>
    </pluginRepository>
  </pluginRepositories>
```

If the `<executions/>` block is configured, the plugin will run during your selected lifecycle. Otherwise, you can launch the execution independently with:
```sh
mvn licensescan:audit
```
Let me know if you find this plugin useful!
--Carlo
