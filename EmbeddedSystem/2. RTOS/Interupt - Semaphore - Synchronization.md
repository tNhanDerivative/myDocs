
[Interupt and Semaphore](https://www.youtube.com/watch?v=IrDcBZX0AdY)

#
# Interupt maximum latency

CPU has a special built-in hardware to read interrupt status AFTER EVERY INSTRUCTION.
And latency cause by INTERRUPT ENTRY instruction which is one of the longest instruction.
![interrupt routines](Interrupt_Routines.png)


Latency also cause by RTOS disable interrupt TO PREVENT RACE CONDITION show up here in a blackbox
![](RTOS_disable_interrupt.png)


- Kernel aware interrupt have to be set it's priority level UNDER "Kernel aware" level
-  Kernel aware interrupt means: Interrupt that call RTOS API