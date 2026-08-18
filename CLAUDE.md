# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository overview

This repository contains a single static HTML file, [booking.html](booking.html): a Thai-language barbershop
booking demo ("The Gentleman's Cut"). There is no build system, package manager, server, or test suite —
the file is opened directly in a browser.

## Running

Open the file directly in a browser (no server or build step required):

```
start booking.html
```

## Architecture

Everything lives in the one file: Tailwind CSS is loaded via the CDN `<script>` tag in `<head>`, and all
behavior is vanilla JS in a single `<script>` block at the end of `<body>`. There is no backend — all data
(dates, time slots) is generated client-side and no data is persisted or sent anywhere.

The page is a 3-step wizard implemented as three `<section id="panel-1|2|3">` elements toggled via the
`hidden` class by `goToStep(n)`, rather than separate pages:

1. **panel-1** — pick a date (next 7 days, generated in `generateDates()`) and a time slot
   (`generateTimeSlots()`, with a few slots hardcoded as unavailable).
2. **panel-2** — name/phone form with inline validation (`handleStep2Next()`).
3. **panel-3** — booking summary (`renderSummary()`) and a confirm action (`confirmBooking()`) that
   generates a mock booking reference and swaps in a success view; `resetBooking()` returns to step 1.

All wizard state (selected date/time, name, phone) is held in the single `state` object near the top of
the script — there's no separate state management layer.
