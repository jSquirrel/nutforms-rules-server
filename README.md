# Nutforms Rules Server

[![Build Status](https://travis-ci.org/jSquirrel/nutforms-rules-server.svg?branch=master)](https://travis-ci.org/jSquirrel/nutforms-rules-server)

Module for integrating server-side portion of business rules with the Nutforms library.

The Nutforms library consists of following modules:

* [Nutforms Server](https://github.com/jSquirrel/nutforms-server)
* [Nutforms Rules Server](https://github.com/jSquirrel/nutforms-rules-server)
* [Nutforms Web Client](https://github.com/jSquirrel/nutforms-web-client)
* [Nutforms Rules Web Client](https://github.com/jSquirrel/nutforms-rules-web-client)

## Installation

The build process of this project is managed by [Apache Maven](https://maven.apache.org/).

In order to build the project, clone this repository and install it with maven:

```
git clone git@github.com:jSquirrel/nutforms-rules-server.git
cd nutforms-rules-server
mvn clean install
```

## Running tests

Tests are executed automatically with Maven builds. To run them separately, execute the command `mvn test` in the root of this project.

## Contributing

Feel free to contribute to the project by reporting [issues](https://github.com/jSquirrel/nutforms-rules-server/issues)
or creating [pull requests](https://github.com/jSquirrel/nutforms-rules-server/pulls).

## License

This software is provided under [MIT License](https://opensource.org/licenses/MIT).

## Documentation

This part of the Nutforms library is responsible for transforming the rule aspect described in your application into POJO, which can be then transferred to various parts of the application. Also, it defines Java Servlet which is used to transfer the rules to the client side.

*Note: in order to use the rule module, Nutforms library must be loaded into the project as well.*

[Example project built upon the Nutforms library](https://github.com/jSquirrel/nutforms-example)

### Architecture

The `entity` and `functions` packages are used to declare entites and global functions to be used in tests. These do not affect the core functionality.

Package `inspection` contains the Inspector class, which is used to transform the Drools rule into a POJO. Also, the  package *interpreter* is located here, which contains experimental classes used to build a expression tree out of the rule condition. The *interpreter* package is currently not used when transforming the rules.

Package `metamodel` contains various classes of the meta-model that is used to store the information from the rules. This object is then transferred to target domain, currently, to the presentation layer.

Lastly, package `servlet` contains the definition of the Java Servlet used to transfer the rules from the server to the client.

### Registering the servlet

In order to retrieve the rules from the server, the servlet needs to be registered in the `web.xml` file.

```xml
<servlet>
    <servlet-name>RuleServlet</servlet-name>
    <servlet-class>cz.cvut.fel.nutforms.rules.servlet.RuleServlet</servlet-class>
</servlet>

<servlet-mapping>
    <servlet-name>RuleServlet</servlet-name>
    <url-pattern>/rules/*</url-pattern>
</servlet-mapping>
```

### Rule aspect definition

When the servlet has been registered, you can begin to create rule declaration in the `resources/rules` folder of your application. Rules are currently described with the [Drools DSL](http://www.drools.org/) and the name of the package in these files is used as an identifier of the business context. The naming convention of these contexts is `FQN.action`, where `FQN` is a fully qualified name of the target entity and `action` is the name of the respective business operation.

[An example of rule declaration](https://github.com/jSquirrel/nutforms-example/tree/master/src/main/resources/rules)

### i18n

Loading of the feedback messages in a correct language according to the current user context is achieved via the Localization aspect. The unique key is composed of various parts according to convention `rule.entity.action.name`, where `entity` is the FQN of target entity, `action` is the name of constrained business operation, and `name` is the name of the rule.

[An example of feedback translation](https://github.com/jSquirrel/nutforms-example/blob/master/src/main/resources/FormBundle.properties)
