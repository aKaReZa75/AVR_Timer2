# **TIMER2 Interrupts — ATMEGA328**

This file covers all interrupt-related features of Timer2 such as overflow and compare match A/B interrupts. It explains how to enable and clear them properly using `TIMSK2` and `TIFR2` registers.

---

## **TIFR2 (Timer/Counter Interrupt Flag Register)**

| Bit | Name  | Description                             |
|-----|-------|-----------------------------------------|
| 7–3 | —     | Reserved                                |
| 2   | OCF2B | Output Compare Match Flag for Channel B |
| 1   | OCF2A | Output Compare Match Flag for Channel A |
| 0   | TOV2  | Timer/Counter Overflow Flag             |

- **OCF2A / OCF2B**:  
  Set when `TCNT2` matches `OCR2A` or `OCR2B`.

- **TOV2**:  
  Set when Timer2 overflows (from `0xFF` to `0x00`). Must be cleared by writing logic **1**.

> [!NOTE]  
> Before enabling an interrupt, you should manually clear the corresponding interrupt flag in `TIFR2`.  
> If the flag is already set, the ISR will be triggered immediately after enabling global interrupts, even if no new event has occurred.

> [!TIP]  
> These flags must be cleared by writing a logic **1** to them. Writing 0 has no effect.  
> Use the macro `intFlag_clear(Reg, Bit)` to efficiently clear interrupt flags.  
> Example: `intFlag_clear(TIFR2, TOV2);` clears the Timer2 overflow flag.

---

## **TIMSK2 (Timer/Counter Interrupt Mask Register)**

| Bit | Name   | Description                               |
|-----|--------|-------------------------------------------|
| 7–3 | —      | Reserved                                  |
| 2   | OCIE2B | Output Compare Match Interrupt Enable (B) |
| 1   | OCIE2A | Output Compare Match Interrupt Enable (A) |
| 0   | TOIE2  | Timer Overflow Interrupt Enable           |

- **OCIE2B**: Enable interrupts on output compare matches for Channel B.  
- **OCIE2A**: Enable interrupts on output compare matches for Channel A.  
- **TOIE2**: Enable Timer/Counter overflow interrupt.

> [!CAUTION]  
> Always ensure that global interrupts are enabled using the `sei()` function.  
> Without enabling global interrupts, the microcontroller will not respond to any interrupt triggers.

You can use the following macros to control global interrupt behavior:

- `globalInt_Enable`: Equivalent to `sei()`, enables global interrupts.  
- `globalInt_Disable`: Equivalent to `cli()`, disables global interrupts.

---

### Timer2 Overflow Interrupt

```c
/* ISR for Timer2 Overflow */
ISR(TIMER2_OVF_vect)
{
    /* Code executed on Timer2 overflow */
}

/* Enable Timer2 Overflow interrupt */
bitSet(TIMSK2, TOIE2);

/* Clear the overflow flag */
intFlag_clear(TIFR2, TOV2);
```

---

### Timer2 Compare Match A Interrupt

```c
/* ISR for Compare Match A */
ISR(TIMER2_COMPA_vect)
{
    /* Code executed when TCNT2 == OCR2A */
}

/* Enable Compare Match A interrupt */
bitSet(TIMSK2, OCIE2A);

/* Clear compare match A flag */
intFlag_clear(TIFR2, OCF2A);
```

---

### Timer2 Compare Match B Interrupt

```c
/* ISR for Compare Match B */
ISR(TIMER2_COMPB_vect)
{
    /* Code executed when TCNT2 == OCR2B */
}

/* Enable Compare Match B interrupt */
bitSet(TIMSK2, OCIE2B);

/* Clear compare match B flag */
intFlag_clear(TIFR2, OCF2B);
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
