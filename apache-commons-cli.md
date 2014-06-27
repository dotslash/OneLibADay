#Apache Commons CLI
Apache commons cli provides nice utils for parsing parameters passed with java. This is quick tutorial of the same. The important components of the library are **Options** and **Parser**. 
Options object is required for defining the command line arguments and parser provides tools to parse and use the parameters which follow the format defined in options.

###Options

Options can be seen as a list of Option objects. Each Option defines a command line option. 
Example: in ls -l -h  "-l" and "-h" is an Option.

###Defining an Option

Even though Option has a couple of constructors, it is better to create options using a builder for more readablity.
The important fields in an option are 
* argument : a sensible name for the option
* description 
* longOpt : the commandling long name for the option (example : --help)
* opt : the commandline short name for the option (example : -h)
* seperator : useful for key value type options
* isRequired 
```java
static Option optionWithOneArgument() {
    return OptionBuilder.withArgName("module")
            .hasArg().withDescription("help for module")
            .withLongOpt("help")
            .create('h');
}
```
```java
static Option optionWithKeyValueArgument() {
    return OptionBuilder.withArgName( "property=value" )
            .hasArgs()
            .withValueSeparator()
            .withDescription( "use value for given property" )
            .create( "D" );
}
```
That said I believe the OptionBuilder class is messed up as all the fields and functions in it are static. So the rule is if you start playing with the builder, you must end with a create. 
From version 1.3, there is an Option.Builder which adresses this issue.

###Creating Options object
Once all the Option objects are ready they can be added into a Options object. Alternatively, an option can be directly added into Options by passing into addOption funciton, the required parameters to create one (see example).
```java
Options options = new Options();
options.addOption("a", "all", false, "do not hide entries starting with .");
Option help = optionWithOneArgument();
Option property  = optionWithKeyValueArgument();
options.addOption(help);
options.addOption(property);

```
###Getting help with an Options object
See example
```java
HelpFormatter formatter = new HelpFormatter();
formatter.printHelp( "MyProgram", options );
```
And the output will be like this. For each option it prints the short name, long name, parameter type ,description
```java
usage: MyProgram
 -a,--all              do not hide entries starting with .
 -D <property=value>   use value for given property
 -h,--help <module>    help for module
```
###Parser
Create a parser and parse the args using options into a CommandLine Object. CommandLine has functions to retrieve the values to passed to objects and to know the options present in the args
```java
CommandLineParser parser = new BasicParser();
CommandLine parse = parser.parse(options, args);

System.out.println("argument(s) passed with h : " + parse.getOptionValue("h"));
System.out.println("argument(s) passed with D : " +parse.getOptionProperties("D"));
```
###Sample Program
```java
import java.util.Arrays;
import org.apache.commons.cli.BasicParser;
import org.apache.commons.cli.CommandLine;
import org.apache.commons.cli.CommandLineParser;
import org.apache.commons.cli.HelpFormatter;
import org.apache.commons.cli.Option;
import org.apache.commons.cli.OptionBuilder;
import org.apache.commons.cli.Options;
import org.apache.commons.cli.ParseException;

public class ApacheCliUtils {
    static Option optionWithOneArgument() {
        return OptionBuilder.withArgName("module")
                .hasArg().withDescription("help for module")
                .withLongOpt("help")
                .create('h');
    }

    static Option optionWithKeyValueArgument() {
        return OptionBuilder.withArgName( "property=value" )
                .hasArgs()
                .withValueSeparator()
                .withDescription( "use value for given property" )
                .create( "D" );
    }

    public static void main(String[] args) throws ParseException {

        //create options
        Options options = new Options();
        Option help = optionWithOneArgument();
        options.addOption("a", "all", false, "do not hide entries starting with .");
        Option property  = optionWithKeyValueArgument();
        options.addOption(help);
        options.addOption(property);


        //print help info
        HelpFormatter formatter = new HelpFormatter();
        formatter.printHelp( "MyProgram", options );

        //modify the args for testing
        args = new String[] {
                "-h",
                "moduleName",
                "-D",
                "key1=val1",
                "-D",
                "key2=val2",
                "-D",
                "key3=val3"
        };


        //create a basic parser and parse the args
        CommandLineParser parser = new BasicParser();
        CommandLine parse = parser.parse(options, args);


        System.out.println("argument(s) passed with h : " + parse.getOptionValue("h"));
        System.out.println("argument(s) passed with D : " +parse.getOptionProperties("D"));

    }
}

```
Output of the program
```java
usage: MyProgram
 -a,--all              do not hide entries starting with .
 -D <property=value>   use value for given property
 -h,--help <module>    help for module
argument(s) passed with h : moduleName
argument(s) passed with D : {key3=val3, key2=val2, key1=val1}
```
