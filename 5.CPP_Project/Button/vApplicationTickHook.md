
# Explain code
This algorithm uses a simple state machine to debounce the buttons. It works by maintaining a history of the button states and updating the `depressed` state based on this history.

```cpp
static struct ButtonsDebouncing {
    uint32_t depressed;
    uint32_t previous;
} buttons = { 0U, 0U };
```
This structure is used to keep track of the state of the buttons. It contains two fields:

- `depressed`: A bitmask representing the buttons that are currently depressed (pressed).
- `previous`: A bitmask representing the state of the buttons in the previous tick.

### Reading Button States (Cho code mình vào)

```cpp
current = ~GPIO->P[PB_PORT].DIN; /* read PB0 and BP1 */
```


This line reads the current state of the buttons connected to the PB port. The `~` operator inverts the bits, assuming that a button press results in a logic low (0) and a button release results in a logic high (1).

### Debouncing Algorithm

The debouncing algorithm is implemented in the following lines:
```cpp
tmp = buttons.depressed; /* save the debounced depressed buttons */
buttons.depressed |= (buttons.previous & current); /* set depressed */
buttons.depressed &= (buttons.previous | current); /* clear released */
buttons.previous   = current; /* update the history */
tmp ^= buttons.depressed;     /* changed debounced depressed */

```

### Handling Button Press and Release Events

```cpp
if ((tmp & (1U << PB0_PIN)) != 0U) { /* debounced PB0 state changed? */
    if ((buttons.depressed & (1U << PB0_PIN)) != 0U) { /* PB0 depressed? */

        /* post the "button-pressed" event from ISR */
        static Event const buttonPressedEvt = {BUTTON_PRESSED_SIG};
        Active_postFromISR(AO_blinkyButton, &buttonPressedEvt,
                           &xHigherPriorityTaskWoken);
    }
    else { /* the button is released */


         /* post the "button-released" event from ISR */
         static Event const buttonReleasedEvt = {BUTTON_RELEASED_SIG};
         Active_postFromISR(AO_blinkyButton, &buttonReleasedEvt,
                             &xHigherPriorityTaskWoken);
    }
}

```

# Context switching
This line is used to perform a context switch from the ISR if necessary. It checks the `xHigherPriorityTaskWoken` variable and requests a context switch if a higher priority task has been woken up.

# full code

```cpp
void vApplicationTickHook(void) {
    BaseType_t xHigherPriorityTaskWoken = pdFALSE;

    static struct ButtonsDebouncing {
        uint32_t depressed;
        uint32_t previous;
    } buttons = { 0U, 0U };
    uint32_t current;
    uint32_t tmp;


    current = ~GPIO->P[PB_PORT].DIN; /* read PB0 and BP1 */
    tmp = buttons.depressed; /* save the debounced depressed buttons */
    buttons.depressed |= (buttons.previous & current); /* set depressed */
    buttons.depressed &= (buttons.previous | current); /* clear released */
    buttons.previous   = current; /* update the history */
    tmp ^= buttons.depressed;     /* changed debounced depressed */

    if ((tmp & (1U << PB0_PIN)) != 0U) {  /* debounced PB0 state changed? */
        if ((buttons.depressed & (1U << PB0_PIN)) != 0U) { /* PB0 depressed? */
    
            /* post the "button-pressed" event from ISR */
            static Event const buttonPressedEvt = {BUTTON_PRESSED_SIG};
            Active_postFromISR(AO_blinkyButton, &buttonPressedEvt,
                               &xHigherPriorityTaskWoken);
        }
        else { /* the button is released */
             /* post the "button-released" event from ISR */
             static Event const buttonReleasedEvt = {BUTTON_RELEASED_SIG};
             Active_postFromISR(AO_blinkyButton, &buttonReleasedEvt,
                                 &xHigherPriorityTaskWoken);
        }
    }

    /* notify FreeRTOS to perform context switch from ISR, if needed */
    portEND_SWITCHING_ISR(xHigherPriorityTaskWoken);
}
```