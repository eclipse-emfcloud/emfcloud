# EMF.cloud Codestyle [![build-status](https://img.shields.io/jenkins/build?jobUrl=https://ci.eclipse.org/emfcloud/job/eclipse-emfcloud/job/emfcloud/job/master/)](https://ci.eclipse.org/emfcloud/job/eclipse-emfcloud/job/emfcloud/job/master/) [![publish-status](https://img.shields.io/jenkins/build?jobUrl=https://ci.eclipse.org/emfcloud/job/deploy-emfcloud-checkstyle-m2/&label=publish)](https://ci.eclipse.org/emfcloud/job/deploy-emfcloud-checkstyle-m2/)

Collection of common reusable resources for code formatting and code checks in EMF.cloud Java
projects.

## Code formatting

The `eclipse` directory contains the EMF.cloud settings for formatting and clean-up of Java code as well as code templates. You can use these settings on a per-project basis by simply copying the `.settings` directory into your Eclipse project.

Alternatively, these settings can also be configured globally. This can be done in the preferences (Window > Preferences > Java > Code Style) by creating a new profile and import the settings from the corresponding .xml file of the `eclipse` directory.

### Usage in alternative IDEs

The code style settings are intended for usage in the Eclipse IDE, however, some alternative IDEs provide support to import Eclipse-specific code formatter settings.

If you want to use them in IntelliJ Idea you can follow the instructions in [this blog post](https://blog.jetbrains.com/idea/2014/01/intellij-idea-13-importing-code-formatter-settings-from-eclipse/).

The VSCode Java Language support also offers the possibility to configure [an xml-based Eclipse formatter](https://code.visualstudio.com/docs/java/java-linting#_formatter).

## Checkstyle

[Checkstyle](https://checkstyle.sourceforge.io/) is used for static code analysis in EMF.cloud Java projects. A [custom checkstyle configuration](codestyle/org.eclipse.emfcloud.checkstyle/src/main/resources/emfcloud-checkstyle.xml) for EMF.cloud is provided and
can be integrated in the build process via maven as well as directly into the Eclipse IDE.
The recommended checkstyle version is [`8.39`](https://checkstyle.sourceforge.io/releasenotes.html#Release_8.39).

### Usage in maven

The directory `org.eclipse.emfcloud.checkstyle` contains the `EMF.cloud checkstyle configuration` bundled as a maven artifact. The artifact can be used in a maven build to integrate checkstyle based on the EMF.cloud configuration.

The `pom.xml` of your project has to be configured like this:

```xml
<project>
    ...
    <build>
        <plugins>
            ...
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-checkstyle-plugin</artifactId>
                <version>3.1.1</version>
                <dependencies>
                    <dependency>
                        <groupId>com.puppycrawl.tools</groupId>
                        <artifactId>checkstyle</artifactId>
                        <version>8.39</version>
                    </dependency>
                    <dependency>
                        <groupId>org.eclipse.emfcloud</groupId>
                        <artifactId>org.eclipse.emfcloud.checkstyle</artifactId>
                        <version>0.1.0-SNAPSHOT</version>
                    </dependency>
                </dependencies>
                <configuration>
                    <configLocation>emfcloud-checkstyle.xml</configLocation>
                </configuration>
            </plugin>
            ...
        </plugins>
    </build>
    ...
</project>
```

</br>
If you are using a SNAPSHOT version of `org.eclipse.emfcloud.checkstyle` you also have to specify the Sonatype snapshot repository:
</br>
</br>

```xml
<project>
    ...
    <repositories>
        <repository>
            <id>sonatype</id>
            <name>Sonatype</name>
            <url>https://oss.sonatype.org/content/groups/public</url>
        </repository>
    </repositories>
    ...
</project>
```

</br>

### Usage in Eclipse IDE

Checkstyle integration into the Eclipse IDE can be achieved with the help of the `Eclipse Checkstyle Plugin`. Go to <https://checkstyle.org/eclipse-cs/#!/> and follow the instructions to install the plugin.

The `eclipse` directory contains a preconfigured `.checkstyle` file to use the EMF.cloud checkstyle rules in your Eclipse project. Simply copy the file into your Eclipse project and then activate checkstyle with: right-click on your project >  Checkstyle > Activate checkstyle.

Alternatively, you can also setup a manual configuration. Go to Window > Preferences > Checkstyle and setup new Global Check Configuration by clicking the 'New...' button.

As type choose 'Remote Configuration' and as location use the raw link to the [custom checkstyle configuration](codestyle/org.eclipse.emfcloud.checkstyle/src/main/resources/emfcloud-checkstyle.xml):

<https://raw.githubusercontent.com/eclipse-emfcloud/emfcloud/master/codestyle/org.eclipse.emfcloud.checkstyle/src/main/resources/emfcloud-checkstyle.xml>
