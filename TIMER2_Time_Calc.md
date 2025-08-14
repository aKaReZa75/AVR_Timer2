# **TIMER2 Time Calculation — ATMEGA328**

This document helps calculate the overflow time and compare match intervals in **Normal** and **CTC** modes based on prescaler and system clock, with practical examples and formulas.

---

## **Normal Mode**

The time period for **Timer/Counter 2** to overflow or for a compare match can be calculated as:

```
T = (N * 256) / f_clk
```

Where:
- `N` is the prescaler value.
- `256` is the maximum value the timer can count to before overflowing (in Normal mode).
- `f_clk` is the system clock frequency.

### Example Calculation for Overflow Time

For a **16 MHz** clock and a **prescaler of 64**, the time period for a **Timer/Counter 2** overflow would be:

```
T = (64 * 256) / 16,000,000 = 16,384 / 16,000,000 = 1.024 ms
```

| Prescaler | Time (ms) |
|-----------|-----------|
| 1         | 0.016     |
| 8         | 0.128     |
| 32        | 0.512     |
| 64        | 1.024     |
| 128       | 2.048     |
| 256       | 4.096     |
| 1024      | 16.384    |

This table shows the time period for a **Timer/Counter 2** overflow for different prescaler values, assuming a **16 MHz** clock and a **max count of 256** before overflow.

---

## **Time Calculation with TCNT2**

To reach specific time intervals in Normal mode, you can preload the **TCNT2** register with a value less than 256. The time until overflow is then calculated as:

```
T = ((256 - TCNT2) * N) / f_clk
```

Where:
- `TCNT2` is the initial value in the TCNT2 register.
- `N` is the prescaler value.
- `f_clk` is the system clock frequency.

### Example Calculation Using TCNT2

To generate an interrupt **1 millisecond** after the timer starts, using a **prescaler of 64** and a **16 MHz** clock:

We want:
```
T = 1 ms = 0.001 s
```

Using the formula:
```
0.001 = ((256 - TCNT2) * 64) / 16,000,000
```

Solving:
```
(256 - TCNT2) = 0.001 * 16,000,000 / 64 = 250
TCNT2 = 256 - 250 = 6
```

So, to achieve a 1 ms delay, set `TCNT2 = 6`.

> [!IMPORTANT]  
You must re-set the TCNT2 register to the calculated value (6 in this case) at the end of the overflow interrupt. This ensures consistent timing.

```c
ISR(TIMER2_OVF_vect)
{
    TCNT2 = 6;
}
```

---

## **CTC Mode**

In **CTC (Clear Timer on Compare Match)** mode, the timer resets when it matches the value in the **OCR2A** register. The time period is calculated as:

```
T = (N * (OCR2A + 1)) / f_clk
```

Where:
- `OCR2A` is the compare match value.
- `N` is the prescaler value.
- `f_clk` is the system clock frequency.

### Example Calculation in CTC Mode

For a **16 MHz** clock, a **prescaler of 64**, and `OCR2A = 249`:

```
T = (64 * (249 + 1)) / 16,000,000 = (64 * 250) / 16,000,000 = 16,000 / 16,000,000 = 1 ms
```

In CTC mode, the timer counts from 0 to `OCR2A` before resetting, which is ideal for generating precise periodic interrupts.

> [!TIP]  
In Normal mode, use the Overflow interrupt (OVF).  
In CTC mode, use the Compare Match interrupt triggered when `TCNT2 == OCR2A`.



# 🌟 Support Me
If you found this repository useful:
- Subscribe to my [YouTube Channel](https://www.youtube.com/@aKaReZa75).
- Share this repository with others.
- Give this repository and my other repositories a star.
- Follow my [GitHub account](https://github.com/aKaReZa75).

# ✉️ Contact Me
Feel free to reach out to me through any of the following platforms:
- 📧 [Email: aKaReZa75@gmail.com](mailto:aKaReZa75@gmail.com)
- 🎥 [YouTube: @aKaReZa75](https://www.youtube.com/@aKaReZa75)
- 💼 [LinkedIn: @akareza75](https://www.linkedin.com/in/akareza75)
