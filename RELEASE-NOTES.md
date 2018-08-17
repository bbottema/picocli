# picocli Release Notes

# <a name="3.6.0"></a> Picocli 3.6.0 (UNRELEASED)
The picocli community is pleased to announce picocli 3.6.0.

This is a bugfix release.

This is the thirty-nineth public release.
Picocli follows [semantic versioning](http://semver.org/).

## <a name="3.6.0"></a> Table of Contents
* [New and noteworthy](#3.6.0-new)
* [Promoted features](#3.6.0-promoted)
* [Fixed issues](#3.6.0-fixes)
* [Deprecations](#3.6.0-deprecated)
* [Potential breaking changes](#3.6.0-breaking-changes)

## <a name="3.6.0-new"></a> New and Noteworthy


## <a name="3.6.0-promoted"></a> Promoted Features
Promoted features are features that were incubating in previous versions of picocli but are now supported and subject to backwards compatibility. 

No features have been promoted in this picocli release.

## <a name="3.6.0-fixes"></a> Fixed issues
- [#433] API: Added method `printHelpIfRequested` that accepts a `ColorScheme` parameter. Thanks to [Benny Bottema](https://github.com/bbottema) for the suggestion.
- [#444] Bugfix: Usage help shows duplicate aliases if registered with same alias multiple times.

## <a name="3.6.0-deprecated"></a> Deprecations
No features were deprecated in this release.

## <a name="3.6.0-breaking-changes"></a> Potential breaking changes
This release has no breaking changes.


# <a name="3.5.2"></a> Picocli 3.5.2
The picocli community is pleased to announce picocli 3.5.2.

This is a bugfix release that fixes an issue where subcommand aliases were not recognized in some cases.

This is the thirty-eighth public release.
Picocli follows [semantic versioning](http://semver.org/).

## <a name="3.5.2"></a> Table of Contents
* [New and noteworthy](#3.5.2-new)
* [Promoted features](#3.5.2-promoted)
* [Fixed issues](#3.5.2-fixes)
* [Deprecations](#3.5.2-deprecated)
* [Potential breaking changes](#3.5.2-breaking-changes)

## <a name="3.5.2-new"></a> New and Noteworthy


## <a name="3.5.2-promoted"></a> Promoted Features
Promoted features are features that were incubating in previous versions of picocli but are now supported and subject to backwards compatibility. 

No features have been promoted in this picocli release.

## <a name="3.5.2-fixes"></a> Fixed issues
- [#443] Bugfix: Subcommand aliases were not recognized in some cases. Thanks to [K. Alex Mills](https://github.com/kalexmills) for the bug report.

## <a name="3.5.2-deprecated"></a> Deprecations
No features were deprecated in this release.

## <a name="3.5.2-breaking-changes"></a> Potential breaking changes
This release has no breaking changes.



# <a name="3.5.1"></a> Picocli 3.5.1
The picocli community is pleased to announce picocli 3.5.1.

This is a bugfix release that fixes an issue where CommandSpec injected into Mixins had a `null` CommandLine.

This is the thirty-seventh public release.
Picocli follows [semantic versioning](http://semver.org/).

## <a name="3.5.1"></a> Table of Contents
* [New and noteworthy](#3.5.1-new)
* [Promoted features](#3.5.1-promoted)
* [Fixed issues](#3.5.1-fixes)
* [Deprecations](#3.5.1-deprecated)
* [Potential breaking changes](#3.5.1-breaking-changes)

## <a name="3.5.1-new"></a> New and Noteworthy



## <a name="3.5.1-promoted"></a> Promoted Features
Promoted features are features that were incubating in previous versions of picocli but are now supported and subject to backwards compatibility. 

No features have been promoted in this picocli release.

## <a name="3.5.1-fixes"></a> Fixed issues
- [#439] Bugfix: CommandSpec injected into Mixins had a `null` CommandLine. Thanks to [Adam Zegelin](https://github.com/zegelin) for the bug report.

## <a name="3.5.1-deprecated"></a> Deprecations
No features were deprecated in this release.

## <a name="3.5.1-breaking-changes"></a> Potential breaking changes
This release has no breaking changes.


# <a name="3.5.0"></a> Picocli 3.5.0
The picocli community is pleased to announce picocli 3.5.0.

This release contains new features, bugfixes and enhancements.

Password support: for options and positional parameters marked as `interactive`, the user is prompted to enter a value on the console.
When running on Java 6 or higher, picocli will use the <a href="https://docs.oracle.com/javase/8/docs/api/java/io/Console.html#readPassword-java.lang.String-java.lang.Object...-"><code>Console.readPassword</code></a> API so that user input is not echoed to the console. 

Client code can now perform simple validation in annotated setter methods by throwing a `ParameterException` on invalid input. 

Also, from this release, the comment character in @-files (argument files) and the end-of-options delimiter (`--` by default) are configurable.


This is the thirty-sixth public release.
Picocli follows [semantic versioning](http://semver.org/).

## <a name="3.5.0"></a> Table of Contents
* [New and noteworthy](#3.5.0-new)
* [Promoted features](#3.5.0-promoted)
* [Fixed issues](#3.5.0-fixes)
* [Deprecations](#3.5.0-deprecated)
* [Potential breaking changes](#3.5.0-breaking-changes)

## <a name="3.5.0-new"></a> New and Noteworthy

### <a name="3.5.0-passwords"></a><a name="3.5.0-interactive"></a> `Interactive` Options for Passwords or Passphrases
This release introduces password support: for options and positional parameters marked as `interactive`, the user is prompted to enter a value on the console.
When running on Java 6 or higher, picocli will use the <a href="https://docs.oracle.com/javase/8/docs/api/java/io/Console.html#readPassword-java.lang.String-java.lang.Object...-"><code>Console.readPassword</code></a> API so that user input is not echoed to the console. 

Example usage:

```java
class Login implements Callable<Object> {
    @Option(names = {"-u", "--user"}, description = "User name")
    String user;

    @Option(names = {"-p", "--password"}, description = "Passphrase", interactive = true)
    String password;

    public Object call() throws Exception {
        MessageDigest md = MessageDigest.getInstance("SHA-256");
        md.update(password.getBytes());
        System.out.printf("Hi %s, your passphrase is hashed to %s.%n", user, base64(md.digest()));
        return null;
    }
    
    private String base64(byte[] arr) { /* ... */ }
}
```

When this command is invoked like this:

```java
CommandLine.call(new Login(), "-u", "user123", "-p");
```

Then the user will be prompted to enter a value:

```
Enter value for --password (Passphrase): 
```

When running on Java 6 or higher, the user input is not echoed to the console.
After the user enters a value and presses enter, the `call()` method is invoked, which prints the following:

```bash
Hi user123, your passphrase is hashed to 75K3eLr+dx6JJFuJ7LwIpEpOFmwGZZkRiB84PURz6U8=.
```

### <a name="3.5.0-validation"></a> Simple Validation in Setter Methods
Methods annotated with `@Option` and `@Parameters` can do simple input validation by throwing a `ParameterException` when invalid values are specified on the command line.

```java
class ValidationExample {
    private Map<String, String> properties = new LinkedHashMap<>();
    
    @Spec private CommandSpec spec; // injected by picocli
    
    @Option(names = {"-D", "--property"}, paramLabel = "KEY=VALUE")
    public void setProperty(Map<String, String> map) {
        for (String key : map.keySet()) {
            String newValue = map.get(key);
            validateUnique(key, newValue);
            properties.put(key, newValue);
        }
    }

    private void validateUnique(String key, String newValue) {
        String existing = properties.get(key);
        if (existing != null && !existing.equals(newValue)) {
            throw new ParameterException(spec.commandLine(),
                    String.format("Duplicate key '%s' for values '%s' and '%s'.",
                    key, existing, newValue));
        }
    }
}
```

Prior to this release, the exception thrown from the method was wrapped in a `java.lang.reflect.InvocationTargetException`, which in turn was wrapped in a `PicocliException`, and finally in another `ParameterException`.

By following the recipe above and throwing a `ParameterException` on invalid input, all these intermediate exceptions are skipped.


## <a name="3.5.0-promoted"></a> Promoted Features
Promoted features are features that were incubating in previous versions of picocli but are now supported and subject to backwards compatibility. 

No features have been promoted in this picocli release.

## <a name="3.5.0-fixes"></a> Fixed issues
- [#430] Bugfix: formatting was incorrect (did not break on embedded newlines) in the subcommands list descriptions. Thanks to [Benny Bottema](https://github.com/bbottema) for the bug report.
- [#431] Better support for validation in setter methods: cleaner stack trace.
- [#432] Make comment character in @-files (argument files) configurable.
- [#359] Make end-of-options delimiter configurable.
- [#82] Support reading passwords from the console with echoing disabled.

## <a name="3.5.0-deprecated"></a> Deprecations
No features were deprecated in this release.

## <a name="3.5.0-breaking-changes"></a> Potential breaking changes
This release has no breaking changes.

# <a name="3.4.0"></a> Picocli 3.4.0
The picocli community is pleased to announce picocli 3.4.0.

This release contains new features, bugfixes and enhancements.
The parser can now ignore case when parsing arguments for an Enum option or positional parameter.
New methods `Help.Ansi.text(String)` and `Help.Ansi.string(String)` assist client code in easily creating ANSI messages outside usage help and version help.

This is the thirty-fifth public release.
Picocli follows [semantic versioning](http://semver.org/).

## <a name="3.4.0"></a> Table of Contents
* [New and noteworthy](#3.4.0-new)
* [Promoted features](#3.4.0-promoted)
* [Fixed issues](#3.4.0-fixes)
* [Deprecations](#3.4.0-deprecated)
* [Potential breaking changes](#3.4.0-breaking-changes)

## <a name="3.4.0-new"></a> New and Noteworthy

## <a name="3.4.0-promoted"></a> Promoted Features
Promoted features are features that were incubating in previous versions of picocli but are now supported and subject to backwards compatibility. 

No features have been promoted in this picocli release.

## <a name="3.4.0-fixes"></a> Fixed issues
- [#14] New API: Support enum values to be parsed in an case-insensitive way.
- [#376] New API: `Help.Ansi.text(String)` and `Help.Ansi.string(String)` help client code easily create ANSI messages outside usage help and version help.
- [#412] Enhancement: Enum constant names are now returned from `ArgSpec::completionCandidates()`. Thanks to [Radovan Panák](https://github.com/rpanak).
- [#417] Enhancement: Ensure bash scripts have correct line separators. Thanks to [Holger Stenger](https://github.com/stengerh).
- [#425] Enhancement: Fix autocomplete script errors in zsh. Thanks to [Anthony Keenan](https://github.com/anthonykeenan).
- [#419] Bugfix: Default value for arrays was not rendered correctly with `@{DEFAULT-VALUE}`.
- [#418] Doc: Improve installation instructions for autocompletion scripts.
- [#420] Doc: Added a Quick Guide
  

## <a name="3.4.0-deprecated"></a> Deprecations
No features were deprecated in this release.

## <a name="3.4.0-breaking-changes"></a> Potential breaking changes
This release has no breaking changes.


# <a name="3.3.0"></a> Picocli 3.3.0
The picocli community is pleased to announce picocli 3.3.0.

This release contains a bugfix for the JLine TAB completion support and improves the error messages for missing required parameters and unmatched arguments.

This is the thirty-fourth public release.
Picocli follows [semantic versioning](http://semver.org/).

## <a name="3.3.0"></a> Table of Contents
* [New and noteworthy](#3.3.0-new)
* [Promoted features](#3.3.0-promoted)
* [Fixed issues](#3.3.0-fixes)
* [Deprecations](#3.3.0-deprecated)
* [Potential breaking changes](#3.3.0-breaking-changes)

## <a name="3.3.0-new"></a> New and Noteworthy
### `UnmatchedArgumentException` Improvements
The `UnmatchedArgumentException` class now has several methods that allow an application to offer suggestions for fixes to the end user.

For example:
```java
class App {
    @Option(names = "--file") File[] files;
    @Option(names = "--find") String pattern;
    
    public static void main(String[] args) {
        App app = new App();
        try {
            new CommandLine(app).parse(args);
            // ...
            
        } catch (ParameterException ex) {
            System.err.println(ex.getMessage());
            if (!UnmatchedArgumentException.printSuggestions(ex, System.err)) { // new API
                ex.getCommandLine().usage(System.err, ansi);
            }
        }
    } 
}
```

If you run this class with an invalid option that is similar to an actual option, the `UnmatchedArgumentException.printSuggestions` method will show the actual options. For example:

```
<cmd> -fi
```

Prints this output:

```
Unknown option: -fi
Possible solutions: --file, --find
```

This is the behaviour for the `CommandLine` convenience methods `run`, `call` and `parseWithHandlers`. 
Note that if possible fixes are found, the usage help message is not displayed.

## <a name="3.3.0-promoted"></a> Promoted Features
Promoted features are features that were incubating in previous versions of picocli but are now supported and subject to backwards compatibility. 

No features have been promoted in this picocli release.

## <a name="3.3.0-fixes"></a> Fixed issues
- [#411] Bugfix: Completion candidates were only generated for the first option, not for subsequent options.
- [#409] Enhancement: Improve error message for missing required positional parameters. Thanks to [Mārtiņš Kalvāns](https://github.com/sisidra) and [Olle Lundberg](https://github.com/lndbrg).
- [#298] Enhancement: Add help for mistyped commands and options. Added new API to `UnmatchedArgumentException`. Thanks to [Philippe Charles](https://github.com/charphi).

## <a name="3.3.0-deprecated"></a> Deprecations
No features were deprecated in this release.

## <a name="3.3.0-breaking-changes"></a> Potential breaking changes
The error message format has changed. This may impact client tests that expect a specific error message.



# <a name="3.2.0"></a> Picocli 3.2.0
The picocli community is pleased to announce picocli 3.2.0.

This release contains new features and enhancements:

* Improved support for Dependency Injection
* Methods can now be annotated with `@Option` and `@Parameters`
* Support for JLine-based interactive command line interfaces (`completionCandidates` attribute on `@Option` and `@Parameters`, and the `AutoComplete.complete` method)
* New `@Spec` annotation for injecting a command with its `CommandSpec`

This is the thirty-third public release.
Picocli follows [semantic versioning](http://semver.org/).

## <a name="3.2.0"></a> Table of Contents
* [New and noteworthy](#3.2.0-new)
* [Promoted features](#3.2.0-promoted)
* [Fixed issues](#3.2.0-fixes)
* [Deprecations](#3.2.0-deprecated)
* [Potential breaking changes](#3.2.0-breaking-changes)

## <a name="3.2.0-new"></a> New and Noteworthy
### <a name="3.2.0-di"></a> Dependency Injection
This release makes integration with Dependency Injection containers extremely easy:

* `CommandLine` constructor now accepts a `Class` instance as the user object, and will delegate to the `IFactory` to get an instance.
* New `CommandLine.run(Class<Runnable>, IFactory, ...)` and `CommandLine.call(Class<Callable>, IFactory, ...)` methods. These work the same as the existing `run` and `call` methods except that the `Runnable` or `Callable` instance is created by the factory.

The below example shows how to create an `IFactory` implementation with a Guice `Injector`:

```java
import com.google.inject.*;
import picocli.CommandLine.IFactory;

public class GuiceFactory implements IFactory {
    private final Injector injector = Guice.createInjector(new DemoModule());

    @Override
    public <K> K create(Class<K> aClass) throws Exception {
        return injector.getInstance(aClass);
    }

    static class DemoModule extends AbstractModule {
        @Override
        protected void configure() {
            bind(java.util.List.class).to(java.util.LinkedList.class);
            bind(Runnable.class).to(InjectionDemo.class);
        }
    }
}
```


Use the custom factory when creating a `CommandLine` instance, or when invoking the `run` or `call` convenience methods:

```java
import javax.inject.Inject;

@Command(name = "di-demo")
public class InjectionDemo implements Runnable {
    @Inject java.util.List list;

    @Option(names = "-x") int x;

    public static void main(String[] args) {
        CommandLine.run(Runnable.class, new GuiceFactory(), args);
    }

    @Override
    public void run() {
        assert list instanceof java.util.LinkedList;
    }
}
```


### <a name="3.2.0-method-annotations"></a> Annotated Methods
From this release, `@Option` and `@Parameter` annotations can be added to methods as well as fields of a class.

For concrete classes, annotate "setter" methods (methods that accept a parameter) and when the option is specified on the command line, picocli will invoke the method with the value specified on the command line, converted to the type of the method parameter.

Alternatively, you may annotate "getter-like" methods (methods that return a value) on an interface, and picocli will create an instance of the interface that returns the values specified on the command line, converted to the method return type. This feature is inspired by [Jewel CLI](https://github.com/lexicalscope/jewelcli).

#### Annotating Methods of an Interface
The `@Option` and `@Parameters` annotations can be used on methods of an interface that return a value. For example:

```java
interface Counter {
    @Option(names = "--count")
    int getCount();
}
```
You use it by specifying the class of the interface:
```java
CommandLine cmd = new CommandLine(Counter.class); // specify a class
String[] args = new String[] {"--count", "3"};
cmd.parse(args);
Counter counter = cmd.getCommand(); // picocli created an instance
assert counter.getCount() == 3; // method returns command line value
```

#### Annotating Methods of a Concrete Class
The `@Option` and `@Parameters` annotations can be used on methods of a class that accept a parameter. For example:

```java
class Counter {
    int count;
    
    @Option(names = "--count")
    void setCount(int count) {
        this.count = count;
    }
}
```
You use it by passing an instance of the class:
```java
Counter counter = new Counter(); // the instance to populate
CommandLine cmd = new CommandLine(counter);
String[] args = new String[] {"--count", "3"};
cmd.parse(args);
assert counter.count == 3; // method was invoked with command line value
```

### <a name="3.2.0-jline"></a> JLine Tab-Completion Support

This release adds support for JLine Tab-Completion.

[Jline 2.x](https://github.com/jline/jline2) and [3.x](https://github.com/jline/jline3) is a Java library for handling console input, often used to create interactive shell applications.

Command line applications based on picocli can generate completion candidates for the command line in the JLine shell. The generated completion candidates are context sensitive, so once a subcommand is specified, only the options for that subcommand are shown, and once an option is specified, only parameters for that option are shown.

Below is an example picocli `Completer` implementation for JLine 2.x:

```java
import jline.console.completer.ArgumentCompleter;
import jline.console.completer.Completer;
import picocli.AutoComplete;
import picocli.CommandLine;
import picocli.CommandLine.Model.CommandSpec;

import java.util.List;

public class PicocliJLineCompleter implements Completer {
    private final CommandSpec spec;

    public PicocliJLineCompleter(CommandSpec spec) {
        this.spec = spec;
    }

    @Override
    public int complete(String buffer, int cursor, List<CharSequence> candidates) {
        // use the jline internal parser to split the line into tokens
        ArgumentCompleter.ArgumentList list =
                new ArgumentCompleter.WhitespaceArgumentDelimiter().delimit(buffer, cursor);

        // let picocli generate completion candidates for the token where the cursor is at
        return AutoComplete.complete(spec,
                list.getArguments(),
                list.getCursorArgumentIndex(),
                list.getArgumentPosition(),
                cursor,
                candidates);
    }
}
```

### <a name="3.2.0-completion-candidates"></a> Completion Candidates
From this release, `@Options` and `@Parameters` have a new `completionCandidates` attribute that can be used to generate a list of completions for this option or positional parameter. For example:

```java
static class MyAbcCandidates extends ArrayList<String> {
    MyAbcCandidates() { super(Arrays.asList("A", "B", "C")); }
}

class ValidValuesDemo {
    @Option(names = "-o", completionCandidates = MyAbcCandidates.class)
    String option;
}
```

This will generate completion option values `A`, `B` and `C` in the generated bash auto-completion script and in JLine.


### <a name="3.2.0-default-variable"></a> `${DEFAULT-VALUE}` Variable
From picocli 3.2, it is possible to embed the default values in the description for an option or positional parameter by
specifying the variable `${DEFAULT-VALUE}` in the description text.
Picocli uses reflection to get the default values from the annotated fields.

The variable is replaced with the default value regardless of the `@Command(showDefaultValues)` attribute
and regardless of the `@Option(showDefaultValues)` or `@Parameters(showDefaultValues)` attribute.

```java
class DefaultValues {
    @Option(names = {"-f", "--file"},
            description = "the file to use (default: ${DEFAULT-VALUE})")
    File file = new File("config.xml");
}

CommandLine.usage(new DefaultValues(), System.out);
```
This produces the following usage help:
```text
Usage: <main class> -f=<file>
  -f, --file=<file>   the file to use (default: config.xml)
```

### <a name="3.2.0-completion-variable"></a>  `${COMPLETION-CANDIDATES}` Variable
Similarly, it is possible to embed the completion candidates in the description for an option or positional parameter by
specifying the variable `${COMPLETION-CANDIDATES}` in the description text.

This works for java `enum` classes and for options or positional parameters of non-enum types for which completion candidates are specified.

```java
enum Lang { java, groovy, kotlin, javascript, frege, clojure }

static class MyAbcCandidates extends ArrayList<String> {
    MyAbcCandidates() { super(Arrays.asList("A", "B", "C")); }
}

class ValidValuesDemo {
    @Option(names = "-l", description = "Enum. Values: ${COMPLETION-CANDIDATES}")
    Lang lang = null;

    @Option(names = "-o", completionCandidates = MyAbcCandidates.class,
            description = "Candidates: ${COMPLETION-CANDIDATES}")
    String option;
}

CommandLine.usage(new ValidValuesDemo(), System.out);
```
This produces the following usage help:
```text
Usage: <main class> -l=<lang> -o=<option>
  -l=<lang>     Enum. Values: java, groovy, kotlin, javascript, frege, clojure
  -o=<option>   Candidates: A, B, C
```

### <a name="3.2.0-Spec"></a> `@Spec` Annotation  
A new `@Spec` annotation is now available that injects the `CommandSpec` model of the command into a command field.

This is useful when a command needs to use the picocli API, for example to walk the command hierarchy and iterate over its sibling commands.
This complements the `@ParentCommand` annotation;  the `@ParentCommand` annotation injects a user-defined command object, whereas this annotation injects a picocli class.
  
```java
class InjectSpecExample implements Runnable {
   @Spec CommandSpec commandSpec;
   //...
   public void run() {
       // do something with the injected spec
   }
}
  
```  
  
  
  

### <a name="3.2.0-lenient-parse"></a> Lenient Parse Mode

This release adds the ability to continue parsing invalid input to the end.
When `collectErrors` is set to `true`, and a problem occurs during parsing, an `Exception` is added to the `ParseResult.errors()` list and parsing continues. The default behaviour (when `collectErrors` is `false`) is to abort parsing by throwing the `Exception`.

This is useful when generating completion candidates on partial input, and is also useful when using picocli in
languages like Clojure where idiomatic error handling does not involve throwing and catching exceptions.

When using this feature, applications are responsible for actively verifying that no errors occurred before executing the business logic. Use with care!


## <a name="3.2.0-promoted"></a> Promoted Features
Promoted features are features that were incubating in previous versions of picocli but are now supported and subject to backwards compatibility. 

No features have been promoted in this picocli release.

## <a name="3.2.0-fixes"></a> Fixed issues
- [#182] New Feature: Add support for annotating methods with `@Option` and `@Parameters`.
- [#393] New feature: Add support for JLine completers.
- [#389] New feature: Support 'lenient' parsing mode: don't throw `Exceptions` but add them to the `ParseResult.errors()` list and continue parsing.
- [#392] New feature: Ability to map command line arguments to picocli spec elements. Internally used for generating completion candidates.
- [#391] New feature: Add API to get completion candidates for option and positional parameter values of any type.
- [#395] New feature: Allow embedding default values anywhere in description for `@Option` or `@Parameters`.
- [#259] New Feature: Added `@Spec` annotation to inject `CommandSpec` into application field.
- [#400] Enhancement: Add run/call static methods that accept an `IFactory`. This allows Dependency Injection containers to provide the Runnable/Callable implementation.
- [#404] Enhancement: Ask IFactory for implementation before creating Proxy for interface. Needed for Dependency Injection.
- [#398] Enhancement: Allow `@PicocliScript` annotation on Groovy script `@Field` variables instead of just on imports.
- [#322] Enhancement: Add `defaultValue` attribute to @Option and @Parameters annotation.
- [#375] Enhancement: Improve `ParameterIndexGapException` error message. Thanks to [gpettey](https://github.com/gpettey).
- [#405] Enhancement: Add method `CommandLine.getUsageMessage()`.
- [#406] Enhancement: Added fields to `ParameterException`. Thanks to [David Hait](https://github.com/dhait).
- [#401] Doc: The user manual no longer includes the full `CommandLine.java` source code.

## <a name="3.2.0-deprecated"></a> Deprecations
No features were deprecated in this release.

## <a name="3.2.0-breaking-changes"></a> Potential breaking changes
This release has no breaking changes.


# <a name="3.1.0"></a> Picocli 3.1.0
The picocli community is pleased to announce picocli 3.1.0.

This release contains bugfixes and support for command aliases.

Picocli has a new logo! Many thanks to [Reallinfo](https://github.com/reallinfo) for the design!

<img src="http://picocli.info/images/logo/horizontal.png" height="100"> 

This is the thirty-second public release.
Picocli follows [semantic versioning](http://semver.org/).

## <a name="3.1.0"></a> Table of Contents
* [New and noteworthy](#3.1.0-new)
* [Promoted features](#3.1.0-promoted)
* [Fixed issues](#3.1.0-fixes)
* [Deprecations](#3.1.0-deprecated)
* [Potential breaking changes](#3.1.0-breaking-changes)

## <a name="3.1.0-new"></a> New and Noteworthy
### Command Aliases
This release adds support for command aliases.

```java
@Command(name = "top", subcommands = {SubCommand.class},
        description = "top level command")
static class TopLevelCommand { }

@Command(name = "sub", aliases = {"s", "sb"},
        description = "I'm a subcommand")
static class SubCommand {}

new CommandLine(new TopLevelCommand()).usage(System.out);
```
The above would print the following usage help message:

```text
Usage: top [COMMAND]
top level command
Commands:
  sub, s, sb   I'm a subcommand
```

## <a name="3.1.0-promoted"></a> Promoted Features
Promoted features are features that were incubating in previous versions of picocli but are now supported and subject to backwards compatibility. 

No features have been promoted in this picocli release.

## <a name="3.1.0-fixes"></a> Fixed issues
- [#288] New feature: add support for command aliases. 
- [#383] Enhancement: [Reallinfo](https://github.com/reallinfo) designed the new picocli logo. Amazing work, many thanks!
- [#388] Bugfix: Prevent AnnotationFormatError "Duplicate annotation for class" with @PicocliScript when the script contains classes. Thanks to [Bradford Powell](https://github.com/bpow) for the bug report.

## <a name="3.1.0-deprecated"></a> Deprecations
No features were deprecated in this release.

## <a name="3.1.0-breaking-changes"></a> Potential breaking changes
This release has no breaking changes.


# <a name="3.0.2"></a> Picocli 3.0.2
The picocli community is pleased to announce picocli 3.0.2.

This release contains bugfixes and enhancements for programmatic configuration.

This is the thirty-first public release.
Picocli follows [semantic versioning](http://semver.org/).

## <a name="3.0.2"></a> Table of Contents
* [New and noteworthy](#3.0.2-new)
* [Promoted features](#3.0.2-promoted)
* [Fixed issues](#3.0.2-fixes)
* [Deprecations](#3.0.2-deprecated)
* [Potential breaking changes](#3.0.2-breaking-changes)

## <a name="3.0.2-new"></a> New and Noteworthy

## <a name="3.0.2-promoted"></a> Promoted Features
Promoted features are features that were incubating in previous versions of picocli but are now supported and subject to backwards compatibility. 

No features have been promoted in this picocli release.

## <a name="3.0.2-fixes"></a> Fixed issues
- [#381] Bugfix: Prevent NPE when adding programmatically created subcommands to CommandLine. Thanks to [Mikusch](https://github.com/Mikusch) for the bug report.
- [#382] Enhancement: Subcommand name should be initialized when added to parent.

## <a name="3.0.2-deprecated"></a> Deprecations
No features were deprecated in this release.

## <a name="3.0.2-breaking-changes"></a> Potential breaking changes
This release has no breaking changes.


# <a name="3.0.1"></a> Picocli 3.0.1
The picocli community is pleased to announce picocli 3.0.1.

This release fixes a bug for map options and has several improvements for the usage help message, especially for subcommands.

This is the thirtieth public release.
Picocli follows [semantic versioning](http://semver.org/).

## <a name="3.0.1"></a> Table of Contents
* [New and noteworthy](#3.0.1-new)
* [Promoted features](#3.0.1-promoted)
* [Fixed issues](#3.0.1-fixes)
* [Deprecations](#3.0.1-deprecated)
* [Potential breaking changes](#3.0.1-breaking-changes)

## <a name="3.0.1-new"></a> New and Noteworthy
From this release, the usage help synopsis of the subcommand shows not only the subcommand name but also the parent command name. For example, take the following hierarchy of subcommands.

```java
@Command(name = "main", subcommands = {Sub.class}) class App { }
@Command(name = "sub", subcommands = {SubSub.class}) class Sub { }
@Command(name = "subsub", mixinStandardHelpOptions = true) class SubSub { }

CommandLine parser = new CommandLine(new App());
ParseResult result = parser.parseArgs("sub", "subsub", "--help");
CommandLine.printHelpIfRequested(result);
```
The above code prints the usage help for the `subsub` nested subcommand. Notice that this shows not only the subcommand name but the full command hierarchy:

```
Usage: main sub subsub [-hV]
  -h, --help      Show this help message and exit.
  -V, --version   Print version information and exit.
```

## <a name="3.0.1-promoted"></a> Promoted Features
Promoted features are features that were incubating in previous versions of picocli but are now supported and subject to backwards compatibility. 

No features have been promoted in this picocli release.

## <a name="3.0.1-fixes"></a> Fixed issues
- [#287] Enhancement: Subcommand help now also shows parent command name in synopsis.
- [#378] Bugfix: Map option problem when value contains '=' separator. Thanks to [Markus Kramer](https://github.com/MarkusKramer) for the bug report.
- [#377] Bugfix: Standard help options should be added last: when `mixinStandardHelpOptions` is set and `sortOptions` is false, the help options should appear after the command options in the usage help message.

## <a name="3.0.1-deprecated"></a> Deprecations
No features were deprecated in this release.

## <a name="3.0.1-breaking-changes"></a> Potential breaking changes
The usage help synopsis of the subcommand shows not only the subcommand name but also the parent command name (and parent's parent command name, up to the top-level command).



# <a name="3.0.0"></a> Picocli 3.0.0
The picocli community is pleased to announce picocli 3.0.0.

This release offers a [programmatic API](http://picocli.info/picocli-3.0-programmatic-api.html) for creating command line applications, in addition to the annotations API. The programmatic API allows applications to dynamically create command line options on the fly, and also makes it possible to create idiomatic domain-specific languages for processing command line arguments, using picocli, in other JVM languages. The picocli community is proud to announce that [Apache Groovy](http://groovy-lang.org/)'s [CliBuilder](http://docs.groovy-lang.org/docs/next/html/gapi/groovy/cli/picocli/CliBuilder.html) DSL for command line applications has been rewritten to use the picocli programmatic API, starting from Groovy 2.5.

Another new feature in this release are Mixins. Mixins allow reusing common options, parameters and command attributes in multiple applications without copy-and-paste duplication.

This release aims to reduce boilerplate code in user applications even further with the new `mixinStandardHelpOptions` command attribute. Picocli adds standard `usageHelp` and `versionHelp` options to commands with this attribute. Additionally picocli now offers a `HelpCommand` that can be installed as a subcommand on any application command to provide usage help for the parent command or sibling subcommands.

From this release, picocli is better at following unix conventions: by default it now prints to stdout when the user requested help, and prints to stderr when the input was invalid or an unexpected error occurred. This release also gives better control over the process exit code.

A new `@Unmatched` annotation allows applications to easily capture unmatched arguments (arguments that could not be matched with any of the registered options or positional parameters).

Usage help message improvements: the usage help message width is now configurable, and the message layout is improved to reduce horizontal padding. Furthermore, you can now specify for individual options or positional parameters whether their default value should be shown in the description or hidden.

Finally, this release adds several options to configure parser behaviour. Picocli can now be configured to function like Apache Commons CLI, to facilitate migration from Apache Commons CLI to picocli.


This is the twenty-ninth public release.
Picocli follows [semantic versioning](http://semver.org/).

## <a name="3.0.0"></a> Table of Contents
* [New and noteworthy](#3.0.0-new)
* [Promoted features](#3.0.0-promoted)
* [Fixed issues](#3.0.0-fixes)
* [Deprecations](#3.0.0-deprecated)
* [Potential breaking changes](#3.0.0-breaking-changes)

## <a name="3.0.0-new"></a> New and Noteworthy

### <a name="3.0.0-Programmatic-API"></a> Programmatic API
This release offers a programmatic API for creating command line applications, in addition to the annotations API. The programmatic API allows applications to dynamically create command line options on the fly, and also makes it possible to create idiomatic domain-specific languages for processing command line arguments, using picocli, in other JVM languages. (Example: Groovy [CliBuilder](http://docs.groovy-lang.org/docs/next/html/gapi/groovy/cli/picocli/CliBuilder.html).)

_If you have suggestions for improving the programmatic API, please raise a ticket on GitHub!_

#### Example
```java
CommandSpec spec = CommandSpec.create();
spec.mixinStandardHelpOptions(true); // --help and --version options
spec.addOption(OptionSpec.builder("-c", "--count")
        .paramLabel("COUNT")
        .type(int.class)
        .description("number of times to execute").build());
spec.addPositional(PositionalParamSpec.builder()
        .paramLabel("FILES")
        .type(List.class).auxiliaryTypes(File.class) // List<File>
        .description("The files to process").build());

CommandLine commandLine = new CommandLine(spec);
try {
    // see also the CommandLine.parseWithHandler(s) convenience methods
    ParseResult pr = commandLine.parseArgs(args);
    
    if (CommandLine.printHelpIfRequested(pr)) {
        return;
    }
    int count = pr.matchedOptionValue('c', 1);
    List<File> files = pr.matchedPositionalValue(0, Collections.<File>emptyList());
    for (File f : files) {
        for (int i = 0; i < count; i++) {
            System.out.printf("%d: %s%n", i, f);
        }
    }
} catch (ParseException invalidInput) {
    System.err.println(invalidInput.getMessage());
    invalidInput.getCommandLine().usage(System.err);
}
```

#### CommandSpec
`CommandSpec` models a command. It is the programmatic variant of the `@Command` annotation. It has a name and a version, both of which may be empty.  It also has a `UsageMessageSpec` to configure aspects of the usage help message and a `ParserSpec` that can be used to control the behaviour of the parser.

#### OptionSpec and PositionalParamSpec
`OptionSpec` models a named option, and `PositionalParamSpec` models one or more positional parameters. They are the programmatic variant of the `@Option` and `@Parameters` annotations, respectively.

An `OptionSpec` must have at least one name, which is used during parsing to match command line arguments. Other attributes can be left empty and picocli will give them a reasonable default value. This defaulting is why `OptionSpec` objects are created with a builder: this allows you to specify only some attributes and let picocli initialise the other attributes. For example, if only the option’s name is specified, picocli assumes the option takes no parameters (arity = 0), and is of type `boolean`. Another example, if arity is larger than `1`, picocli sets the type to `List` and the `auxiliary type` to `String`.

`PositionalParamSpec` objects don’t have names, but have an index range instead. A single `PositionalParamSpec` object can capture multiple positional parameters. The default index range is set to `0..*` (all indices). A command may have multiple `PositionalParamSpec` objects to capture positional parameters at different index ranges. This can be useful if positional parameters at different index ranges have different data types.

Similar to `OptionSpec` objects, Once a `PositionalParamSpec` is constructed, its configuration becomes immutable, but its `value` can still be modified. Usually the value is set during command line parsing when a non-option command line argument is encountered at a position in its index range.

#### <a name="3.0.0-ParseResult"></a> ParseResult
A `ParseResult` class is now available that allows applications to inspect the result of parsing a sequence of command line arguments.

This class provides methods to query whether the command line arguments included certain options or position parameters, and what the value or values of these options and positional parameters was. Both the original command line argument String value as well as a strongly typed value can be obtained.


### Mixins for Reuse
Mixins are a convenient alternative to subclassing: picocli annotations from _any_ class can be added to ("mixed in" with) another command. This includes options, positional parameters, subcommands and command attributes. Picocli [autoHelp](#3.0.0-alpha-1-autohelp) internally uses a mixin.

A mixin is a separate class with options, positional parameters, subcommands and command attributes that can be reused in other commands. Mixins can be installed by calling the `CommandLine.addMixin` method with an object of this class, or annotating a field in your command with `@Mixin`. Here is an example mixin class:

```java
public class ReusableOptions {

    @Option(names = { "-v", "--verbose" }, description = {
        "Specify multiple -v options to increase verbosity.", "For example, `-v -v -v` or `-vvv`" })
    protected boolean[] verbosity = new boolean[0];
}
```

#### Adding Mixins Programmatically
The below example shows how a mixin can be added programmatically with the `CommandLine.addMixin` method.

```java
CommandLine commandLine = new CommandLine(new MyCommand());
commandline.addMixin("myMixin", new ReusableOptions());
```
#### `@Mixin` Annotation
A command can also include mixins by annotating fields with `@Mixin`. All picocli annotations found in the mixin class are added to the command that has a field annotated with `@Mixin`. For example:

```java
@Command(name = "zip", description = "Example reuse with @Mixin annotation.")
public class MyCommand {

    // adds the options defined in ReusableOptions to this command
    @Mixin
    private ReusableOptions myMixin;
}
```


### <a name="3.0.0-mixinStandardHelpOptions"></a> Standard Help Options
This release introduces the `mixinStandardHelpOptions` command attribute. When this attribute is set to `true`, picocli adds a mixin to the command that adds `usageHelp` and `versionHelp` options to the command. For example:

```java
@Command(mixinStandardHelpOptions = true, version = "auto help demo - picocli 3.0")
class AutoHelpDemo implements Runnable {

    @Option(names = "--option", description = "Some option.")
    String option;

    @Override public void run() { }
}
```

Commands with `mixinStandardHelpOptions` do not need to explicitly declare fields annotated with `@Option(usageHelp = true)` and `@Option(versionHelp = true)` any more. The usage help message for the above example looks like this:
```text
Usage: <main class> [-hV] [--option=<option>]
      --option=<option>   Some option.
  -h, --help              Show this help message and exit.
  -V, --version           Print version information and exit.
```

### <a name="3.0.0-HelpCommand"></a> Help Command

From this release, picocli provides a `help` subcommand (`picocli.CommandLine.HelpCommand`) that can be installed as a subcommand on any application command to provide usage help for the parent command or sibling subcommands. For example:

```java
@Command(subcommands = HelpCommand.class)
class AutoHelpDemo implements Runnable {

    @Option(names = "--option", description = "Some option.")
    String option;

    @Override public void run() { }
}
```


```text
# print help for the `maincommand` command
maincommand help

# print help for the `subcommand` command
maincommand help subcommand
```

For applications that want to create a custom help command, this release also introduces a new interface `picocli.CommandLine.IHelpCommandInitializable` that provides custom help commands with the information they need: access to the parent command and sibling commands, whether to use Ansi colors or not, and the streams to print the usage help message to.

### <a name="3.0.0-Unmatched"></a> `@Unmatched` Annotation
Unmatched arguments are the command line arguments that could not be assigned to any of the defined options or positional parameters. From this release, fields annotated with `@Unmatched` will be populated with the unmatched arguments. The field must be of type `String[]` or `List<String>`.

If picocli finds a field annotated with `@Unmatched`, it automatically sets `unmatchedArgumentsAllowed` to `true` so no `UnmatchedArgumentException` is thrown when a command line argument cannot be assigned to an option or positional parameter.

### <a name="3.0.0-std"></a> Stdout or Stderr
From picocli v3.0, the `run` and `call` convenience methods follow unix conventions: print to stdout when the user requested help, and print to stderr when the input was invalid or an unexpected error occurred.

Custom handlers can extend `AbstractHandler` to facilitate following this convention. `AbstractHandler` also provides `useOut` and `useErr` methods to allow customizing the target output streams, and `useAnsi` to customize the Ansi style to use:

```java
@Command class CustomizeTargetStreamsDemo implements Runnable {
    public void run() { ... }

    public static void main(String... args) {
        CommandLine cmd = new CommandLine(new CustomizeTargetStreamsDemo());

        PrintStream myOut = getOutputPrintStream(); // custom stream to send command output to
        PrintStream myErr = getErrorPrintStream();  // custom stream for error messages

        cmd.parseWithHandlers(
                new RunLast().useOut(myOut).useAnsi(Help.Ansi.ON),
                new DefaultExceptionHandler().useErr(myErr).useAnsi(Help.Ansi.OFF),
                args);
    }
}
```

### <a name="3.0.0-exit-code"></a> Exit Code Support
From picocli v3.0, the built-in parse result handlers (`RunFirst`, `RunLast` and `RunAll`) and exception handler (`DefaultExceptionHandler`) can specify an exit code. If an exit code was specified, the handler terminates the JVM with the specified status code when finished.

```java
@Command class ExitCodeDemo implements Runnable {
    public void run() { throw new ParameterException(new CommandLine(this), "exit code demo"); }

    public static void main(String... args) {
        CommandLine cmd = new CommandLine(new ExitCodeDemo());
        cmd.parseWithHandlers(
                new RunLast().andExit(123),
                new DefaultExceptionHandler().andExit(456),
                args);
    }
}
```
Running this command prints the following to stderr and exits the JVM with status code `456`.

```
exit code demo
Usage: <main class>
```

Custom handlers can extend `AbstractHandler` to inherit this behaviour.


### <a name="3.0.0-ShowDefault"></a> Fine-grained ShowDefault

This release adds a `showDefaultValue` attribute to the `@Option` and `@Parameters` annotation. This allows you to specify for each individual option and positional parameter whether its default value should be shown in the usage help.

This attribute accepts three values:

* `ALWAYS` - always display the default value of this option or positional parameter, even `null` values, regardless what value of `showDefaultValues` was specified on the command
* `NEVER` - don't show the default value for this option or positional parameter, regardless what value of `showDefaultValues` was specified on the command
* `ON_DEMAND` - (this is the default) only show the default value for this option or positional parameter if `showDefaultValues` was specified on the command

The `NEVER` value is useful for security sensitive command line arguments like passwords. The `ALWAYS` value is useful when you only want to show the default value for a few arguments but not for all (in combination with `@Command(showDefaultValues = false)`).

### <a name="3.0.0-UsageHelpLayout"></a> Improved Usage Help Message Layout
Previously, the usage message layout had a fixed width long option name column: this column is always 24 characters, even if none of the options have a long option name.

This gave strange-looking usage help messages in some cases. For example:
```java
@Command(name="ls")
class App {
    @Option(names = "-a", description = "display all files") boolean a;
    @Option(names = "-l", description = "use a long listing format") boolean l;
    @Option(names = "-t", description = "sort by modification time") boolean t;
}
```

The usage message for this example was:
```
Usage: ls [-alt]
  -a                          display all files
  -l                          use a long listing format
  -t                          sort by modification time
```
From this release, picocli adjusts the width of the long option name column to the longest name (up to max 24).

The new usage message for this example looks like this:
```
Usage: ls [-alt]
  -a     display all files
  -l     use a long listing format
  -t     sort by modification time
```

### <a name="3.0.0-StricterArity"></a> Stricter Arity Validation

Until this release, options with mandatory parameters would consume as many arguments as required, even if those arguments matched other option flags. For example:

Given a command like this:
```java
class App {
  @Option(names = "-a", arity = "2")
  String[] a;
  
  @Option(names = "-v")
  boolean v;
}
```
Prior to this change, the following input would be accepted:
```
<command> -a 1 -v
```
In previous versions, picocli accepted this and assigned `"1"` and `"-v"` as the two values for the `-a` option.
From this release, the parser notices that one of the arguments is an option and throws a `MissingParameterException` because not enough parameters were specified for the first option.


## <a name="3.0.0-promoted"></a> Promoted Features
Promoted features are features that were incubating in previous versions of picocli but are now supported and subject to backwards compatibility. 

No features have been promoted in this picocli release.

## <a name="3.0.0-fixes"></a> Fixed issues
- [#371] Fixed bug where autocompletion did not work correctly for subcommands with embedded hyphens. Thanks to [Paulius Fadelis](https://github.com/Fadelis) for the bug report.
- [#372] Simplify Kotlin example in user manual. Thanks to [Dustin Spicuzza](https://github.com/virtuald).

### 3.0.0-alpha-1
- [#245] New Feature: from 3.0, picocli offers an API for programmatic configuration.
- [#257] New Feature: new `ParseResult` class allows programmatic inspection of the result of parsing a sequence of command line arguments.
- [#144] New Feature: added support for mixins to allow reusing common options, positional parameters, subcommands and command attributes from any object.
- [#253] New Feature: added `@Unmatched` annotation for unmatched arguments.
- [#175] New Feature: `mixinStandardHelpOptions` attribute to install the standard `--help` and `--version` options, obviating the need for fields annotated with `@Option(usageHelp = true)` and `@Option(versionHelp = true)`.
- [#175] New Feature: picocli now provides a `HelpCommand` that can be installed as a subcommand on any application command to provide usage help for the parent command or sibling subcommands.
- [#175] New Feature: new `IHelpCommandInitializable` interface facilitates construction of custom help commands.
- [#250] Enhancement: the `run` and `call` convenience methods now follow convention: print to stdout when the user requested help, print to stderr when the input was invalid or an unexpected error occurred. Added `AbstractHandler` to facilitate following this convention for custom parse result handlers and exception handlers. 
- [#251] New Feature: exit code support. The built-in parse result handlers (`RunFirst`, `RunLast` and `RunAll`) and exception handler  (`DefaultExceptionHandler`) can now optionally specify an exit code. If specified, the handler terminates the JVM with the specified status code when finished. Custom handlers can extend `AbstractHandler` to inherit this behaviour. 
- [#262] New Feature: new `showDefaultValue` attribute on `@Option` and `@Parameters` gives fine-grained control over which default values to show or hide. Thanks to [ymenager](https://github.com/ymenager) for the request.
- [#268] New Feature: new `helpCommand` attribute on `@Command`: if the command line arguments contain a subcommand annotated with `helpCommand`, the parser will not validate the required options or positional parameters of the parent command. Thanks to [ymenager](https://github.com/ymenager) for the request.
- [#277] New Feature: new `hidden` attribute on `@Command` to omit the specified subcommand from the usage help message command list of the parent command. Thanks to [pditommaso](https://github.com/pditommaso).
- [#159] Enhancement: make help usage message width configurable. Thanks to [pditommaso](https://github.com/pditommaso).

### 3.0.0-alpha-2
- [#312] Enhancement and API change (against earlier alpha version): Remove `AbstractSimpleParseResultHandler` class and `parseWithSimpleHandlers` method.
- [#311] Enhancement and API change (against earlier alpha version): Simplify parseWithHandlers: removed prototypeReturnValue parameter.
- [#307] Enhancement: Provide CommandLine.usage(PrintWriter) method for testing and to facilitate [GROOVY-8520](https://issues.apache.org/jira/browse/GROOVY-8520) migration from commons-cli to picocli.
- [#306] Enhancement: Support generating autocompletion scripts for non-public @Command classes. Thanks to [cbeams](https://github.com/cbeams) for the request.
- [#308] Enhancement: Provide API to disallow POSIX clustered short options.
- [#310] Enhancement: PicocliBaseScript should follow conventions for stdout and stderr: requested help to stdout, invalid input usage help to stderr.
- [#309] Bugfix: Tests were failing on environments that support ANSI colors.

### 3.0.0-alpha-3
- [#313] Enhancement and New API: add method (later removed in 3.0.0-beta-1) `CommandLine::setMaxArityIsMaxTotalParams` to configure the parser to use `arity` to limit the total number of values accumulated in an option or positional parameter.
- [#314] Enhancement and New API: add method `CommandLine::setUsageHelpWidth` and `UsageMessageSpec::width` to set the max usage help message width.
- [#316] Enhancement: Support lenient mode where annotations are optional when extracting annotations.
- [#317] Enhancement: Change semantics of ParseResult.rawOptionValue to mean values after split (but before type conversion).

### 3.0.0-alpha-4
- [#318] API Change: Split model IBinding into IGetter and ISetter.
- [#320] API Change: Rename parser config `maxArityIsMaxTotalParams` to `arityRestrictsCumulativeSize`. (Property was removed in 3.0.0-beta-1.)
- [#216] Enhancement: Parsed values now replace the default value of multi-value (array, Collection or Map) options and positional parameters instead of being appended to them. Thanks to [wiwie](https://github.com/wiwie) for the request.
- [#261] Enhancement: Options and positional parameters with a `defaultValue` are never required. Thanks to [ymenager](https://github.com/ymenager) for the request.
- [#315] Enhancement: Initialize ArgSpec value with `defaultValue` before parsing command line.
- [#263] Bugfix: positional parameter defaults were not shown in usage help message. Thanks to [ymenager](https://github.com/ymenager) for the bug report.

### 3.0.0-alpha-5
- [#329] New API: Add parser configuration to control whether boolean flags should be toggled.
- [#328] New API: Provide getter methods on `OptionSpec.Builder` and `PositionalParamSpec.Builder`.
- [#326] New API: Add parser configuration to treat unmatched options as positional parameters.
- [#283] New API: Provide `getMissing` method on MissingParameterException to get a reference to the problematic options and positional parameters. Thanks to [jcapsule](https://github.com/jcapsule) for the suggestion.
- [#334] API Change: Rename `ArgSpec.rawStringValues()` to `ArgSpec.stringValues()`.
- [#342] API Change: Prefix ParseResult methods with `matched` if they return only matched options/positionals.
- [#340] API Change: Rename `ParseResult.optionValue(String, T)` to `matchedOptionValue(String, T)`.
- [#338] API Change: Remove `ParseResult.rawOptionValue(s)` and `rawPositionalValue(s)` methods.
- [#339] API Change: Remove `ParseResult.matchedOptionValue(OptionSpec)` and `matchedPositionalValue(PositionalParamSpec)` methods.
- [#347] API Change: Make `ArgSpec.getValue`, `setValue` and `isMultiValue` public methods.
- [#333] Enhancement: Added subcommand to synopsis in generated usage help. Thanks to [jcapsule](https://github.com/jcapsule) for the pull request.
- [#323] Enhancement: Remove dependency on java.sql package: picocli should only require the java.base module when running in Java 9.
- [#325] Enhancement: Allow custom type converter to map empty String to custom default value for empty options. Thanks to [jesselong](https://github.com/jesselong) for the suggestion.
- [#303] Enhancement: Improve validation to prevent common mistakes.
- [#70]  Enhancement: Positional parameters should only consume values where type conversion succeeds.
- [#346] Enhancement: Validate that arity min is never greater than max.
- [#348] Enhancement: Interpreter should call `ArgSpec.setValue` for every matched option or positional parameter.
- [#327] Bugfix: Default values should not cause options and positional parameters to be added to ParseResult.
- [#330] Bugfix: `Interpreter` should clear option's and positional parameter's `stringValues` list before parsing new input.
- [#335] Bugfix: Abstract class `ArgSpec` should not implement `equals` and `hashCode`. 
- [#345] Bugfix: Stop processing varargs when cumulative size reached. (This functionality was removed in 3.0.0-beta-1.)

### 3.0.0-alpha-6
- [#349] New API: Add `longestName()` convenience method to OptionSpec.
- [#352] New API: Add method to copy all attributes of a ParserSpec to a CommandSpec.
- [#353] New API: Add method to copy all attributes of a UsageMessageSpec to a CommandSpec.
- [#343] New API: Add method `Help.Ansi.Text::concat` and deprecate the `append` method. ("Append" suggests the Text object is modified, like StringBuilder, but Text is immutable.)
- [#350] Enhancement: Improve error message for `usageHelp` and `versionHelp` validation.
- [#344] Enhancement: Don't show WARN message for unmatched args or overwritten options.
- [#351] Documentation: Improve javadoc for OptionSpec.usageHelp and versionHelp.
- [#354] Bug fix: Interpreter should reset options and positional parameters to their initial value before parsing new input.

### 3.0.0-beta-1
- [#364] API Change: Remove parser option `arityRestrictsCumulativeSize`.
- [#355] API Change: Add method `ArgSpec.hasInitialValue`.
- [#361] API Change: Add parser option `aritySatisfiedByAttachedOptionParam` for commons-cli compatibility.
- [#363] API Change: Add parser option to limit the number of parts when splitting to max arity, for compatibility with commons-cli.
- [#360] Enhancement: Dynamically adjust width of long option name column (up to max 24).

### 3.0.0-beta-2
- [#366] API Change: Add `ArgSpec.getTypedValues()` method.
- [#365] Enhancement: Stricter arity validation: options with mandatory parameters no longer consume other option flags.
- [#357] Enhancement: Improve synopsis format. Be more succinct when `limitSplit` is true. Support repeating groups.


## <a name="3.0.0-deprecated"></a> Deprecations
### 3.0.0-alpha-1
The `picocli.CommandLine.Help::Help(Object, CommandLine.Help.ColorScheme)` constructor has been deprecated. Use the `picocli.CommandLine.Help::Help(CommandLine.CommandSpec, CommandLine.Help.ColorScheme)` constructor instead. 

The `picocli.CommandLine.IParseResultHandler` interface has been deprecated. Use the `picocli.CommandLine.IParseResultHandler2` interface instead.

The `picocli.CommandLine.IExceptionHandler` interface has been deprecated. Use the `picocli.CommandLine.IExceptionHandler2` interface instead.

### 3.0.0-alpha-6
- The `Help.Ansi.Text::append` method is now deprecated in favour of the new `concat` method.


## <a name="3.0.0-breaking-changes"></a> Potential breaking changes
### 3.0.0-alpha-1
#### Help API Changes
The following public fields were removed from the `picocli.CommandLine.Help` class. Instead, set these attributes on a `CommandLine.CommandSpec` object passed to any of the `Help` constructors.

* abbreviateSynopsis 
* commandListHeading 
* commandName 
* customSynopsis 
* description 
* descriptionHeading 
* footer 
* footerHeading 
* header 
* headerHeading 
* optionFields 
* optionListHeading 
* parameterLabelRenderer - replaced with the `Help.parameterLabelRenderer()` method
* parameterListHeading 
* requiredOptionMarker
* separator 
* showDefaultValues 
* sortOptions 
* synopsisHeading 

Method signature changes on inner classes and interfaces of the `Help` class:

* Interface method `CommandLine.Help.IOptionRenderer::render` signature changed: `CommandLine.Option` and `Field` parameters are replaced with a single `CommandLine.OptionSpec` parameter.
* Interface method `CommandLine.Help.IParameterRenderer::render` signature changed: `CommandLine.Parameters` and `Field` parameters are replaced with a single `CommandLine.PositionalParamSpec` parameter.
* Interface method `CommandLine.Help.IParamLabelRenderer::renderParameterLabel` signature changed: `Field` parameter replaced with `CommandLine.ArgSpec` parameter.
* Class `CommandLine.Help.Layout` all methods changed: `Field` parameters replaced by `CommandLine.ArgSpec`, `CommandLine.OptionSpec` and `CommandLine.PositionalParamSpec` parameters.

### 3.0.0-alpha-2
- [#311] API change from 3.0.0-alpha-1: the `parseWithHandlers` methods signature changed: removed the `prototypeReturnValue` parameter.
- [#312] API change from 3.0.0-alpha-1: Remove `AbstractSimpleParseResultHandler` class and `parseWithSimpleHandlers` method.

### 3.0.0-alpha-3
- Utility method `CommandLine.Help.join` signature changed: now takes an additional `usageHelpWidth` parameter.
- Constructor `CommandLine.Help.Layout(ColorScheme)` signature changed: now takes an additional `usageHelpWidth` parameter. 
- Public field `CommandLine.Help.TextTable.columns` is now private; added public method `CommandLine.Help.TextTable.columns()`.
- Constructor `CommandLine.Help.TextTable(Ansi)` is replaced with factory method `CommandLine.Help.TextTable.forDefaultColumns(Ansi, int)`.
- Constructor `CommandLine.Help.TextTable(Ansi, int...)` is replaced with factory method `CommandLine.Help.TextTable.forColumnWidths(Ansi, int...)`.
- Constructor `CommandLine.Help.TextTable(Ansi, Column...)` modifier changed from public to protected.
- Added factory method `CommandLine.Help.TextTable.forColumns(Ansi, Column...)`.
- Renamed `CommandLine.MaxValuesforFieldExceededException` to `CommandLine.MaxValuesExceededException`.

### 3.0.0-alpha-4
- Parsed values now replace the default value of multi-value (array, Collection or Map) options and positional parameters instead of being appended to them.
- The `IBinding` interface introduced in v3.0.0-alpha-1 has been replaced with two functional interfaces `IGetter` and `ISetter`.
- The `UnmatchedArgsBinding` factory methods introduced in v3.0.0-alpha-1 have been replaced with `forStringArrayConsumer` and `forStringCollectionSupplier`. 

### 3.0.0-alpha-5
Changes against earlier 3.0.0-alpha versions:

* Renamed `ArgSpec.rawStringValues()` to `ArgSpec.stringValues()`.
* Renamed `ParseResult` methods with `matched` if they return only matched options/positionals:
    * `options` to `matchedOptions`
    * `positionalParams` to `matchedPositionals`
    * `option(char)`, `option(String)` to `matchedOption`
    * `positional(int)` to `matchedPositional`
    * `hasOption(char)`, `hasOption(String)`, `hasOption(OptionSpec)` to `hasMatchedOption`
    * `hasPositional(int)`, `hasPositional(PositionalParamSpec)` to `hasMatchedPositional`
* Renamed `ParseResult.optionValue(String, T)` to `matchedOptionValue(String, T)`, and `positionalValue` to `matchedPositionalValue`.
* Removed `ParseResult::rawOptionValue(s)` and `rawPositionalValue(s)` methods.
* Removed `ParseResult.matchedOptionValue(OptionSpec)` and `matchedPositionalValue(PositionalParamSpec)` methods.

### 3.0.0-beta-1
- The usage message format changed: it now dynamically adjusts the width of the long option name column. This may break tests.
- API Change: Removed parser option `arityRestrictsCumulativeSize` introduced in 3.0.0-alpha-3.

### 3.0.0-beta-2
- Stricter arity validation may break existing applications that intentionally consume arguments regardless of whether arguments are other options.


# <a name="3.0.0-beta-2"></a> Picocli 3.0.0-beta-2
The picocli community is pleased to announce picocli 3.0.0-beta-2.

This release contains enhancements and bug fixes.

This is the twenty-eighth public release.
Picocli follows [semantic versioning](http://semver.org/).

## <a name="3.0.0-beta-2-toc"></a> Table of Contents
* [New and noteworthy](#3.0.0-beta-2-new)
* [Promoted features](#3.0.0-beta-2-promoted)
* [Fixed issues](#3.0.0-beta-2-fixes)
* [Deprecations](#3.0.0-beta-2-deprecated)
* [Potential breaking changes](#3.0.0-beta-2-breaking-changes)

## <a name="3.0.0-beta-2-new"></a> New and Noteworthy

### Stricter Arity Validation

Until this release, options with mandatory parameters would consume as many arguments as required, even if those arguments matched other option flags. For example:

Given a command like this:
```java
class App {
  @Option(names = "-a", arity = "2")
  String[] a;
  
  @Option(names = "-v")
  boolean v;
}
```
Prior to this change, the following input would be accepted:
```
<command> -a 1 -v
```
In previous versions, picocli accepted this and assigned `"1"` and `"-v"` as the two values for the `-a` option.
From this release, the parser notices that one of the arguments is an option and throws a `MissingParameterException` because not enough parameters were specified for the first option.


## <a name="3.0.0-beta-2-promoted"></a> Promoted Features
Promoted features are features that were incubating in previous versions of picocli but are now supported and subject to backwards compatibility. 

No features have been promoted in this picocli release.

## <a name="3.0.0-beta-2-fixes"></a> Fixed issues
- [#366] API Change: Add `ArgSpec.getTypedValues()` method.
- [#365] Enhancement: Stricter arity validation: options with mandatory parameters no longer consume other option flags.
- [#357] Enhancement: Improve synopsis format. Be more succinct when `limitSplit` is true. Support repeating groups.

## <a name="3.0.0-beta-2-deprecated"></a> Deprecations

## <a name="3.0.0-beta-2-breaking-changes"></a> Potential breaking changes
- Stricter arity validation may break existing applications that intentionally consume arguments regardless of whether arguments are other options.


# <a name="3.0.0-beta-1"></a> Picocli 3.0.0-beta-1
The picocli community is pleased to announce picocli 3.0.0-beta-1.

This release contains enhancements and bug fixes.

This is the twenty-seventh public release.
Picocli follows [semantic versioning](http://semver.org/).

## <a name="3.0.0-beta-1-toc"></a> Table of Contents
* [New and noteworthy](#3.0.0-beta-1-new)
* [Promoted features](#3.0.0-beta-1-promoted)
* [Fixed issues](#3.0.0-beta-1-fixes)
* [Deprecations](#3.0.0-beta-1-deprecated)
* [Potential breaking changes](#3.0.0-beta-1-breaking-changes)

## <a name="3.0.0-beta-1-new"></a> New and Noteworthy
Previously, the usage message layout had a fixed width long option name column: this column is always 24 characters, even if none of the options have a long option name.

This gives weird-looking usage help messages in some cases. For example:
```java
@Command(name="ls")
class App {
    @Option(names = "-a", description = "display all files") boolean a;
    @Option(names = "-l", description = "use a long listing format") boolean l;
    @Option(names = "-t", description = "sort by modification time") boolean t;
}
```

The usage message for this example was:
```
Usage: ls [-alt]
  -a                          display all files
  -l                          use a long listing format
  -t                          sort by modification time
```
From this release, picocli adjusts the width of the long option name column to the longest name (max 24).

The new usage message for this example looks like this:
```
Usage: ls [-alt]
  -a     display all files
  -l     use a long listing format
  -t     sort by modification time
```

## <a name="3.0.0-beta-1-promoted"></a> Promoted Features
Promoted features are features that were incubating in previous versions of picocli but are now supported and subject to backwards compatibility. 

No features have been promoted in this picocli release.

## <a name="3.0.0-beta-1-fixes"></a> Fixed issues
- [#364] API Change: Remove parser option `arityRestrictsCumulativeSize`.
- [#355] API Change: Add method `ArgSpec.hasInitialValue`.
- [#361] API Change: Add parser option `aritySatisfiedByAttachedOptionParam` for commons-cli compatibility.
- [#363] API Change: Add parser option to limit the number of parts when splitting to max arity, for compatibility with commons-cli.
- [#360] Enhancement: Dynamically adjust width of long option name column (up to max 24).


## <a name="3.0.0-beta-1-deprecated"></a> Deprecations

## <a name="3.0.0-beta-1-breaking-changes"></a> Potential breaking changes

- The usage message format changed: it now dynamically adjusts the width of the long option name column. This may break tests.
- API Change: Removed parser option `arityRestrictsCumulativeSize` introduced in 3.0.0-alpha-3.

# <a name="3.0.0-alpha-6"></a> Picocli 3.0.0-alpha-6
The picocli community is pleased to announce picocli 3.0.0-alpha-6.

This release contains enhancements and bug fixes.

This is the twenty-sixth public release.
Picocli follows [semantic versioning](http://semver.org/).

## <a name="3.0.0-alpha-6-toc"></a> Table of Contents
* [New and noteworthy](#3.0.0-alpha-6-new)
* [Promoted features](#3.0.0-alpha-6-promoted)
* [Fixed issues](#3.0.0-alpha-6-fixes)
* [Deprecations](#3.0.0-alpha-6-deprecated)
* [Potential breaking changes](#3.0.0-alpha-6-breaking-changes)

## <a name="3.0.0-alpha-6-new"></a> New and Noteworthy


## <a name="3.0.0-alpha-6-promoted"></a> Promoted Features
Promoted features are features that were incubating in previous versions of picocli but are now supported and subject to backwards compatibility. 

No features have been promoted in this picocli release.

## <a name="3.0.0-alpha-6-fixes"></a> Fixed issues
- [#349] New API: Add `longestName()` convenience method to OptionSpec.
- [#352] New API: Add method to copy all attributes of a ParserSpec to a CommandSpec.
- [#353] New API: Add method to copy all attributes of a UsageMessageSpec to a CommandSpec.
- [#343] New API: Add method `Help.Ansi.Text::concat` and deprecate the `append` method. ("Append" suggests the Text object is modified, like StringBuilder, but Text is immutable.)
- [#350] Enhancement: Improve error message for `usageHelp` and `versionHelp` validation.
- [#344] Enhancement: Don't show WARN message for unmatched args or overwritten options.
- [#351] Documentation: Improve javadoc for OptionSpec.usageHelp and versionHelp.
- [#354] Bug fix: Interpreter should reset options and positional parameters to their initial value before parsing new input.

## <a name="3.0.0-alpha-6-deprecated"></a> Deprecations
- The `Help.Ansi.Text::append` method is now deprecated in favour of the new `concat` method.

See [3.0.0-alpha-1](https://github.com/remkop/picocli/releases/tag/v3.0.0-alpha-1#3.0.0-alpha-1-deprecated)

## <a name="3.0.0-alpha-6-breaking-changes"></a> Potential breaking changes
See also breaking changes for 
[3.0.0-alpha-5](https://github.com/remkop/picocli/releases/tag/v3.0.0-alpha-5#3.0.0-alpha-5-breaking-changes),
[3.0.0-alpha-4](https://github.com/remkop/picocli/releases/tag/v3.0.0-alpha-4#3.0.0-alpha-4-breaking-changes),
[3.0.0-alpha-3](https://github.com/remkop/picocli/releases/tag/v3.0.0-alpha-3#3.0.0-alpha-3-breaking-changes),
[3.0.0-alpha-2](https://github.com/remkop/picocli/releases/tag/v3.0.0-alpha-2#3.0.0-alpha-2-breaking-changes),
and [3.0.0-alpha-1](https://github.com/remkop/picocli/releases/tag/v3.0.0-alpha-1#3.0.0-alpha-1-breaking-changes).




# <a name="3.0.0-alpha-5"></a> Picocli 3.0.0-alpha-5
The picocli community is pleased to announce picocli 3.0.0-alpha-5.

This release contains enhancements and bug fixes.

This is the twenty-fifth public release.
Picocli follows [semantic versioning](http://semver.org/).

## <a name="3.0.0-alpha-5-toc"></a> Table of Contents

* [New and noteworthy](#3.0.0-alpha-5-new)
* [Promoted features](#3.0.0-alpha-5-promoted)
* [Fixed issues](#3.0.0-alpha-5-fixes)
* [Deprecations](#3.0.0-alpha-5-deprecated)
* [Potential breaking changes](#3.0.0-alpha-5-breaking-changes)

## <a name="3.0.0-alpha-5-new"></a> New and Noteworthy


## <a name="3.0.0-alpha-5-promoted"></a> Promoted Features
Promoted features are features that were incubating in previous versions of picocli but are now supported and subject to backwards compatibility. 

No features have been promoted in this picocli release.

## <a name="3.0.0-alpha-5-fixes"></a> Fixed issues

- [#329] New API: Add parser configuration to control whether boolean flags should be toggled.
- [#328] New API: Provide getter methods on `OptionSpec.Builder` and `PositionalParamSpec.Builder`.
- [#326] New API: Add parser configuration to treat unmatched options as positional parameters.
- [#283] New API: Provide `getMissing` method on MissingParameterException to get a reference to the problematic options and positional parameters. Thanks to [jcapsule](https://github.com/jcapsule) for the suggestion.
- [#334] API Change: Rename `ArgSpec.rawStringValues()` to `ArgSpec.stringValues()`.
- [#342] API Change: Prefix ParseResult methods with `matched` if they return only matched options/positionals.
- [#340] API Change: Rename `ParseResult.optionValue(String, T)` to `matchedOptionValue(String, T)`.
- [#338] API Change: Remove `ParseResult.rawOptionValue(s)` and `rawPositionalValue(s)` methods.
- [#339] API Change: Remove `ParseResult.matchedOptionValue(OptionSpec)` and `matchedPositionalValue(PositionalParamSpec)` methods.
- [#347] API Change: Make `ArgSpec.getValue`, `setValue` and `isMultiValue` public methods.
- [#333] Enhancement: Added subcommand to synopsis in generated usage help. Thanks to [jcapsule](https://github.com/jcapsule) for the pull request.
- [#323] Enhancement: Remove dependency on java.sql package: picocli should only require the java.base module when running in Java 9.
- [#325] Enhancement: Allow custom type converter to map empty String to custom default value for empty options. Thanks to [jesselong](https://github.com/jesselong) for the suggestion.
- [#303] Enhancement: Improve validation to prevent common mistakes.
- [#70]  Enhancement: Positional parameters should only consume values where type conversion succeeds.
- [#346] Enhancement: Validate that arity min is never greater than max.
- [#348] Enhancement: Interpreter should call `ArgSpec.setValue` for every matched option or positional parameter.
- [#327] Bugfix: Default values should not cause options and positional parameters to be added to ParseResult.
- [#330] Bugfix: `Interpreter` should clear option's and positional parameter's `stringValues` list before parsing new input.
- [#335] Bugfix: Abstract class `ArgSpec` should not implement `equals` and `hashCode`. 
- [#345] Bugfix: Stop processing varargs when cumulative size reached.

## <a name="3.0.0-alpha-5-deprecated"></a> Deprecations
See [3.0.0-alpha-1](https://github.com/remkop/picocli/releases/tag/v3.0.0-alpha-1#3.0.0-alpha-1-deprecated)

## <a name="3.0.0-alpha-5-breaking-changes"></a> Potential breaking changes

* Renamed `ArgSpec.rawStringValues()` to `ArgSpec.stringValues()`.
* Renamed `ParseResult` methods with `matched` if they return only matched options/positionals:
    * `options` to `matchedOptions`
    * `positionalParams` to `matchedPositionals`
    * `option(char)`, `option(String)` to `matchedOption`
    * `positional(int)` to `matchedPositional`
    * `hasOption(char)`, `hasOption(String)`, `hasOption(OptionSpec)` to `hasMatchedOption`
    * `hasPositional(int)`, `hasPositional(PositionalParamSpec)` to `hasMatchedPositional`
* Renamed `ParseResult.optionValue(String, T)` to `matchedOptionValue(String, T)`, and `positionalValue` to `matchedPositionalValue`.
* Removed `ParseResult::rawOptionValue(s)` and `rawPositionalValue(s)` methods.
* Removed `ParseResult.matchedOptionValue(OptionSpec)` and `matchedPositionalValue(PositionalParamSpec)` methods.

See also breaking changes for 
[3.0.0-alpha-4](https://github.com/remkop/picocli/releases/tag/v3.0.0-alpha-4#3.0.0-alpha-4-breaking-changes),
[3.0.0-alpha-3](https://github.com/remkop/picocli/releases/tag/v3.0.0-alpha-3#3.0.0-alpha-3-breaking-changes),
[3.0.0-alpha-2](https://github.com/remkop/picocli/releases/tag/v3.0.0-alpha-2#3.0.0-alpha-2-breaking-changes),
and [3.0.0-alpha-1](https://github.com/remkop/picocli/releases/tag/v3.0.0-alpha-1#3.0.0-alpha-1-breaking-changes).




# <a name="3.0.0-alpha-4"></a> Picocli 3.0.0-alpha-4
The picocli community is pleased to announce picocli 3.0.0-alpha-4.

This release contains enhancements and bug fixes.

This is the twenty-fourth public release.
Picocli follows [semantic versioning](http://semver.org/).

## <a name="3.0.0-alpha-4-toc"></a> Table of Contents

* [New and noteworthy](#3.0.0-alpha-4-new)
* [Promoted features](#3.0.0-alpha-4-promoted)
* [Fixed issues](#3.0.0-alpha-4-fixes)
* [Deprecations](#3.0.0-alpha-4-deprecated)
* [Potential breaking changes](#3.0.0-alpha-4-breaking-changes)

## <a name="3.0.0-alpha-4-new"></a> New and Noteworthy


## <a name="3.0.0-alpha-4-promoted"></a> Promoted Features
Promoted features are features that were incubating in previous versions of picocli but are now supported and subject to backwards compatibility. 

No features have been promoted in this picocli release.

## <a name="3.0.0-alpha-4-fixes"></a> Fixed issues

- [#318] API Change: Split model IBinding into IGetter and ISetter.
- [#320] API Change: Rename parser config `maxArityIsMaxTotalParams` to `arityRestrictsCumulativeSize`.
- [#216] Enhancement: Parsed values now replace the default value of multi-value (array, Collection or Map) options and positional parameters instead of being appended to them. Thanks to [wiwie](https://github.com/wiwie) for the request.
- [#261] Enhancement: Options and positional parameters with a `defaultValue` are never required. Thanks to [ymenager](https://github.com/ymenager) for the request.
- [#315] Enhancement: Initialize ArgSpec value with `defaultValue` before parsing command line.
- [#263] Bugfix: positional parameter defaults were not shown in usage help message. Thanks to [ymenager](https://github.com/ymenager) for the bug report.

## <a name="3.0.0-alpha-4-deprecated"></a> Deprecations
See [3.0.0-alpha-1](https://github.com/remkop/picocli/releases/tag/v3.0.0-alpha-1#3.0.0-alpha-1-deprecated)

## <a name="3.0.0-alpha-4-breaking-changes"></a> Potential breaking changes
- Parsed values now replace the default value of multi-value (array, Collection or Map) options and positional parameters instead of being appended to them.
- The `IBinding` interface introduced in v3.0.0-alpha-1 has been replaced with two functional interfaces `IGetter` and `ISetter`.
- The `UnmatchedArgsBinding` factory methods introduced in v3.0.0-alpha-1 have been replaced with `forStringArrayConsumer` and `forStringCollectionSupplier`. 


See also breaking changes for [3.0.0-alpha-3](https://github.com/remkop/picocli/releases/tag/v3.0.0-alpha-3#3.0.0-alpha-3-breaking-changes),
[3.0.0-alpha-2](https://github.com/remkop/picocli/releases/tag/v3.0.0-alpha-2#3.0.0-alpha-2-breaking-changes),
and [3.0.0-alpha-1](https://github.com/remkop/picocli/releases/tag/v3.0.0-alpha-1#3.0.0-alpha-1-breaking-changes).



# <a name="3.0.0-alpha-3"></a> Picocli 3.0.0-alpha-3
The picocli community is pleased to announce picocli 3.0.0-alpha-3.

This release includes changes to allow picocli to be configured to function like Apache Commons CLI, to support [GROOVY-8520](https://issues.apache.org/jira/browse/GROOVY-8520):
* `maxArityIsMaxTotalParams` parser configuration option to use `arity` to limit the total number of values accumulated in an option or positional parameter.
* Usage message width can now be configured programmatically.
* "Lenient" mode when extracting annotations from a class where picocli annotations are optional (to allow mixing Groovy CLI annotations in Groovy CliBuilder).
* Change semantics of ParseResult.rawOptionValue to mean values after split (but before type conversion).

See [3.0.0-alpha-1](https://github.com/remkop/picocli/releases/tag/v3.0.0-alpha-1#3.0.0-alpha-1) and [3.0.0-alpha-2](https://github.com/remkop/picocli/releases/tag/v3.0.0-alpha-2#3.0.0-alpha-2) for recent functional changes.

This is the twenty-third public release.
Picocli follows [semantic versioning](http://semver.org/).

## <a name="3.0.0-alpha-3-toc"></a> Table of Contents

* [New and noteworthy](#3.0.0-alpha-3-new)
* [Promoted features](#3.0.0-alpha-3-promoted)
* [Fixed issues](#3.0.0-alpha-3-fixes)
* [Deprecations](#3.0.0-alpha-3-deprecated)
* [Potential breaking changes](#3.0.0-alpha-3-breaking-changes)

## <a name="3.0.0-alpha-3-new"></a> New and Noteworthy
### Max Arity Is Max Total Params

This release introduces a `maxArityIsMaxTotalParams` parser configuration option.

By default, the arity of an option is the number of arguments _for each occurrence_ of the option.
For example, if option `-a` has `arity = "2"`, then the following is a perfectly valid command:
for each occurrence of the option, two option parameters are specified.
```bash
<command> -a 1 2 -a 3 4 -a 5 6
```
However, if `CommandLine.setMaxArityIsMaxTotalParams(true)` is called first, the above example would result in a `MaxValuesExceededException` because the total number of values (6) exceeds the arity of 2.

Additionally, by default, when `maxArityIsMaxTotalParams` is `false`, arity is only applied _before_ the argument is split into parts,
while when `maxArityIsMaxTotalParams` is set to `true`, validation is applied _after_ a command line argument has been split into parts.

For example, if we have an option like this:
```java
@Option(name = "-a", arity = "1..2", split = ",") String[] values;
```
By default, the following input would be a valid command:
```bash
<command> -a 1,2,3
```
By default, the option arity tells the parser to consume 1 to 2 arguments, and the option was followed by a single parameter, `"1,2,3"`, which is fine.

However, if `maxArityIsMaxTotalParams` is set to true, the above example would result in a `MaxValuesExceededException` because the argument is split into 3 parts, which exceeds the max arity of 2.

## <a name="3.0.0-alpha-3-promoted"></a> Promoted Features
Promoted features are features that were incubating in previous versions of picocli but are now supported and subject to backwards compatibility. 

No features have been promoted in this picocli release.

## <a name="3.0.0-alpha-3-fixes"></a> Fixed issues

- [#313] Enhancement and New API: add method `CommandLine::setMaxArityIsMaxTotalParams` to configure the parser to use `arity` to limit the total number of values accumulated in an option or positional parameter.
- [#314] Enhancement and New API: add method `CommandLine::setUsageHelpWidth` and `UsageMessageSpec::width` to set the max usage help message width.
- [#316] Enhancement: Support lenient mode where annotations are optional when extracting annotations.
- [#317] Enhancement: Change semantics of ParseResult.rawOptionValue to mean values after split (but before type conversion).

## <a name="3.0.0-alpha-3-deprecated"></a> Deprecations
See [3.0.0-alpha-1](https://github.com/remkop/picocli/releases/tag/v3.0.0-alpha-1#3.0.0-alpha-1-deprecated)

## <a name="3.0.0-alpha-3-breaking-changes"></a> Potential breaking changes
- Utility method `CommandLine.Help.join` signature changed: now takes an additional `usageHelpWidth` parameter.
- Constructor `CommandLine.Help.Layout(ColorScheme)` signature changed: now takes an additional `usageHelpWidth` parameter. 
- Public field `CommandLine.Help.TextTable.columns` is now private; added public method `CommandLine.Help.TextTable.columns()`.
- Constructor `CommandLine.Help.TextTable(Ansi)` is replaced with factory method `CommandLine.Help.TextTable.forDefaultColumns(Ansi, int)`.
- Constructor `CommandLine.Help.TextTable(Ansi, int...)` is replaced with factory method `CommandLine.Help.TextTable.forColumnWidths(Ansi, int...)`.
- Constructor `CommandLine.Help.TextTable(Ansi, Column...)` modifier changed from public to protected.
- Added factory method `CommandLine.Help.TextTable.forColumns(Ansi, Column...)`.
- Renamed `CommandLine.MaxValuesforFieldExceededException` to `CommandLine.MaxValuesExceededException`.

See [3.0.0-alpha-2](https://github.com/remkop/picocli/releases/tag/v3.0.0-alpha-2#3.0.0-alpha-2-breaking-changes).
See [3.0.0-alpha-1](https://github.com/remkop/picocli/releases/tag/v3.0.0-alpha-1#3.0.0-alpha-1-breaking-changes).



# <a name="3.0.0-alpha-2"></a> Picocli 3.0.0-alpha-2
The picocli community is pleased to announce picocli 3.0.0-alpha-2.

This release includes some bug fixes and small enhancements. See [3.0.0-alpha-1](https://github.com/remkop/picocli/releases/tag/v3.0.0-alpha-1#3.0.0-alpha-1) for recent functional changes.

This is the twenty-second public release.
Picocli follows [semantic versioning](http://semver.org/).

## <a name="3.0.0-alpha-2-fixes"></a> Fixed issues

- [#312] Enhancement and API change: Remove `AbstractSimpleParseResultHandler` class and `parseWithSimpleHandlers` method.
- [#311] Enhancement and API change: Simplify parseWithHandlers: removed prototypeReturnValue parameter.
- [#307] Enhancement: Provide CommandLine.usage(PrintWriter) method for testing and to facilitate [GROOVY-8520](https://issues.apache.org/jira/browse/GROOVY-8520) migration from commons-cli to picocli.
- [#306] Enhancement: Support generating autocompletion scripts for non-public @Command classes. Thanks to [cbeams](https://github.com/cbeams) for the request.
- [#308] Enhancement: Provide API to disallow POSIX clustered short options.
- [#310] Enhancement: PicocliBaseScript should follow conventions for stdout and stderr: requested help to stdout, invalid input usage help to stderr.
- [#309] Bugfix: Tests were failing on environments that support ANSI colors.

## <a name="3.0.0-alpha-2-deprecated"></a> Deprecations
See [3.0.0-alpha-1](https://github.com/remkop/picocli/releases/tag/v3.0.0-alpha-1#3.0.0-alpha-1-deprecated)

## <a name="3.0.0-alpha-2-breaking-changes"></a> Potential breaking changes
- [#311] API change from 3.0.0-alpha-1: the `parseWithHandlers` methods signature changed: removed the `prototypeReturnValue` parameter.
- [#312] API change from 3.0.0-alpha-1: Remove `AbstractSimpleParseResultHandler` class and `parseWithSimpleHandlers` method.

See [3.0.0-alpha-1](https://github.com/remkop/picocli/releases/tag/v3.0.0-alpha-1#3.0.0-alpha-1-breaking-changes)

# <a name="3.0.0-alpha-1"></a> Picocli 3.0.0-alpha-1
The picocli community is pleased to announce picocli 3.0.0-alpha-1.

This release offers a programmatic API for creating command line applications, in addition to annotations. The programmatic API allows applications to dynamically create command line options on the fly, and also makes it possible to create idiomatic domain-specific languages for processing command line arguments, using picocli, in other JVM languages.

Another new feature in this release are Mixins. Mixins allow reusing common options, parameters and command attributes in multiple applications without copy-and-paste duplication.

Third, this release aims to reduce boilerplate code in user applications even further with the new `mixinStandardHelpOptions` command attribute. Picocli adds standard `usageHelp` and `versionHelp` options to commands with this attribute. Additionally picocli now offers a `HelpCommand` that can be installed as a subcommand on any application command to provide usage help for the parent command or sibling subcommands.

From this release, picocli is better at following unix conventions: print to stdout when the user requested help, and print to stderr when the input was invalid or an unexpected error occurred.

Also, this release gives better control over the process exit code.

Additionally, fields annotated with `@Unmatched` will be populated with the unmatched arguments.

Furthermore, this release adds a `showDefaultValue` attribute to the `@Option` and `@Parameters` annotation.

This is the twenty-first public release.
Picocli follows [semantic versioning](http://semver.org/).

## <a name="3.0.0-alpha-1-toc"></a> Table of Contents

* [New and noteworthy](#3.0.0-alpha-1-new)
* [Promoted features](#3.0.0-alpha-1-promoted)
* [Fixed issues](#3.0.0-alpha-1-fixes)
* [Deprecations](#3.0.0-alpha-1-deprecated)
* [Potential breaking changes](#3.0.0-alpha-1-breaking-changes)

## <a name="3.0.0-alpha-1-new"></a> New and Noteworthy

### <a name="3.0.0-alpha-1-Programmatic-API"></a> Programmatic API (INCUBATING)
This release offers a programmatic API for creating command line applications, in addition to annotations. The programmatic API allows applications to dynamically create command line options on the fly, and also makes it possible to create idiomatic domain-specific languages for processing command line arguments, using picocli, in other JVM languages.

Note that the programmatic API is incubating and the API may change in subsequent releases. _If you have suggestions for improving the programmatic API, please raise a ticket on GitHub!_

#### Example
```java
CommandSpec spec = CommandSpec.create();
spec.mixinStandardHelpOptions(true); // usageHelp and versionHelp options
spec.addOption(OptionSpec.builder("-c", "--count")
        .paramLabel("COUNT")
        .type(int.class)
        .description("number of times to execute").build());
spec.addPositional(PositionalParamSpec.builder()
        .paramLabel("FILES")
        .type(List.class)
        .auxiliaryTypes(File.class) // List<File>
        .description("The files to process").build());
CommandLine commandLine = new CommandLine(spec);

commandLine.parseWithSimpleHandlers(new AbstractSimpleParseResultHandler() {
    public void handle(ParseResult pr) {
        int count = pr.optionValue('c', 1);
        List<File> files = pr.positionalValue(0, Collections.<File>emptyList());
        for (int i = 0; i < count; i++) {
            for (File f : files) {
                System.out.printf("%d: %s%n", i, f);
            }
        }
    }
}, args);
```

#### CommandSpec (INCUBATING)
`CommandSpec` models a command. It is the programmatic variant of the `@Command` annotation. It has a name and a version, both of which may be empty.  It also has a `UsageMessageSpec` to configure aspects of the usage help message and a `ParserSpec` that can be used to control the behaviour of the parser.

#### OptionSpec and PositionalParamSpec (INCUBATING)
`OptionSpec` models a named option, and `PositionalParamSpec` models one or more positional parameters. They are the programmatic variant of the `@Option` and `@Parameters` annotations, respectively.

An `OptionSpec` must have at least one name, which is used during parsing to match command line arguments. Other attributes can be left empty and picocli will give them a reasonable default value. This defaulting is why `OptionSpec` objects are created with a builder: this allows you to specify only some attributes and let picocli initialise the other attributes. For example, if only the option’s name is specified, picocli assumes the option takes no parameters (arity = 0), and is of type `boolean`. Another example, if arity is larger than `1`, picocli sets the type to `List` and the `auxiliary type` to `String`.

`PositionalParamSpec` objects don’t have names, but have an index range instead. A single `PositionalParamSpec` object can capture multiple positional parameters. The default index range is set to `0..*` (all indices). A command may have multiple `PositionalParamSpec` objects to capture positional parameters at different index ranges. This can be useful if positional parameters at different index ranges have different data types.

Similar to `OptionSpec` objects, Once a `PositionalParamSpec` is constructed, its configuration becomes immutable, but its `value` can still be modified. Usually the value is set during command line parsing when a non-option command line argument is encountered at a position in its index range.

#### <a name="3.0.0-alpha-1-ParseResult"></a> ParseResult (INCUBATING)
A `ParseResult` class is now available that allows applications to inspect the result of parsing a sequence of command line arguments.

This class provides methods to query whether the command line arguments included certain options or position parameters, and what the value or values of these options and positional parameters was. Both the original command line argument String value as well as a strongly typed value can be obtained.


### Mixins for Reuse
Mixins are a convenient alternative to subclassing: picocli annotations from _any_ class can be added to ("mixed in" with) another command. This includes options, positional parameters, subcommands and command attributes. Picocli [autoHelp](#3.0.0-alpha-1-autohelp) internally uses a mixin.

A mixin is a separate class with options, positional parameters, subcommands and command attributes that can be reused in other commands. Mixins can be installed by calling the `CommandLine.addMixin` method with an object of this class, or annotating a field in your command with `@Mixin`. Here is an example mixin class:

```java
public class ReusableOptions {

    @Option(names = { "-v", "--verbose" }, description = {
        "Specify multiple -v options to increase verbosity.", "For example, `-v -v -v` or `-vvv`" })
    protected boolean[] verbosity = new boolean[0];
}
```

#### Adding Mixins Programmatically
The below example shows how a mixin can be added programmatically with the `CommandLine.addMixin` method.

```java
CommandLine commandLine = new CommandLine(new MyCommand());
commandline.addMixin("myMixin", new ReusableOptions());
```
#### `@Mixin` Annotation
A command can also include mixins by annotating fields with `@Mixin`. All picocli annotations found in the mixin class
are added to the command that has a field annotated with `@Mixin`. For example:

```java
@Command(name = "zip", description = "Example reuse with @Mixin annotation.")
public class MyCommand {

    // adds the options defined in ReusableOptions to this command
    @Mixin
    private ReusableOptions myMixin;
}
```


### <a name="3.0.0-alpha-1-mixinStandardHelpOptions"></a> Standard Help Options
This release introduces the `mixinStandardHelpOptions` command attribute. When this attribute is set to `true`, picocli adds a mixin to the command that adds `usageHelp` and `versionHelp` options to the command. For example:

```java
@Command(mixinStandardHelpOptions = true, version = "auto help demo - picocli 3.0")
class AutoHelpDemo implements Runnable {

    @Option(names = "--option", description = "Some option.")
    String option;

    @Override public void run() { }
}
```

Commands with `mixinStandardHelpOptions` do not need to explicitly declare fields annotated with `@Option(usageHelp = true)` and `@Option(versionHelp = true)` any more. The usage help message for the above example looks like this:
```text
Usage: <main class> [-hV] [--option=<option>]
      --option=<option>       Some option.
  -h, --help                  Show this help message and exit.
  -V, --version               Print version information and exit.
```

### <a name="3.0.0-alpha-1-HelpCommand"></a> Help Command

From this release, picocli provides a `help` subcommand (`picocli.CommandLine.HelpCommand`) that can be installed as a subcommand on any application command to provide usage help for the parent command or sibling subcommands. For example:

```java
@Command(subcommands = HelpCommand.class)
class AutoHelpDemo implements Runnable {

    @Option(names = "--option", description = "Some option.")
    String option;

    @Override public void run() { }
}
```


```text
# print help for the `maincommand` command
maincommand help

# print help for the `subcommand` command
maincommand help subcommand
```

For applications that want to create a custom help command, this release also introduces a new interface `picocli.CommandLine.IHelpCommandInitializable` that provides custom help commands with the information they need: access to the parent command and sibling commands, whether to use Ansi colors or not, and the streams to print the usage help message to.

### <a name="3.0.0-alpha-1-Unmatched"></a> `@Unmatched` Annotation
From this release, fields annotated with `@Unmatched` will be populated with the unmatched arguments.
The field must be of type `String[]` or `List<String>`.

If picocli finds a field annotated with `@Unmatched`, it automatically sets `unmatchedArgumentsAllowed` to `true`
so no `UnmatchedArgumentException` is thrown when a command line argument cannot be assigned to an option or positional parameter.

### <a name="3.0.0-alpha-1-std"></a> Stdout or Stderr
From picocli v3.0, the `run` and `call` convenience methods follow unix conventions:
print to stdout when the user requested help, and print to stderr when the input was invalid or an unexpected error occurred.

Custom handlers can extend `AbstractHandler` to facilitate following this convention.
`AbstractHandler` also provides `useOut` and `useErr` methods to allow customizing the target output streams,
and `useAnsi` to customize the Ansi style to use:

```java
@Command class CustomizeTargetStreamsDemo implements Runnable {
    public void run() { ... }

    public static void main(String... args) {
        CommandLine cmd = new CommandLine(new CustomizeTargetStreamsDemo());

        PrintStream myOut = getOutputPrintStream(); // custom stream to send command output to
        PrintStream myErr = getErrorPrintStream();  // custom stream for error messages

        cmd.parseWithHandlers(
                new RunLast().useOut(myOut).useAnsi(Help.Ansi.ON),
                new DefaultExceptionHandler().useErr(myErr).useAnsi(Help.Ansi.OFF),
                args);
    }
}
```

### <a name="3.0.0-alpha-1-exit-code"></a> Exit Code Support
From picocli v3.0, the built-in parse result handlers (`RunFirst`, `RunLast` and `RunAll`) and exception handler
(`DefaultExceptionHandler`) can specify an exit code.
If an exit code was specified, the handler terminates the JVM with the specified status code when finished.

```java
@Command class ExitCodeDemo implements Runnable {
    public void run() { throw new ParameterException(new CommandLine(this), "exit code demo"); }

    public static void main(String... args) {
        CommandLine cmd = new CommandLine(new ExitCodeDemo());
        cmd.parseWithHandlers(
                new RunLast().andExit(123),
                new DefaultExceptionHandler().andExit(456),
                args);
    }
}
```
Running this command prints the following to stderr and exits the JVM with status code `456`.

```
exit code demo
Usage: <main class>
```

Custom handlers can extend `AbstractHandler` to inherit this behaviour.


### Fine-grained ShowDefault

This release adds a `showDefaultValue` attribute to the `@Option` and `@Parameters` annotation. This allows you to specify for each individual option and positional parameter whether its default value should be shown in the usage help.

This attribute accepts three values:

* `ALWAYS` - always display the default value of this option or positional parameter, even `null` values, regardless what value of `showDefaultValues` was specified on the command
* `NEVER` - don't show the default value for this option or positional parameter, regardless what value of `showDefaultValues` was specified on the command
* `ON_DEMAND` - (this is the default) only show the default value for this option or positional parameter if `showDefaultValues` was specified on the command

The `NEVER` value is useful for security sensitive command line arguments like passwords. The `ALWAYS` value is useful when you only want to show the default value for a few arguments but not for all (in combination with `@Command(showDefaultValues = false)`).


## <a name="3.0.0-alpha-1-promoted"></a> Promoted features
Promoted features are features that were incubating in previous versions of picocli but are now supported and subject to backwards compatibility. 

No features have been promoted in this picocli release.

## <a name="3.0.0-alpha-1-fixes"></a> Fixed issues

- [#245] New Feature: from 3.0, picocli offers an API for programmatic configuration.
- [#257] New Feature: new `ParseResult` class allows programmatic inspection of the result of parsing a sequence of command line arguments.
- [#144] New Feature: added support for mixins to allow reusing common options, positional parameters, subcommands and command attributes from any object.
- [#253] New Feature: added `@Unmatched` annotation for unmatched arguments.
- [#175] New Feature: `mixinStandardHelpOptions` attribute to install the standard `--help` and `--version` options, obviating the need for fields annotated with `@Option(usageHelp = true)` and `@Option(versionHelp = true)`.
- [#175] New Feature: picocli now provides a `HelpCommand` that can be installed as a subcommand on any application command to provide usage help for the parent command or sibling subcommands.
- [#175] New Feature: new `IHelpCommandInitializable` interface facilitates construction of custom help commands.
- [#250] Enhancement: the `run` and `call` convenience methods now follow convention: print to stdout when the user requested help, print to stderr when the input was invalid or an unexpected error occurred. Added `AbstractHandler` to facilitate following this convention for custom parse result handlers and exception handlers. 
- [#251] New Feature: exit code support. The built-in parse result handlers (`RunFirst`, `RunLast` and `RunAll`) and exception handler  (`DefaultExceptionHandler`) can now optionally specify an exit code. If specified, the handler terminates the JVM with the specified status code when finished. Custom handlers can extend `AbstractHandler` to inherit this behaviour. 
- [#262] New Feature: new `showDefaultValue` attribute on `@Option` and `@Parameters` gives fine-grained control over which default values to show or hide. Thanks to [ymenager](https://github.com/ymenager) for the request.
- [#268] New Feature: new `helpCommand` attribute on `@Command`: if the command line arguments contain a subcommand annotated with `helpCommand`, the parser will not validate the required options or positional parameters of the parent command. Thanks to [ymenager](https://github.com/ymenager) for the request.
- [#277] New Feature: new `hidden` attribute on `@Command` to omit the specified subcommand from the usage help message command list of the parent command. Thanks to [pditommaso](https://github.com/pditommaso).
- [#159] Enhancement: make help usage message width configurable. Thanks to [pditommaso](https://github.com/pditommaso).

## <a name="3.0.0-alpha-1-deprecated"></a> Deprecations

The `picocli.CommandLine.Help::Help(Object, CommandLine.Help.ColorScheme)` constructor has been deprecated. Use the `picocli.CommandLine.Help::Help(CommandLine.CommandSpec, CommandLine.Help.ColorScheme)` constructor instead. 

The `picocli.CommandLine.IParseResultHandler` interface has been deprecated. Use the `picocli.CommandLine.IParseResultHandler2` interface instead.

The `picocli.CommandLine.IExceptionHandler` interface has been deprecated. Use the `picocli.CommandLine.IExceptionHandler2` interface instead.

## <a name="3.0.0-alpha-1-breaking-changes"></a> Potential breaking changes

### Help API Changes
The following public fields were removed from the `picocli.CommandLine.Help` class. Instead, set these attributes on a `CommandLine.CommandSpec` object passed to any of the `Help` constructors.

* abbreviateSynopsis 
* commandListHeading 
* commandName 
* customSynopsis 
* description 
* descriptionHeading 
* footer 
* footerHeading 
* header 
* headerHeading 
* optionFields 
* optionListHeading 
* parameterLabelRenderer - replaced with the `Help.parameterLabelRenderer()` method
* parameterListHeading 
* requiredOptionMarker
* separator 
* showDefaultValues 
* sortOptions 
* synopsisHeading 

Method signature changes on inner classes and interfaces of the `Help` class:

* Interface method `CommandLine.Help.IOptionRenderer::render` signature changed: `CommandLine.Option` and `Field` parameters are replaced with a single `CommandLine.OptionSpec` parameter.
* Interface method `CommandLine.Help.IParameterRenderer::render` signature changed: `CommandLine.Parameters` and `Field` parameters are replaced with a single `CommandLine.PositionalParamSpec` parameter.
* Interface method `CommandLine.Help.IParamLabelRenderer::renderParameterLabel` signature changed: `Field` parameter replaced with `CommandLine.ArgSpec` parameter.
* Class `CommandLine.Help.Layout` all methods changed: `Field` parameters replaced by `CommandLine.ArgSpec`, `CommandLine.OptionSpec` and `CommandLine.PositionalParamSpec` parameters.




# <a name="2.3.0"></a> Picocli 2.3.0
The picocli community is pleased to announce picocli 2.3.0.

This release contains bugfixes and new features.

This release introduces a new parser flag `stopAtPositional` to treat the first positional parameter as end-of-options, and a `stopAtUnmatched` parser flag to stop matching options and positional parameters as soon as an unmatched argument is encountered.

These flags are useful for applications that need to delegate part of the command line to third party commands.

This release offers better support for options with optional values, allowing applications to distinguish between cases where the option was not specified at all, and cases where the option was specified without a value.


This is the twentieth public release.
Picocli follows [semantic versioning](http://semver.org/).

## <a name="2.3.0-toc"></a> Table of Contents

* [New and noteworthy](#2.3.0-new)
* [Promoted features](#2.3.0-promoted)
* [Fixed issues](#2.3.0-fixes)
* [Deprecations](#2.3.0-deprecated)
* [Potential breaking changes](#2.3.0-breaking-changes)

## <a name="2.3.0-new"></a> New and noteworthy

### Stop At Positional
By default, positional parameters can be mixed with options on the command line, but this is not always desirable. From this release, applications can call `CommandLine.setStopAtPositional(true)` to force the parser to treat all values following the first positional parameter as positional parameters.

When this flag is set, the first positional parameter effectively serves as an "end of options" marker, without requiring a separate `--` argument.

### Stop At Unmatched
From this release, applications can call `CommandLine.setStopAtUnmatched(true)` to force the parser to stop interpreting options and positional parameters as soon as it encounters an unmatched argument.

When this flag is set, the first unmatched argument and all subsequent command line arguments are added to the unmatched arguments list returned by `CommandLine.getUnmatchedArguments()`.


### Optional Values
If an option is defined with `arity = "0..1"`, it may or not have a parameter value. If such an option is specified without a value on the command line, it is assigned an empty String. If the option is not specified, it keeps its default value. For example:

```java
class OptionalValueDemo implements Runnable {
    @Option(names = "-x", arity = "0..1", description = "optional parameter")
    String x;

    public void run() { System.out.printf("x = '%s'%n", x); }

    public static void main(String... args) {
       CommandLine.run(new OptionalValueDemo(), System.out, args);
    }
}
```
Gives the following results:
```bash
java OptionalValueDemo -x value
x = 'value'

java OptionalValueDemo -x
x = ''

java OptionalValueDemo
x = 'null'
```


## <a name="2.3.0-promoted"></a> Promoted features
Promoted features are features that were incubating in previous versions of picocli but are now supported and subject to backwards compatibility. 

No features have been promoted in this picocli release.

## <a name="2.3.0-fixes"></a> Fixed issues

- [#215] API: `stopAtUnmatched` flag to stop parsing on first unmatched argument. Thanks to [defnull](https://github.com/defnull) for the request.
- [#284] API: `stopAtPositional` flag to treat first positional parameter as end-of-options. Thanks to [defnull](https://github.com/defnull) and [pditommaso](https://github.com/pditommaso) for the request.
- [#279] Enhancement: assign empty String when String option was specified without value. Thanks to [pditommaso](https://github.com/pditommaso) for the request.
- [#285] Bugfix: Vararg positional parameters should not consume options. Thanks to [pditommaso](https://github.com/pditommaso) for the bug report.
- [#286] Documentation: clarify when picocli instantiates fields for options and positional parameters. Thanks to [JanMosigItemis](https://github.com/JanMosigItemis) for pointing this out.

## <a name="2.3.0-deprecated"></a> Deprecations

This release has no additional deprecations.

## <a name="2.3.0-breaking-changes"></a> Potential breaking changes

This release has no breaking changes.


# <a name="2.2.2"></a> Picocli 2.2.2
The picocli community is pleased to announce picocli 2.2.2.

This is a bugfix release.


This is the nineteenth public release.
Picocli follows [semantic versioning](http://semver.org/).

## <a name="2.2.2-toc"></a> Table of Contents

* [New and noteworthy](#2.2.2-new)
* [Promoted features](#2.2.2-promoted)
* [Fixed issues](#2.2.2-fixes)
* [Deprecations](#2.2.2-deprecated)
* [Potential breaking changes](#2.2.2-breaking-changes)

## <a name="2.2.2-new"></a> New and noteworthy

This is a bugfix release and does not include any new features.

## <a name="2.2.2-promoted"></a> Promoted features
Promoted features are features that were incubating in previous versions of picocli but are now supported and subject to backwards compatibility. 

No features have been promoted in this picocli release.

## <a name="2.2.2-fixes"></a> Fixed issues

- [#282] Bugfix: unmatched option heuristic did not work when there were no options to compare against. Thanks to [jcapsule](https://github.com/jcapsule).

## <a name="2.2.2-deprecated"></a> Deprecations

This release has no additional deprecations.

## <a name="2.2.2-breaking-changes"></a> Potential breaking changes

This release has no breaking changes.



# <a name="2.2.1"></a> Picocli 2.2.1
The picocli community is pleased to announce picocli 2.2.1.

This is a bugfix release.


This is the eighteenth public release.
Picocli follows [semantic versioning](http://semver.org/).

## <a name="2.2.1-toc"></a> Table of Contents

* [New and noteworthy](#2.2.1-new)
* [Promoted features](#2.2.1-promoted)
* [Fixed issues](#2.2.1-fixes)
* [Deprecations](#2.2.1-deprecated)
* [Potential breaking changes](#2.2.1-breaking-changes)

## <a name="2.2.1-new"></a> New and noteworthy

This is a bugfix release and does not include any new features.

## <a name="2.2.1-promoted"></a> Promoted features
Promoted features are features that were incubating in previous versions of picocli but are now supported and subject to backwards compatibility. 

No features have been promoted in this picocli release.

## <a name="2.2.1-fixes"></a> Fixed issues

- [#254] Bugfix: Starting from 2.0.3, usage help didn't render empty lines in multi-line descriptions.
- [#255] Documentation: Update Kotlin example in user manual for the new Kotlin 1.2 array literal syntax in annotations.

## <a name="2.2.1-deprecated"></a> Deprecations

This release has no additional deprecations.

## <a name="2.2.1-breaking-changes"></a> Potential breaking changes

This release has no breaking changes.




# <a name="2.2.0"></a> Picocli 2.2.0

The picocli community is pleased to announce picocli 2.2.

This release is a "Project Coin"-like release for picocli: small changes with a nice pay-off.


In command line applications with subcommands, options of the parent command are often intended as "global" options that apply to all the subcommands. This release introduces a new `@ParentCommand` annotation that makes it easy for subcommands to access such parent command options: fields of the subcommand annotated with `@ParentCommand` are initialized with a reference to the parent command. 

This release adds support for more built-in types, so applications don't need to register custom converters for common types. The new types include Java 7 classes like `java.nio.file.Path` and Java 8 classes like the value classes in the `java.time` package. These converters are loaded using reflection and are not available when running on Java 5 or Java 6.

This release also adds a `converter` attribute to the `@Option` and `@Parameter` annotations. This allows a specific option or positional parameter to use a different converter than would be used by default based on the type of the field.

Furthermore, the `@Command` annotation now supports a `versionProvider` attribute. This is useful when the version of an application should be detected dynamically at runtime. For example, an implementation may return version information obtained from the JAR manifest, a properties file or some other source.

Finally, applications may now specify a custom factory for instantiating classes that were configured as annotation attributes, like subcommands, type converters and version providers.



This is the seventeenth public release.
Picocli follows [semantic versioning](http://semver.org/).

## <a name="2.2.0-toc"></a> Table of Contents

* [New and noteworthy](#2.2.0-new)
* [Promoted features](#2.2.0-promoted)
* [Fixed issues](#2.2.0-fixes)
* [Deprecations](#2.2.0-deprecated)
* [Potential breaking changes](#2.2.0-breaking-changes)

## <a name="2.2.0-new"></a> New and noteworthy

### New `@ParentCommand` annotation 

In command line applications with subcommands, options of the top level command are often intended as "global" options that apply to all the subcommands. Prior to this release, subcommands had no easy way to access their parent command options unless the parent command made these values available in a global variable.

The `@ParentCommand` annotation makes it easy for subcommands to access their parent command options: subcommand fields annotated with `@ParentCommand` are initialized with a reference to the parent command. For example:

```java
@Command(name = "fileutils", subcommands = List.class)
class FileUtils {

    @Option(names = {"-d", "--directory"},
            description = "this option applies to all subcommands")
    File baseDirectory;
}

@Command(name = "list")
class List implements Runnable {

    @ParentCommand
    private FileUtils parent; // picocli injects reference to parent command

    @Option(names = {"-r", "--recursive"}, 
            description = "Recursively list subdirectories")
    private boolean recursive;

    @Override
    public void run() {
        list(new File(parent.baseDirectory, "."));
    }

    private void list(File dir) {
        System.out.println(dir.getAbsolutePath());
        if (dir.isDirectory()) {
            for (File f : dir.listFiles()) {
                System.out.println(f.getAbsolutePath());
                if (f.isDirectory() && recursive) {
                    list(f);
                }
            }
        }
    }
}
```

### More built-in types

This release adds support for more built-in types, so applications don't need to register custom converters for common types. The new types include Java 7 classes like `java.nio.file.Path` and Java 8 classes like the value classes in the `java.time` package. These converters are loaded using reflection and are not available when running on Java 5 or Java 6.

Converters for the following types were added in this release:

* `java.nio.file.Path` (requires Java 7 or higher)
* `java.time` value objects: `Duration`, `Instant`, `LocalDate`, `LocalDateTime`, `LocalTime`, `MonthDay`, `OffsetDateTime`, `OffsetTime`, `Period`, `Year`, `YearMonth`, `ZonedDateTime`, `ZoneId`, `ZoneOffset`  (requires Java 8 or higher, invokes the `parse` method of these classes)
* `java.lang.Class` (for the fully qualified class name)
* `java.nio.ByteOrder` (for the Strings `"BIG_ENDIAN"` or `"LITTLE_ENDIAN"`)
* `java.util.Currency` (for the ISO 4217 code of the currency)
* `java.net.NetworkInterface` (for the InetAddress or name of the network interface)
* `java.util.TimeZoneConverter` (for the ID for a TimeZone)
* `java.sql.Connection` (for a database url of the form `jdbc:subprotocol:subname`)
* `java.sql.Driver` (for a database URL of the form `jdbc:subprotocol:subname`)
* `java.sql.Timestamp` (for values in the `"yyyy-MM-dd HH:mm:ss"` or `"yyyy-MM-dd HH:mm:ss.fffffffff"` formats)

### Option-specific Type Converters
This release adds a `converter` attribute to the `@Option` and `@Parameter` annotations. This allows a specific option or positional parameter to use a different converter than would be used by default based on the type of the field.

For example, you may want to convert the constant names defined in [`java.sql.Types`](https://docs.oracle.com/javase/9/docs/api/java/sql/Types.html) to their `int` value for a specific field, but this should not impact any other `int` fields: other `int` fields should continue to use the default int converter that parses numeric values.

Example usage:

```java
class App {
    @Option(names = "--sqlType", converter = SqlTypeConverter.class)
    int sqlType;
}
```

Example implementation:

```java
class SqlTypeConverter implements ITypeConverter<Integer> {
    public Integer convert(String value) throws Exception {
        switch (value) {
            case "ARRAY"  : return Types.ARRAY;
            case "BIGINT" : return Types.BIGINT;
            case "BINARY" : return Types.BINARY;
            case "BIT"    : return Types.BIT;
            case "BLOB"   : return Types.BLOB;
            //...
        }
    }
}
```

### Dynamic Version Information
From this release, the `@Command` annotation supports a `versionProvider` attribute. Applications may specify a `IVersionProvider` implementation in this attribute, and picocli will instantiate this class
and invoke it to collect version information.

This is useful when the version of an application should be detected dynamically at runtime. For example, an implementation may return version information obtained from the JAR manifest, a properties file or some other source.

Custom version providers need to implement the `picocli.CommandLine.IVersionProvider` interface:

```java
public interface IVersionProvider {
    /**
     * Returns version information for a command.
     * @return version information (each string in the array is displayed on a separate line)
     * @throws Exception an exception detailing what went wrong when obtaining version information
     */
    String[] getVersion() throws Exception;
}
```

The GitHub project has a manifest file-based [example](https://github.com/remkop/picocli/blob/master/examples/src/main/java/picocli/examples/VersionProviderDemo2.java) and a build-generated version properties file-based [example](https://github.com/remkop/picocli/blob/master/examples/src/main/java/picocli/examples/VersionProviderDemo1.java) version provider implementation.

### Custom factory
Declaratively registered subcommands, type converters and version providers must be instantiated somehow. From this release, a custom factory can be specified when constructing a `CommandLine` instance. This allows full control over object creation and opens possibilities for Inversion of Control and Dependency Injection. For example:

```jshelllanguage
IFactory myFactory = getCustomFactory();
CommandLine cmdLine = new CommandLine(new Git(), myFactory);
```

## <a name="2.2.0-promoted"></a> Promoted features
Promoted features are features that were incubating in previous versions of picocli but are now supported and subject to backwards compatibility. 

No features have been promoted in this picocli release.

## <a name="2.2.0-fixes"></a> Fixed issues

- [#247] New `@ParentCommand` annotation to inject a reference to the parent command into subcommand fields. Thanks to [michaelpj](https://github.com/michaelpj) for the request.
- [#83]  Add more built-in converters. Thanks to [garydgregory](https://github.com/garydgregory/jcommander-addons) for the inspiration.
- [#237] Option and Positional Parameter-specific type converters. Thanks to [godzsa](https://github.com/godzsa) for the request.
- [#236] Allow obtaining version information dynamically at runtime. Thanks to [kcris](https://github.com/kcris) for the request.
- [#169] Configurable factory to instantiate subcommands that are registered via annotation attributes. Thanks to [kakawait](https://github.com/kakawait) for the request.
- [#252] Example version provider implementations.

## <a name="2.2.0-deprecated"></a> Deprecations

This release has no additional deprecations.

## <a name="2.2.0-breaking-changes"></a> Potential breaking changes

This release has no breaking changes.



# <a name="2.1.0"></a> Picocli 2.1.0

This release contains bugfixes and new features.

Users sometimes run into system limitations on the length of a command line when creating a command line with lots of options or with long arguments for options.
Starting from this release, picocli supports "argument files" or "@-files". Argument files are files that themselves contain arguments to the command. When picocli encounters an argument beginning with the character `@', it expands the contents of that file into the argument list.

Secondly, this release adds support for multi-value boolean flags. A common use case where this is useful is to let users control the level of output verbosity by specifying more `-v` flags on the command line. For example, `-v` could give high-level output, `-vv` could show more detailed output, and `-vvv` could show debug-level information.

Finally, thanks to [aadrian](https://github.com/aadrian) and [RobertZenz](https://github.com/RobertZenz), an `examples` subproject containing running examples has been added. (Your contributions are welcome!)
 
This is the sixteenth public release.
Picocli follows [semantic versioning](http://semver.org/).

## <a name="2.1.0-toc"></a> Table of Contents

* [New and noteworthy](#2.1.0-new)
* [Promoted features](#2.1.0-promoted)
* [Fixed issues](#2.1.0-fixes)
* [Deprecations](#2.1.0-deprecated)
* [Potential breaking changes](#2.1.0-breaking-changes)

## <a name="2.1.0-new"></a> New and noteworthy

### Argument Files (`@`-files)
An argument file can include options and positional parameters in any combination. The arguments within a file can be space-separated or newline-separated. If an argument contains embedded whitespace, put the whole argument in double or single quotes (`"-f=My Files\Stuff.java"`).

Lines starting with `#` are comments and are ignored. The file may itself contain additional @-file arguments; any such arguments will be processed recursively.

Multiple @-files may be specified on the command line.

For example, suppose a file with arguments exists at `/home/foo/args`, with these contents:

```text
# This line is a comment and is ignored.
ABC -option=123
'X Y Z'
```

A command may be invoked with the @file argument, like this:
```bash
java MyCommand @/home/foo/args
```

The above will be expanded to the contents of the file:
```bash
java MyCommand ABC -option=123 "X Y Z"
```

This feature is similar to the 'Command Line Argument File' processing supported by gcc, javadoc and javac.
The documentation for these tools shows further examples.

### Repeated Boolean Flags

Multi-valued boolean options are now supported. For example:
```jshelllanguage
@Option(names = "-v", description = { "Specify multiple -v options to increase verbosity.",
                                      "For example, `-v -v -v` or `-vvv`"})
boolean[] verbosity;
```

Users may specify multiple boolean flag options without parameters. For example:
```bash
<command> -v -v -v -vvv
```
The above example results in six `true` values being added to the `verbosity` array.


## <a name="2.1.0-promoted"></a> Promoted features
Promoted features are features that were incubating in previous versions of picocli but are now supported and subject to backwards compatibility. 

No features have been promoted in this picocli release.

## <a name="2.1.0-fixes"></a> Fixed issues

- [#126] New feature: Support expanding argument files, also called `@-files`.
- [#241] New feature (enhancing [#126]): Recursively process nested @-files; allow multiple arguments per line, allow quoted arguments with embedded whitespace.
- [#217] New feature: Support repeated boolean flag options captured in multi-valued fields.
- [#223] New feature: Added `examples` subproject containing running examples.  Thanks to [aadrian](https://github.com/aadrian) and [RobertZenz](https://github.com/RobertZenz).
- [#68]  Enhancement: Reject private final primitive fields annotated with @Option or @Parameters: because compile-time constants are inlined, updates by picocli to such fields would not be visible to the application.
- [#239] Enhancement: Improve error message when Exception thrown from Runnable/Callable.
- [#240] Bugfix: RunAll handler should return empty list, not null, when help is requested.
- [#244] Bugfix: the parser only considered `help` options instead of any of `help`, `usageHelp` and `versionHelp` to determine if missing required options can be ignored when encountering a subcommand. Thanks to [mkavanagh](https://github.com/mkavanagh).

## <a name="2.1.0-deprecated"></a> Deprecations

The `Range::defaultArity(Class)` method is now deprecated in favour of the `Range::defaultArity(Field)` method introduced in v2.0.

## <a name="2.1.0-breaking-changes"></a> Potential breaking changes

Private final fields that are either `String` or primitive types can no longer be annotated with `@Option` or `@Parameters`.
Picocli will throw an `InitializationException` when it detects such fields,
because compile-time constants are inlined, and updates by picocli to such fields would not be visible to the application.



# <a name="2.0.3"></a> Picocli 2.0.3

The picocli community is pleased to announce picocli 2.0.3.

This is a bugfix release that fixes a parser bug where the first argument following a clustered options was treated as a positional parameter, and removes the erroneous runtime dependency on `groovy-lang` that slipped in with the 2.0 release.

This release also includes a minor enhancement to support embedded newlines in usage help sections like header or descriptions. This allows Groovy applications to use [triple-quoted](http://groovy-lang.org/syntax.html#_triple_double_quoted_string) and [dollar slashy](http://groovy-lang.org/syntax.html#_dollar_slashy_string) multi-line strings in usage help sections.

This is the fifteenth public release.
Picocli follows [semantic versioning](http://semver.org/).

## <a name="2.0.3-toc"></a> Table of Contents

* [New and noteworthy](#2.0.3-new)
* [Promoted features](#2.0.3-promoted)
* [Fixed issues](#2.0.3-fixes)
* [Deprecations](#2.0.3-deprecated)
* [Potential breaking changes](#2.0.3-breaking-changes)

## <a name="2.0.3-new"></a> New and noteworthy

This is a bugfix release and does not include any new features.

## <a name="2.0.3-promoted"></a> Promoted features
Promoted features are features that were incubating in previous versions of picocli but are now supported and subject to backwards compatibility. 

No features have been promoted in this picocli release.

## <a name="2.0.3-fixes"></a> Fixed issues
- [#230] Enhancement: Support embedded newlines in usage help sections like header or descriptions. Thanks to [ddimtirov](https://github.com/ddimtirov).
- [#233] Bugfix: Parser bug: first argument following clustered options is treated as a positional parameter. Thanks to [mgrossmann](https://github.com/mgrossmann). 
- [#232] Bugfix: Remove required runtime dependency on `groovy-lang`. Thanks to [aadrian](https://github.com/aadrian). 

## <a name="2.0.3-deprecated"></a> Deprecations

This release does not deprecate any features.

## <a name="2.0.3-breaking-changes"></a> Potential breaking changes

This release does not include any breaking features.


# <a name="2.0.2"></a> Picocli 2.0.2

The picocli community is pleased to announce picocli 2.0.2.

This is a bugfix release that prevents an EmptyStackException from being thrown when the command line
ends in a cluster of boolean options, and furthermore fixes two scripting-related minor issues.

This is the fourteenth public release.
Picocli follows [semantic versioning](http://semver.org/).

## <a name="2.0.2-toc"></a> Table of Contents

* [New and noteworthy](#2.0.2-new)
* [Promoted features](#2.0.2-promoted)
* [Fixed issues](#2.0.2-fixes)
* [Deprecations](#2.0.2-deprecated)
* [Potential breaking changes](#2.0.2-breaking-changes)

## <a name="2.0.2-new"></a> New and noteworthy

This is a bugfix release and does not include any new features.

## <a name="2.0.2-promoted"></a> Promoted features
Promoted features are features that were incubating in previous versions of picocli but are now supported and subject to backwards compatibility. 

No features have been promoted in this picocli release.

## <a name="2.0.2-fixes"></a> Fixed issues

- [#226] Bugfix: EmptyStackException when command line ends in a cluster of boolean options. Thanks to [RobertZenz](https://github.com/RobertZenz).
- [#222] Bugfix: Register default converter for Object fields for better scripting support.
- [#219] Bugfix: Command line system property -Dpicocli.trace (without value) throws exception when used with Groovy.
- [#220] Enhancement: Improve tracing for positional parameters (provide detail on current position).
- [#221] Enhancement: Add documentation for Grapes bug workaround on Groovy versions before 2.4.7.

## <a name="2.0.2-deprecated"></a> Deprecations

This release does not deprecate any features.

## <a name="2.0.2-breaking-changes"></a> Potential breaking changes

This release does not include any breaking features.


# <a name="2.0.1"></a> Picocli 2.0.1

The picocli community is pleased to announce picocli 2.0.1.

This is a bugfix release that removes a dependency on Java 1.7 which was accidentally included.

This is the thirteenth public release.
Picocli follows [semantic versioning](http://semver.org/).

## <a name="2.0.1-toc"></a> Table of Contents

* [New and noteworthy](#2.0.1-new)
* [Promoted features](#2.0.1-promoted)
* [Fixed issues](#2.0.1-fixes)
* [Deprecations](#2.0.1-deprecated)
* [Potential breaking changes](#2.0.1-breaking-changes)

## <a name="2.0.1-new"></a> New and noteworthy

This is a bugfix release and does not include any new features.

## <a name="2.0.1-promoted"></a> Promoted features
Promoted features are features that were incubating in previous versions of picocli but are now supported and subject to backwards compatibility. 

No features have been promoted in this picocli release.

## <a name="2.0.1-fixes"></a> Fixed issues

- [#214] Removed a dependency on Java 1.7 that was accidentally included. Thanks to [sjsajj](https://github.com/sjsajj).

## <a name="2.0.1-deprecated"></a> Deprecations

This release does not deprecate any features.

## <a name="2.0.1-breaking-changes"></a> Potential breaking changes

This release does not include any breaking features.


# <a name="2.0"></a> Picocli 2.0

The picocli community is pleased to announce picocli 2.0.

First, picocli now provides tight integration for Groovy scripts.
The new `@picocli.groovy.PicocliScript` annotation allows Groovy scripts to use the `@Command` annotation,
and turns a Groovy script into a picocli-based command line application.
When the script is run, `@Field` variables annotated with `@Option` or `@Parameters`
are initialized from the command line arguments before the script body is executed.

Applications with subcommands will also benefit from upgrading to picocli 2.0.
The `CommandLine.run` and `CommandLine.call` methods now work for subcommands,
and if more flexibility is needed, take a look at the new `parseWithHandler` methods.
Error handling for subcommands has been improved in this release with the new `ParseException.getCommandLine` method.

Improved ease of use: Usage help is now printed automatically from the `CommandLine.run` and `CommandLine.call` methods
and from the built-in handlers used with the `parseWithHandler` method.

The parser has been improved and users can now mix positional arguments with options on the command line.
Previously, positional parameter had to follow the options.
**Important notice:** To make this feature possible, the default parsing behaviour of multi-value options and parameters changed to be non-greedy by default.

Finally, this release includes many [bug fixes](#2.0-fixes).

This is the twelfth public release.
Picocli follows [semantic versioning](http://semver.org/).

## <a name="2.0-toc"></a> Table of Contents

* [New and noteworthy](#2.0-new)
    * [Groovy Script Support](#2.0-groovy-script)
    * [Better Subcommand Support](#2.0-subcommands)
    * [Easier To Use](#2.0-ease-of-use)
    * [Parser Improvements](#2.0-parser-improvements) 
    * [Usage Help Format Improvements](#2.0-help-improvements)
* [Promoted features](#2.0-promoted)
* [Fixed issues](#2.0-fixes)
* [Deprecations](#2.0-deprecated)
* [Potential breaking changes](#2.0-breaking-changes)



## <a name="2.0-new"></a> New and noteworthy

### <a name="2.0-groovy-script"></a> Groovy Script Support
Picocli 2.0 introduces special support for Groovy scripts.

Scripts annotated with `@picocli.groovy.PicocliScript` are automatically transformed to use
`picocli.groovy.PicocliBaseScript` as their base class and can also use the `@Command` annotation to
customize parts of the usage message like command name, description, headers, footers etc.

Before the script body is executed, the `PicocliBaseScript` base class parses the command line and initializes
`@Field` variables annotated with `@Option` or `@Parameters`.
The script body is executed if the user input was valid and did not request usage help or version information.

```groovy
// exampleScript.groovy
@Grab('info.picocli:picocli:2.0.0')
@Command(name = "myCommand", description = "does something special")
@PicocliScript
import picocli.groovy.PicocliScript
import picocli.CommandLine.Command
import picocli.CommandLine.Option
import groovy.transform.Field

@Option(names = "-x", description = "number of repetitions")
@Field int count;

@Option(names = ["-h", "--help"], usageHelp = true, description = "print this help message and exit")
@Field boolean helpRequested;

//if (helpRequested) { // not necessary: PicocliBaseScript takes care of this
//    CommandLine.usage(this, System.err); return 0;
//}
count.times {
   println "hi"
}
// the CommandLine that parsed the args is available as a property
assert this.commandLine.commandName == "myCommand"
```

### <a name="2.0-subcommands"></a> Better Subcommand Support
New methods `CommandLine::parseWithHandler` were added. These methods intend to offer the same ease of use as 
the `run` and `call` methods, but with more flexibility and better support for nested subcommands.

The `CommandLine::call` and `CommandLine::run` now support subcommands and will execute the **last** subcommand 
specified by the user. (Previously subcommands were ignored and only the top-level command was executed.)

From this release, picocli exceptions provide a `getCommandLine` method 
that returns the command or subcommand where parsing or execution failed.
Previously, if the user provided invalid input for applications with subcommands,
it was difficult to pinpoint exactly which subcommand failed to parse the input.

### <a name="2.0-ease-of-use"></a> Easier To Use
The convenience methods will automatically print usage help and version information
when the user specifies options annotated with the `versionHelp` or `usageHelp` attributes.

Methods that automatically print help:

* CommandLine::call
* CommandLine::run
* CommandLine::parseWithHandler (with the built-in Run…​ handlers)
* CommandLine::parseWithHandlers (with the built-in Run…​ handlers)

Methods that do not automatically print help:

* CommandLine::parse
* CommandLine::populateCommand

Also notable: Collection and Map fields no longer require the `type` annotation attribute: 
picocli now infers the conversion target type from the generic type parameters where possible.

### <a name="2.0-parser-improvements"></a> Parser Improvements 
Positional parameters can now be mixed with options on the command line. Previously, positional parameter had to follow the options.

To make this feature possible, the default parsing behaviour of multi-value options and parameters changed to be non-greedy by default.

### <a name="2.0-help-improvements"></a> Usage Help Format Improvements
This release contains various bugfixes related to improving the usage help format for multi-value options and collections.
For example, for Maps that don't have a `paramLabel`, picocli now shows key type and value type instead of the internal Java field name.

## <a name="2.0-promoted"></a> Promoted features
Promoted features are features that were incubating in previous versions of picocli but are now supported and subject to backwards compatibility. 

The following are the features that have been promoted in this picocli release.

* Autocomplete - as of picocli 2.0, autocompletion is no longer beta.

## <a name="2.0-fixes"></a> Fixed issues

- [#207] API Change: Provide ability to find which subcommand threw a ParameterException API enhancement. Thanks to [velit](https://github.com/velit) and [AshwinJay](https://github.com/AshwinJay).
- [#179] API Change to remove full JRE dependency and require only Compact Profile. Replace use of `java.awt.Point` with `picocli.CommandLine.Help.TextTable.Cell`. Thanks to [webfolderio](https://github.com/webfolderio).
- [#205] API Change: `CommandLine::getCommand` now returns a generic type instead of Object so client code can avoid type casting.
- [#196] API Change: `Option::type()` and `Parameters::type()` now return empty array by default (was `{String.class}`).
- [#210] API Change: Deprecated the option `help` attribute in favour of the `usageHelp` and `versionHelp` attributes.
- [#115] New feature: Added new `CommandLine::parseWithHandler` convenience methods for commands with subcommands.
- [#130] New feature: Options and positional parameters can now be mixed on the command line.
- [#196] New feature: Infer type from collections and maps when `type` annotation not specified. Thanks to [ddimtirov](https://github.com/ddimtirov) for the suggestion.
- [#197] New feature: Use `type` attribute to determine conversion target type instead of field type. This allows fields to be declared as interfaces or abstract types (or arrays/collections/maps of these) and via the `type` attribute picocli will be able to convert String arguments to concrete implementation objects.
- [#187] New feature: Added methods to programmatically get and set the command name.
- [#192] Bugfix: Default arity should be 1, not *, for array and collection options. Thanks to [RobertZenz](https://github.com/RobertZenz).
- [#193] Bugfix: Splitting an argument should not cause max arity to be exceeded.
- [#191] Bugfix: Arity should not limit the total number of values put in an array or collection. Thanks to [RobertZenz](https://github.com/RobertZenz).
- [#186] Bugfix: Confusing usage message for collection options. Thanks to [AlexFalappa](https://github.com/AlexFalappa).
- [#181] Bugfix: Incorrect help message was displayed for short options with paramLabel when arity > 1.
- [#184] Bugfix/Enhancement: Programmatically setting the separator is now reflected in the usage help message. Thanks to [defnull](https://github.com/defnull).
- [#200] Bugfix: Prevent NPE when command name is set to empty string or spaces. Thanks to [jansohn](https://github.com/jansohn).
- [#203] Bugfix: Empty string parameter parsed incorrectly. Thanks to [AshwinJay](https://github.com/AshwinJay).
- [#194] Enhancement: Usage help should show split regex for option/parameters.
- [#198] Enhancement: Usage help parameter list details should indicate arity for positional parameters.
- [#195] Enhancement: Usage help should show Map types if paramLabel not specified.
- [#183] Enhancement: Add examples to user manual for using picocli in other JVM languages. Thanks to [binkley](https://github.com/binkley) for pointing out that Kotlin may support array literals in annotations from 1.2.
- [#185] Enhancement: Exception message text for missing options should not use field names but be more descriptive and consistent with usage help. Thanks to [AlexFalappa](https://github.com/AlexFalappa).
- [#201] Enhancement: Usage help should not show `null` default values. Thanks to [jansohn](https://github.com/jansohn).
- [#202] Enhancement: Java 9: add `Automatic-Module-Name: info.picocli` to MANIFEST.MF to make picocli play nice in the Java 9 module system.
- [#204] Enhancement: instantiate `LinkedHashSet` instead of `HashSet` for `Set` fields to preserve input ordering.
- [#208] Enhancement: Remove pom.xml, which was not being maintained. Picocli can only be built with gradle going forward.
- [#212] Enhancement: Improve javadoc for picocli.AutoComplete.

## <a name="2.0-deprecated"></a> Deprecations
The `help` attribute for options is now deprecated. Please change to use `usageHelp` and `versionHelp` attributes instead.
From picocli v2.0, the convenience methods will automatically print usage help and version information
when requested with the `versionHelp` and `usageHelp` option attributes (but not for the `help` attribute).


## <a name="2.0-breaking-changes"></a> Potential breaking changes

This release has a number of incompatible changes:
* Multi-value options (array, list and map fields) are **not greedy by default** any more.
* **Arity is not max values**: end users may specify multi-value options (array, list and map fields) an unlimited number of times.
* A single argument that is split into parts with a regex now **counts as a single argument** (so `arity="1"` won't prevent all parts from being added to the field)
* API change: replaced `java.awt.Point` with custom `Cell` class as return value type for public method `Help.TextTable.putValue()`.
* API change: `@Option.type()` and `@Parameters.type()` now return an empty array by default (was `{String.class}`).
* API change: `ParameterException` and all subclasses now require a `CommandLine` object indicating the command or subcommand that the user provided invalid input for.
* The `CommandLine::call` and `CommandLine::run` now support subcommands and will execute the **last** subcommand specified by the user. Previously subcommands were ignored and the top-level command was executed unconditionally.

I am not happy about the disruption these changes may cause, but I felt they were needed for three reasons:
the old picocli v1.0 behaviour caused ambiguity in common use cases,
was inconsistent with most Unix tools, 
and prevented supporting mixing options with positional arguments on the command line.

To illustrate the new non-greedy behaviour, consider this example program:
```java
class MixDemo {
  @Option(names="-o") List<String> options;
  @Parameters         List<String> positionalParams;

  public void run() {
    System.out.println("positional: " + positionalParams);
    System.out.println("options   : " + options);
  }

  public static void main(String[] args) {
    CommandLine.run(new MixDemo(), System.err, args);
  }
}
```
We run this program as below, where the option is followed by multiple values:

```bash
$ java MixDemo -o 1 2 3
```

Previously, the arguments following `-o` would all end up in the `options` list. Running the above command with picocli 1.0 would print out the following:

```bash
# (Previously, in picocli-1.0.1)
$ java MixDemo -o 1 2 3

positional: null
options   : [1, 2, 3]
```

From picocli 2.0, only the first argument following `-o` is added to the `options` list, the remainder is parsed as positional parameters:

```bash
# (Currently, in picocli-2.0)
$ java MixDemo -o 1 2 3

positional: [2, 3]
options   : [1]
```

To put multiple values in the options list in picocli 2.0, users can specify the `-o` option multiple times:
```bash
$ java MixDemo -o 1 -o 2 -o 3

positional: null
options   : [1, 2, 3]
```

Alternatively, application authors can make a multi-value option greedy in picocli v2.0 by explicitly setting a variable arity:
```java
class Args {
    @Option(names = "-o", arity = "1..*") List<String> options;
}
```
(... "greedy" means consume until the next option, so not necessarily all remaining command line arguments.)



# <a name="1.0.1"></a> 1.0.1 - Bugfix release.

## <a name="1.0.1-summary"></a> Summary: zsh autocompletion bugfix

This is the eleventh public release.
Picocli follows [semantic versioning](http://semver.org/).

- [#178] Fixed autocompletion bug for subcommands in zsh. Autocomplete on zsh would show only the global command options even when a subcommand was specified. Autocompletion now works for nested subcommands.

# <a name="1.0.0"></a> 1.0.0 - Bugfix and enhancements release.

## <a name="1.0.0-summary"></a> Summary

New features: command line autocompletion, `-Dkey=value`-like Map options and parser tracing.

Non-breaking changes to support Callable commands, Map options and format specifiers in version help.

This is the tenth public release.
Picocli follows [semantic versioning](http://semver.org/).

## <a name="1.0.0-fixes"></a> Fixed issues

* [#121] New feature: command line autocompletion. Picocli can generate bash and zsh completion scripts that allow the shell to generate potential completion matches based on the `@Option` and `@Command` annotations in your application. After this script is installed, the shell will show the options and subcommands available in your java command line application, and in some cases show possible option values.
* [#67] New feature: Map options like `-Dkey1=val1 -Dkey2=val2`. Both key and value can be strongly typed (not just Strings).
* [#158] New feature: parser TRACING for easy troubleshooting. The trace level can be controlled with a system property.
* [#170] New feature: added `call` convenience method similar to `run`. Applications whose main business logic may throw an exception or returns a result can now implement `Callable` and reduce some boilerplate code.
* [#149] Parser now throws UnmatchedArgumentException for args that resemble options but are not, instead of treating like them positional parameters. Thanks to [giaco777](https://github.com/giaco777).
* [#172] Parser now throws MaxValuesforFieldExceededException when multi-valued option or parameters max arity exceeded
* [#173] Parser now throws UnmatchedArgumentException when not all positional parameters are assigned to a field
* [#171] WARN when option overwritten with different value (when isOverwrittenOptionsAllowed=true); WARN for unmatched args (when isUnmatchedArgumentsAllowed=true). Thanks to [ddimtirov](https://github.com/ddimtirov).
* [#164] API change: Support format patterns in version string and printVersionHelp
* [#167] API change: Change `type` attribute from `Class` to `Class[]`. This was needed for Map options support.
* [#168] API change: `CommandLine::setSeparator` method now returns this CommandLine (was void), allowing for chained method calls.
* [#156] Added example to user manual to clarify main command common usage. Thanks to [nagkumar](https://github.com/nagkumar).
* [#166] Fixed bug where adjacent markup sections resulted in incorrect ANSI escape sequences
* [#174] Fixed bug where under some circumstances, unmatched parameters were added to UnmatchedParameters list twice

# <a name="0.9.8"></a> 0.9.8 - Bugfix and enhancements release for public review. API may change.

## <a name="0.9.8-summary"></a> Summary

Non-breaking changes to add better help support and better subcommand support.

## <a name="0.9.8-fixes"></a> Fixed issues

* [#162] Added new Version Help section to user manual; added `version` attribute on `@Command`; added `CommandLine::printVersionHelp` convenience method to print version information from this annotation to the console
* [#145] Added `usageHelp` and `versionHelp` attributes on `@Option`; added `CommandLine::isUsageHelpRequested` and `CommandLine::isVersionHelpRequested` to allow external components to detect whether usage help or version information was requested (without inspecting the annotated domain object). Thanks to [kakawait](https://github.com/kakawait).
* [#160] Added `@since` version in javadoc for recent API changes.
* [#157] API change: added `CommandLine::getParent` method to get the parent command of a subcommand. Thanks to [nagkumar](https://github.com/nagkumar).
* [#152] Added support for registering subcommands declaratively with the `@Command(subcommands#{...})` annotation. Thanks to [nagkumar](https://github.com/nagkumar).
* [#146] Show underlying error when type conversion fails
* [#147] Toggle boolean flags instead of setting to `true`
* [#148] Long string in default value no longer causes infinite loop when printing usage. Thanks to [smartboyathome](https://github.com/smartboyathome).
* [#142] First line of long synopsis no longer overshoots 80-character usage help width. Thanks to [waacc-gh](https://github.com/waacc-gh).

# <a name="0.9.7"></a> 0.9.7 - Bugfix and enhancements release for public review. API may change.

## <a name="0.9.7-summary"></a> Summary

Version 0.9.7 has some breaking API changes.

**Better Groovy support**

It was [pointed out](https://github.com/remkop/picocli/issues/135) that Groovy had trouble distinguishing between
the static `parse(Object, String...)` method and the instance method `parse(String...)`.

To address this, the static `parse(Object, String...)` method has been renamed
to `populateCommand(Object, String...)` in  version 0.9.7.

**Nested subcommands**

* Version 0.9.7 adds support for [nested sub-subcommands](https://github.com/remkop/picocli/issues/127)
* `CommandLine::parse` now returns `List<CommandLine>` (was `List<Object>`)
* `CommandLine::getCommands` now returns `Map<String, CommandLine>` (was `Map<String, Object>`)
* renamed method `CommandLine::addCommand` to `addSubcommand`
* renamed method `CommandLine::getCommands` to `getSubcommands`

**Miscellaneous**

Renamed class `Arity` to `Range` since it is not just used for @Option and @Parameters `arity` but also for `index` in positional @Parameters.

## <a name="0.9.7-fixes"></a> Fixed issues

* [#127] Added support for nested sub-subcommands
* [#135] API change: renamed static convenience method `CommandLine::parse` to `populateCommand`
* [#134] API change: `CommandLine::parse` now returns `List<CommandLine>` (was `List<Object>`)
* [#133] API change: `CommandLine::getCommands` now returns `Map<String, CommandLine>` (was `Map<String, Object>`)
* [#133] API change: Added method `CommandLine::getCommand`
* [#136] API change: renamed method `CommandLine::addCommand` to `addSubcommand`;
* [#136] API change: renamed method `CommandLine::getCommands` to `getSubcommands`
* [#131] API change: Renamed class `Arity` to `Range`
* [#137] Improve validation: disallow index gap in @Parameters annotations
* [#132] Improve validation: parsing should fail when unmatched arguments remain
* [#138] Improve validation: disallow option overwriting by default
* [#129] Make "allow option overwriting" configurable
* [#140] Make "allow unmatched arguments" configurable
* [#139] Improve validation: CommandLine must be constructed with a command that has at least one of @Command, @Option or @Parameters annotation
* [#141] Bugfix: prevent NullPointerException when sorting required options/parameters

# <a name="0.9.6"></a> 0.9.6 - Bugfix release for public review. API may change.

* [#128] Fix unexpected MissingParameterException when a help-option is supplied (bug)

# <a name="0.9.5"></a> 0.9.5 - Bugfix and enhancements release for public review. API may change.

* [#122] API change: remove field CommandLine.ansi (enhancement)
* [#123] API change: make public Arity fields final (enhancement)
* [#124] API change: make Help fields optionFields and positionalParameterFields final and unmodifiable (enhancement)
* [#118] BumpVersion gradle task scrambles chars in manual (bug)
* [#119] Add gradle task to publish to local folder (enhancement)

# <a name="0.9.4"></a> 0.9.4 - Bugfix release for public review. API may change.

* [#114] Replace ISO-8613-3 "true colors" with more widely supported 256-color palette (enhancement)
* [#113] Fix javadoc warnings (doc enhancement)
* [#117] The build should work for anyone checking out the project (bug)
* [#112] Improve (shorten) user manual (doc enhancement)
* [#105] Automate publishing to JCentral & Maven Central

# <a name="0.9.3"></a> 0.9.3 - Bugfix release for public review. API may change.

* [#90] Automate release
* [#111] Improve picocli.Demo (enhancement)
* [#110] Fix javadoc for `parse(String...)` return value (doc enhancement)
* [#108] Improve user manual (doc enhancement)
* [#109] `run` convenience method should accept PrintStream (enhancement)

# <a name="0.9.2"></a> 0.9.2 - Bugfix release for public review. API may change.

* [#106] MissingParameterException not thrown for missing mandatory @Parameters when options are specified
* [#104] Investigate why colors don't show by default on Cygwin

# <a name="0.9.1"></a> 0.9.1 - Bugfix release for public review. API may change.

* [#103] Replace javadoc occurences of ASCII with ANSI.  (doc bug)
* [#102] Move ColorScheme inside Ansi class  (enhancement question wontfix)
* [#101] Cosmetics: indent `Default: <value>` by 2 spaces  (enhancement)
* [#100] Improve error message for DuplicateOptionAnnotationsException  (enhancement)
* [#99] MissingRequiredParams error shows optional indexed Parameters  (bug)
* [#98] MissingRequiredParams error shows indexed Parameters in wrong order when not declared in index order  (bug)
* [#97] Fix compiler warnings  (bug)
* [#96] Synopsis shows indexed Parameters in wrong order when subclassing for reuse (bug)
* [#95] EmptyStackException when no args are passed to object annotated with Parameters (bug)
* [#94] heading fields are not inherited when subclassing for reuse  (bug)
* [#93] Only option fields are set accessible, not parameters fields  (bug)
* [#91] Syntax highlighting in manual source blocks  (doc enhancement)

# <a name="0.9.0"></a> 0.9.0 (was 0.4.0) - User Manual and API Changes. Initial public release.

* [#89] Improve error message for missing required options and parameters  (enhancement)
* [#88] Code cleanup  (enhancement)
* [#87] Add `CommandLine.usage` methods with a ColorScheme parameter  (enhancement)
* [#86] Work around issue on Windows (Jansi?) where style OFF has no effect  (bug)
* [#85] Javadoc for Ansi classes  (doc)
* [#84] System property to let end users set color scheme  (enhancement)
* [#81] Improve README  (doc enhancement)
* [#80] Support customizable Ansi color scheme  (enhancement)
* [#79] Approximate `istty()` by checking `System.console() != null`  (enhancement)
* [#78] Add method CommandLine.setUsageWidth(int)  (enhancement wontfix)
* [#77] Replace PicoCLI in javadoc with picocli  (doc enhancement)
* [#76] @Parameters javadoc is out of date  (bug doc)
* [#75] The default value for the `showDefaultValues` attribute should be `false`  (bug)
* [#74] rename attribute `valueLabel` to `paramLabel`  (enhancement)
* [#73] Remove @Parameters synopsis attribute  enhancement)
* [#72] numeric parameter conversion should parse as decimal  (bug enhancement)
* [#71] Allow multiple values for an option -pA,B,C or -q="A B C"  (enhancement)
* [#66] Support ansi coloring  (doc enhancement)
* [#65] Consider removing the `required` Option attribute  (enhancement question wontfix)
* [#64] Test that boolean options with arity=1 throw MissingParameterException when no value exists (not ParameterException)  (bug QA)
* [#35] Allow users to express arity as a range: 0..* or 1..3 (remove "varargs" attribute)  (enhancement)
* [#30] Test & update manual for exceptions thrown from custom type converters  (doc QA)
* [#26] Ergonomic API - convenience method to parse & run an app  (duplicate enhancement)
* [#12] Create comparison feature table with prior art  (doc)
* [#11] Write user manual  (doc in-progress)
* [#6] Array field values should be preserved (like Collections) and new values appended  (enhancement)
* [#4] Should @Option and @Parameters have listConverter attribute instead of elementType?  (enhancement question wontfix)


# <a name="0.3.0"></a> 0.3.0 - Customizable Usage Help

* [#69] Improve TextTable API  (enhancement question)
* [#63] Unify @Option and @Parameters annotations  (enhancement wontfix)
* [#59] Support declarative API for customizing usage help message  (enhancement wontfix)
* [#58] Add unit tests for ShortestFirst comparator  (QA)
* [#57] Consider using @Usage separator for parsing as well as for usage help  (enhancement)
* [#56] Add unit tests for customizable option parameter name and positional parameter name  (QA)
* [#55] Add unit tests for detailed Usage line  (QA)
* [#54] Add unit tests for DefaultLayout  (QA)
* [#53] Add unit tests for DefaultParameterRenderer  (QA)
* [#52] Add unit tests for DefaultOptionRenderer  (QA)
* [#51] Add unit tests for MinimalOptionRenderer  (QA)
* [#50] Add unit tests for Arity  (QA)
* [#49] Detailed usage header should cluster boolean options  (enhancement)
* [#48] Show positional parameters details in TextTable similar to option details  (enhancement)
* [#47] Reduce API surface for usage Help  (enhancement)
* [#44] Support detailed Usage line instead of generic Usage \<main> \[option] [parameters]  (enhancement)
* [#43] Generated help message should show parameter default value (except for boolean fields)  (enhancement)
* [#42] Show option parameter in generated help (use field name or field type?)  (enhancement)
* [#41] Required options should be visually distinct from optional options in usage help details  (enhancement)
* [#40] Test SortByShortestOptionName  (QA)
* [#39] Test that first declared option is selected by ShortestFirst comparator if both equally short  (QA)
* [#38] Test DefaultRenderer chooses shortest option name in left-most field  (QA)
* [#37] Consider returning a list of Points from TextTable::putValue  (enhancement wontfix)
* [#36] javadoc ILayout, IRenderer, DefaultLayout, DefaultRenderer  (doc)
* [#34] Usage should not show options if no options exist  (enhancement)
* [#32] Support customizable user help format.  (enhancement)
* [#31] Add test for recognizing clustered short option when parsing varargs array  (bug QA)
* [#27] Support git-like commands  (enhancement)
* [#8] Add positional @Parameter annotation  (enhancement)
* [#7] Implement online usage help  (enhancement)
* [#5] Rename `description` attribute to `helpText` or `usage`  (enhancement wontfix)


# <a name="0.2.0"></a> 0.2.0 - Vararg Support

* [#25] Use Integer.decode(String) rather than Integer.parseInt  (enhancement)
* [#23] @Option should not greedily consume args if varargs=false  (bug)


# <a name="0.1.0"></a> 0.1.0 - Basic Option and Parameter Parsing

* [#20] add test where option name is "-p", give it input "-pa-p"  (QA)
* [#19] Improve error message for type conversion: include field name (and option name?)  (enhancement)
* [#18] test superclass bean and child class bean where child class field shadows super class and have different annotation Option name  (QA)
* [#17] Test superclass bean and child class bean where child class field shadows super class and have same annotation Option name  (invalid QA)
* [#16] Test arity > 1 for single-value fields (int, File, ...)  (QA)
* [#13] Test for enum type conversation  (QA)
* [#3] Interpreter should set helpRequested=false before parse()  (bug)
* [#2] Test that separators other than '=' can be configured  (QA)
* [#1] Test with other option prefixes than '-'  (QA)
