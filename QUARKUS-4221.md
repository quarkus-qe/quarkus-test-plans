# QUARKUS-4221 Productization of Picocli extension

JIRA: https://issues.redhat.com/browse/QUARKUS-4221

PR: https://github.com/quarkus-qe/quarkus-test-suite/pull/2003
## Scope of the testing
### What to test

The test coverage will be verified with JVM mode and native mode on Baremetal.

Openshift excluded due to the impracticality of running command-line applications with CLI arguments in these environment.

### Scenarios
* Verify command line application with some annotations such as: `@CommandLine.Command` / `@CommandLine.Option` 

  - **Purpose:** Ensure that the greeting command works correctly and produces the expected output when provided with the correct arguments.
  - **Configuration:** Set up a @QuarkusApplication RestService to run the application in a controlled environment.
  - **Test:** Run the command and verify that the output matches the expected greeting message.
  

* Verify the usage of proxy scope with @ApplicationScope in beans to check 'Picocli will not be able to set field values in such beans' according 
the warning in the guide: [simple-command-line-application](https://quarkus.io/guides/picocli#simple-command-line-application).

  - **Purpose:** To test if Picocli can handle beans annotated with `@ApplicationScoped`. 
  According to Picocli documentation, `@ApplicationScoped` beans should not be used with Picocli commands
  because Picocli cannot set field values in such beans due to their proxy nature. 
  This scenario will confirm if the use of @ApplicationScoped with Picocli produces an error or warning.
  - **Configuration:** Create and configure a Picocli command (AgeCommand) annotated with `@ApplicationScoped`. 
  Set up a REST service to execute this command with an age argument.
  - **Test:**  Run the command and verify that the logs contain an error or
  warning message indicating that `@ApplicationScoped` is not compatible with Picocli commands,
  as Picocli cannot set field values in such beans.
  
  
* Verify Handling of Blank and Invalid Arguments

  - **Purpose:** Test the application's response to missing or incorrect arguments for the greeting command.
  - **Configuration:**  Set up REST services with configurations that include blank or invalid arguments for the greeting command.
  - **Test:**  Run the greeting command with these arguments and verify that the application produces appropriate error messages.

  
* Verify command line application with multiple Commands
  - **Purpose:**  Test the behavior of a command-line application with multiple commands using `@TopCommand` and ensure it functions correctly.
  Additionally, verify that if multiple `@CommandLine.Command` annotations are used without `@TopCommand`, it should fail.
  - **Configuration:**
    Set up a Picocli command (EntryCommand) annotated with `@TopCommand`, which includes multiple subcommands (e.g., GreetingCommand, AgeCommand).
    Set up another scenario where multiple `@CommandLine.Command` annotations are present but without the `@TopCommand`.
  - **Test:**
    With `@TopCommand`: Run the command and verify that both commands (e.g., greeting and age) execute successfully as subcommands under EntryCommand.
    Without `@TopCommand`: Run the commands without `@TopCommand` and verify that the application fails to recognize the commands, leading to an error
  indicating that the top command is missing.
  
  
* Verify the functionality of PicocliCommandLineFactory in order to customize Picocli CommandLine instance.
  - **Purpose:**  Test the functionality of PicocliCommandLineFactory to ensure that CommandLine instances are properly customized and that the application 
  behaves as expected with a customized command-line parser.
  - **Configuration:**
    Set up a PicocliCommandLineFactory to produce a customized CommandLine instance.
    Ensure that the factory configures the command with a unique behavior, such as a custom command name or argument parsing logic.
  - **Test:**
    Run the command that relies on the customized CommandLine instance produced by PicocliCommandLineFactory.
    Verify that the command behaves according to the custom configuration (e.g., a specific output or argument handling).
    Ensure that the command output matches the expected customized response (e.g., "CustomizedName").
  
  
* Verify conditional inclusion of commands based on build-time profiles using @IfBuildProfile

  - **Purpose:** Ensure that commands are included or excluded based on the active build-time profile (dev or prod), and that the application behaves correctly when built and run with each profile.
  - **Configuration:**
    Create a configuration class with methods producing @TopCommand beans for dev and prod profiles using `@IfBuildProfile`.
    Implement EntryCommand for the dev profile and OtherEntryCommand for the prod profile.
    Use `@QuarkusApplication` and `RestService` to run the application.
  - **Test:**
     With prod profile:
    Build the application with `mvn clean verify -Dquarkus.profile=prod`.
    Enable the test class (e.g., remove @Disabled annotation).
    Run the application and verify that OtherEntryCommand is available and functions correctly.
    Note: The test for the prod profile is disabled by default because it requires building the application with the prod profile.


### Impact on testsuites and testing automation:
   New tests will be created in a new module quarkus-picocli.

### Impact on resources:
  After the new test classes introduced in quarkus-picocli on JVM  baremetal will be increased 6.381 seconds,
  and on Native baremetal 06:43 min.

## Getting familiar with the feature

Following actions were taken to ensure familiarity:
- Experiment with Quarkus Picocli extension functionalities
- Take a look and understand the Quarkus Picocli [Command Mode with Picocli](https://quarkus.io/guides/picocli) related to.
- Ensure documentation provides clear explanation on configuration options.
- Ensure good user experience and simplicity of use.

## References
- [Command Mode with Picocli](https://quarkus.io/guides/picocli)
- [Command Mode Applications ](https://quarkus.io/guides/command-mode-reference)

## Contact
- Tester: Jose Carranza  <jcarranz@redhat.com>
