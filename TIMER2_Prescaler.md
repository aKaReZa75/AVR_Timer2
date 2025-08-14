# **TIMER2 Prescaler — ATMEGA328**

This document explains how to configure the clock source and prescaler for Timer2, including detailed descriptions of all clock select bits and their effects on timer resolution and overflow frequency.

---

## **TCCR2B (Timer/Counter Control Register B)**

| Bit | Description |
|-----|-------------|
| 7   | FOC2A (Force Output Compare A) |
| 6   | FOC2B (Force Output Compare B) |
| 5   | Reserved |
| 4   | Reserved |
| 3   | WGM22 (Waveform Generation Mode) |
| 2   | CS22 (Clock Select) |
| 1   | CS21 (Clock Select) |
| 0   | CS20 (Clock Select) |

> [!NOTE]  
> WGM22 combines with WGM21 and WGM20 from TCCR2A to define the waveform generation mode.

---

### **FOC2A / FOC2B (Force Output Compare)**

Forces an immediate Compare Match on channel A or B. Only used in non-PWM modes.

```c
/* Force Compare Match on Channel A */
bitSet(TCCR2B, FOC2A);

/* Force Compare Match on Channel B */
bitSet(TCCR2B, FOC2B);
```

Useful for software-triggered output compare actions.

---

### **Clock Select Bit Description**

These bits determine the clock source and prescaler for Timer2.

| CS22 | CS21 | CS20 | Description                                           |
|------|------|------|-------------------------------------------------------|
| 0    | 0    | 0    | No clock source (Timer/Counter stopped)              |
| 0    | 0    | 1    | clk_I/O (No prescaling)                              |
| 0    | 1    | 0    | clk_I/O / 8 (From prescaler)                         |
| 0    | 1    | 1    | clk_I/O / 32 (From prescaler)                        |
| 1    | 0    | 0    | clk_I/O / 64 (From prescaler)                        |
| 1    | 0    | 1    | clk_I/O / 128 (From prescaler)                       |
| 1    | 1    | 0    | clk_I/O / 256 (From prescaler)                       |
| 1    | 1    | 1    | clk_I/O / 1024 (From prescaler)                      |

> [!TIP]  
> Timer2 supports asynchronous operation using an external 32.768 kHz crystal. This allows independent timing, ideal for real-time clock applications.

---

#### No Clock Source

```c
/* Stop Timer2 (No clock source) */
bitClear(TCCR2B, CS20);
bitClear(TCCR2B, CS21);
bitClear(TCCR2B, CS22);
```

- Timer completely disabled  
- Useful for power saving or timer deactivation

---

#### Prescaler = 1

```c
/* Set Prescaler = 1 => Timer clock = 16MHz */
bitSet(TCCR2B, CS20);
bitClear(TCCR2B, CS21);
bitClear(TCCR2B, CS22);
```

---

#### Prescaler = 8

```c
/* Set Prescaler = 8 => Timer clock = 2MHz */
bitClear(TCCR2B, CS20);
bitSet(TCCR2B, CS21);
bitClear(TCCR2B, CS22);
```

---

#### Prescaler = 32

```c
/* Set Prescaler = 32 => Timer clock = 500kHz */
bitSet(TCCR2B, CS20);
bitSet(TCCR2B, CS21);
bitClear(TCCR2B, CS22);
```

---

#### Prescaler = 64

```c
/* Set Prescaler = 64 => Timer clock = 250kHz */
bitClear(TCCR2B, CS20);
bitClear(TCCR2B, CS21);
bitSet(TCCR2B, CS22);
```

---

#### Prescaler = 128

```c
/* Set Prescaler = 128 => Timer clock = 125kHz */
bitSet(TCCR2B, CS20);
bitClear(TCCR2B, CS21);
bitSet(TCCR2B, CS22);
```

---

#### Prescaler = 256

```c
/* Set Prescaler = 256 => Timer clock = 62.5kHz */
bitClear(TCCR2B, CS20);
bitSet(TCCR2B, CS21);
bitSet(TCCR2B, CS22);
```

---

#### Prescaler = 1024

```c
/* Set Prescaler = 1024 => Timer clock = 15.625kHz */
bitSet(TCCR2B, CS20);
bitSet(TCCR2B, CS21);
bitSet(TCCR2B, CS22);
```

---

### Force Compare Match (FOC2A / FOC2B)

- Only valid in non-PWM modes (e.g., Normal, CTC)
- Manually trigger output compare
- Useful for testing or immediate ISR activation

```c
/* Force Compare Match on OC2A */
bitSet(TCCR2B, FOC2A);

/* Force Compare Match on OC2B */
bitSet(TCCR2B, FOC2B);
```



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
