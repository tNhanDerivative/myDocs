
# Entry
structure of the log
## Entry builder
`EntryBuilder` use Builder pattern where each function receive an option and return a pointer
`EntryBuilder` inherited from struct Entry. Which mean it owns an `Entry` object, and can pass as Entry parameter.
`EntryBuilder` is also responsible to submit Entry to Channel

# Channel
When the structure of the Log (Entry) are build we will submit it to a Logging Chanel
Default channel has 
	- an array of Policy 
	- an array of Driver

## Policy
Use the to form a Logger object based on the Entry (structure of the Log)
## Driver
Have Formatters to format the logging output into the output you choose.
It's nicer if I have time to design it so we can add a custom formatter easily
## iChannel

