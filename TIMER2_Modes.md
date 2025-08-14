# **TIMER2 Modes — ATMEGA328**

This document provides a detailed overview of all waveform generation modes of **Timer2**, including **Normal**, **CTC**, **Fast PWM**, and **Phase Correct PWM**. It explains how to configure the **WGM bits** in `TCCR2A` and `TCCR2B`, with practical examples of mode switching and timing behavior.

---

## **TCCR2A (Timer/Counter Control Register A)**

| Bit | Description |
|-----|-------------|
| 7   | COM2A1 (Compare Output Mode for Channel A) |
| 6   | COM2A0 (Compare Output Mode for Channel A) |
| 5   | COM2B1 (Compare Output Mode for Channel B) |
| 4   | COM2B0 (Compare Output Mode for Channel B) |
| 3   | Reserved |
| 2   | Reserved |
| 1   | WGM21 (Waveform Generation Mode) |
| 0   | WGM20 (Waveform Generation Mode) |

> [!CAUTION]  
> The **WGM22** bit is located in the `TCCR2B` register.

---

## **Waveform Generation Modes (WGM22:WGM21:WGM20)**

These bits define the waveform generation mode of Timer2. The available configurations are:

| **Mode** | **WGM22** | **WGM21** | **WGM20** | **Mode Description**               | **TOP**   | **OCRx Update** | **TOV Flag Set** |
|----------|-----------|-----------|-----------|------------------------------------|-----------|------------------|------------------|
| 0        | 0         | 0         | 0         | Normal                             | 0xFF      | Immediate        | MAX              |
| 1        | 0         | 0         | 1         | PWM, Phase Correct                 | 0xFF      | TOP              | BOTTOM           |
| 2        | 0         | 1         | 0         | CTC                                | OCR2A     | Immediate        | MAX              |
| 3        | 0         | 1         | 1         | Fast PWM                           | 0xFF      | BOTTOM           | MAX              |
| 4        | 1         | 0         | 0         | Reserved                           | -         | -                | -                |
| 5        | 1         | 0         | 1         | PWM, Phase Correct (TOP = OCR2A)  | OCR2A     | TOP              | BOTTOM           |
| 6        | 1         | 1         | 0         | Reserved                           | -         | -                | -                |
| 7        | 1         | 1         | 1         | Fast PWM (TOP = OCR2A)            | OCR2A     | BOTTOM           | TOP              |

* **MAX** = 0xFF  
* **BOTTOM** = 0x00

> [!CAUTION]  
> Pay close attention to the **OCRx Update** column. Incorrect timing can lead to unstable PWM or missed interrupts.

---

###  Mode 0: Normal Mode

Timer counts from 0 to 255. Overflow occurs when it wraps around.

```c
/* Configure Timer2 for Normal Mode (Mode 0) */
bitClear(TCCR2A, WGM20);
bitClear(TCCR2A, WGM21);
bitClear(TCCR2B, WGM22);
```

Use case: simple delays, overflow interrupts.

---

###  Mode 1: PWM, Phase Correct (TOP = 0xFF)

Timer counts up to 255 and then down to 0, producing a symmetrical PWM waveform.

```c
/* Configure Timer2 for Phase Correct PWM (Mode 1) */
bitSet(TCCR2A, WGM20);
bitClear(TCCR2A, WGM21);
bitClear(TCCR2B, WGM22);
```

Use case: motor control, LED dimming with center-aligned PWM.

---

###  Mode 2: CTC (Clear Timer on Compare Match)

Timer resets when it matches `OCR2A`. Useful for precise timing.

```c
/* Configure Timer2 for CTC Mode (Mode 2) */
bitClear(TCCR2A, WGM20);
bitSet(TCCR2A, WGM21);
bitClear(TCCR2B, WGM22);
```

Use case: square wave generation, time base creation.

---

###  Mode 3: Fast PWM (TOP = 0xFF)

Timer counts from 0 to 255. Faster PWM with less resolution.

```c
/* Configure Timer2 for Fast PWM (Mode 3) */
bitSet(TCCR2A, WGM20);
bitSet(TCCR2A, WGM21);
bitClear(TCCR2B, WGM22);
```

Use case: high-frequency PWM for audio or fast switching.

---

### Mode 4: Reserved

> [!CAUTION]  
> This mode is reserved and should not be used.

---

###  Mode 5: PWM, Phase Correct (TOP = OCR2A)

Timer counts up and down, using `OCR2A` as TOP.

```c
/* Configure Timer2 for Phase Correct PWM (Mode 5) with TOP = OCR2A */
bitSet(TCCR2A, WGM20);
bitClear(TCCR2A, WGM21);
bitSet(TCCR2B, WGM22);
```

Use case: variable resolution PWM with flexible duty cycle.

---

### Mode 6: Reserved

> [!CAUTION]  
> This mode is reserved and should not be used.

---

###  Mode 7: Fast PWM (TOP = OCR2A)

Timer counts from 0 to `OCR2A`. Allows configurable PWM frequency.

```c
/* Configure Timer2 for Fast PWM (Mode 7) with TOP = OCR2A */
bitSet(TCCR2A, WGM20);
bitSet(TCCR2A, WGM21);
bitSet(TCCR2B, WGM22);
```

Use case: high-speed PWM with adjustable frequency and resolution.



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
