# LedBar.c

Calculate how many led need to turn on
Calculate how much wattage need to use
turn off the led > n_on

```cpp
uint32_t LedBar_setPercent(uint8_t const percent) {
    uint8_t const n_on = (uint8_t)((percent * MAX_LED) / 100U);

    //! @pre percent must not exceed 100
    Q_REQUIRE_ID(100, n_on <= MAX_LED);

    uint32_t p = 0U; // power draw in [uW]
    uint8_t i;
    for (i = 0U; i < n_on; ++i) {
        p += Led_on(i);
    }
    for (i = n_on; i < MAX_LED; ++i) {
        Led_off(i);
    }
    return p;
}
```