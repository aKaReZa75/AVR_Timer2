# **TIMER2 Other Registers — ATMEGA328**

This file provides a detailed description of Timer2 data and compare registers: `TCNT2`, `OCR2A`, and `OCR2B`. It explains how each register works, including bit-level information and their roles in compare match operations for Channel A and B.

---

## **TCNT2 (Timer/Counter Register)**

| Bit | Description |
|-----|-------------|
| 7–0 | TCNT2 (Timer/Counter Value) |

- **TCNT2**: The current value of the Timer/Counter. It increments with each timer tick based on the selected prescaler and clock source.
- Used in all Timer2 modes: Normal, CTC, Fast PWM, and Phase Correct PWM.

```c
/* Read current Timer2 count */
uint8_t count = TCNT2;

/* Reset Timer2 counter */
TCNT2 = 0x00;
```

---

## **OCR2A (Output Compare Register A)**

| Bit | Description |
|-----|-------------|
| 7–0 | OCR2A (Output Compare A Value) |

- **OCR2A**: Holds the value for the output compare match for Channel A.
- In CTC mode, Timer2 resets when `TCNT2 == OCR2A`.
- In PWM modes, it defines the duty cycle for OC2A.

```c
/* Set compare match value for Channel A */
OCR2A = 192;  // Example value for ~75% duty cycle
```

---

## **OCR2B (Output Compare Register B)**

| Bit | Description |
|-----|-------------|
| 7–0 | OCR2B (Output Compare B Value) |

- **OCR2B**: Holds the value for the output compare match for Channel B.
- Used similarly to OCR2A but for Channel B.
- In PWM modes, it defines the duty cycle for OC2B.

```c
/* Set compare match value for Channel B */
OCR2B = 64;  // Example value for ~25% duty cycle
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
