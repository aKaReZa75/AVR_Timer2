# **TIMER2 Outputs — ATMEGA328**

This document explains Compare Output Modes and how they affect the OC2A and OC2B pins in different timer modes, with practical configuration examples for normal, inverting, and non-inverting outputs.

---

> [!TIP]  
The x can be either A or B, depending on whether you are configuring Channel A (OC2A) or Channel B (OC2B).

---

### **Compare Output Mode, non-PWM Mode**

| COM2x1 | COM2x0 | Description                                              |
|--------|--------|----------------------------------------------------------|
| 0      | 0      | Normal port operation, OC2x disconnected.                |
| 0      | 1      | Toggle OC2x on Compare Match.                            |
| 1      | 0      | Clear OC2x on Compare Match.                             |
| 1      | 1      | Set OC2x on Compare Match.                               |

---

### **Compare Output Mode, Fast PWM Mode**

| COM2x1 | COM2x0 | Description                                                                 |
|--------|--------|-----------------------------------------------------------------------------|
| 0      | 0      | Normal port operation, OC2x disconnected.                                   |
| 0      | 1      | WGM22 = 0: Normal Port Operation, OC2x Disconnected. WGM22 = 1: Toggle OC2x |
| 1      | 0      | Clear OC2x on Compare Match, set OC2x at BOTTOM (non-inverting mode).       |
| 1      | 1      | Set OC2x on Compare Match, clear OC2x at BOTTOM (inverting mode).           |

> [!NOTE]  
A special case occurs when OCR2x equals TOP and COM2x1 is set. In this case, the compare match is ignored, but the set or clear is done at BOTTOM.

---

### **Compare Output Mode, Phase Correct PWM Mode**

| COM2x1 | COM2x0 | Description                                                                 |
|--------|--------|-----------------------------------------------------------------------------|
| 0      | 0      | Normal port operation, OC2x disconnected.                                   |
| 0      | 1      | WGM22 = 0: Normal Port Operation, OC2x Disconnected. WGM22 = 1: Toggle OC2x |
| 1      | 0      | Clear OC2x on Compare Match when up-counting. Set OC2x when down-counting.  |
| 1      | 1      | Set OC2x on Compare Match when up-counting. Clear OC2x when down-counting.  |

> [!NOTE]  
A special case occurs when OCR2x equals TOP and COM2x1 is set. In this case, the compare match is ignored, but the set or clear is done at TOP.

---

#### Compare Output Mode — Normal Operation

- Timer used for internal timing only (no waveform on output pins)  
- Suitable for Normal Mode, CTC, or PWM when no output pin is needed

```c
/* Normal Operation (OC2A disconnected) */
bitClear(TCCR2A, COM2A1);
bitClear(TCCR2A, COM2A0);

/* Normal Operation (OC2B disconnected) */
bitClear(TCCR2A, COM2B1);
bitClear(TCCR2A, COM2B0);
```

---

#### Compare Output Mode — Toggle

- Generate square waves for frequency testing or simple waveform generation  
- Works in CTC mode  
- In Fast PWM or Phase Correct PWM, only works when WGM22 = 1

```c
/* Toggle OC2A on Compare Match */
bitClear(TCCR2A, COM2A1);
bitSet(TCCR2A, COM2A0);

/* Toggle OC2B on Compare Match */
bitClear(TCCR2A, COM2B1);
bitSet(TCCR2A, COM2B0);
```

---

#### Compare Output Mode — Non-Inverting

- Standard PWM signal generation  
- Output is HIGH at BOTTOM, LOW when TCNT2 = OCR2x  
- Used to drive motors, LEDs, or other analog devices using PWM  
- Very common for controlling power (e.g., speed, brightness)

```c
/* Non-Inverting PWM Output (Clear OC2A on Compare Match) */
bitSet(TCCR2A, COM2A1);
bitClear(TCCR2A, COM2A0);

/* Non-Inverting PWM Output (Clear OC2B on Compare Match) */
bitSet(TCCR2A, COM2B1);
bitClear(TCCR2A, COM2B0);
```

---

#### Compare Output Mode — Inverting

- Inverted PWM generation  
- Output is LOW at BOTTOM, HIGH when TCNT2 = OCR2x  
- Used when complementary waveforms are needed (e.g., H-bridge drivers, special dimming effects)

```c
/* Inverting PWM Output (Set OC2A on Compare Match) */
bitSet(TCCR2A, COM2A1);
bitSet(TCCR2A, COM2A0);

/* Inverting PWM Output (Set OC2B on Compare Match) */
bitSet(TCCR2A, COM2B1);
bitSet(TCCR2A, COM2B0);
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
