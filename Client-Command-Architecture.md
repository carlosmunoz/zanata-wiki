The Command-Line Client and Maven plugin invoke the same set of commands, acting as wrappers to provide different methods of invocation and argument handling.

`ZanataCommand` is the parent interface of all command types used in Zanata clients.

`ConfigurableCommand` implements `ZanataCommand` and has a `ConfigurableOptions` type parameter. All command types below this in the hierarchy have a `*Command` and a `*Options`, e.g. `PutProjectCommand extends ConfigurableCommand<PutProjectOptions>`.

Commands are concrete implementations, whereas Options are interfaces that have different implementations for different clients. For each available client command there is usually 1 Command class, 1 Options interface and 2 Options implementations. e.g. `PutProjectCommand` (command implementation), `PutProjectOptions` (options interface), `PutProjectOptionsImpl` (options implementation used by Command-Line Client) and `PutProjectMojo` (options implementation used by Maven Plugin).