# example usage
Use window API to loop through process to find target application process ID
then gain handle to the process by calling OpenProcess and pass in process id
Once you have a handle you can use read and write process memory to do your hack
Kernel anti cheat do not let you read and write memory

# Process
OS will give you some useful information including but not limited to `process identifier` or PID the location of execution file that the process coming from(image path)
a process get a allocated memory
# thread
Each process start with main thread