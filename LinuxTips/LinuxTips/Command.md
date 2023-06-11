# Enviroment Variable
`export` sets the environment variable on the left side of the assignment to the value on the right side of the assignment;

such environment variable is visible to the process that sets it and to all the subprocesses spawned in the same environment,
in this case to the Bash instance that sources `~/.bash_profile` and to all the subprocesses spawned in the same environment (which may include e.g. also other shells, which will in turn be able to access it)
