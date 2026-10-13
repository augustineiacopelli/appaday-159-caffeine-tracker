# Caffeine Tracker

**AppADay Day 159** | Category H, Health | Claude API: No

Live: https://augustineiacopelli.github.io/appaday-159-caffeine-tracker/
Portfolio: https://augustineiacopelli.github.io/appaday/

Log every coffee, tea, or energy drink with one tap and watch your caffeine level fade on a live decay curve. Set your half life and bedtime, see when you drop below your sleep safe threshold, how much is left at lights out, and your daily total against the 400 mg guidance. Everything saves on your device, and the app works fully offline once loaded.

## How it works

The big number is the caffeine in your blood right now. A drink does not hit your blood the instant you swallow it, so the model has caffeine pass through two absorption stages, each at a rate of 5.5 per hour, while the body eliminates it with your half life. The result is a level that starts flat, rises smoothly, peaks about an hour after a gulped drink at roughly 85 to 90 percent of the dose, and then decays. The exact blood level from 1 mg gulped x hours ago is KA squared times (e to the minus ke x, minus e to the minus KA x times (1 plus (KA minus ke) x)), divided by (KA minus ke) squared, where ke is the natural log of 2 over the half life. A drink you sip over a start to finish window feeds in evenly across that window, added up exactly with the integral of that curve rather than a rough step model. Right after you tap a drink the blood number is still near zero, and a line under it tells you how many milligrams are still absorbing. The default half life is 5 hours, adjustable from 3 to 7 in half hour steps.

Below the number is a three tile strip. Safe by shows the clock time your level falls under your sleep safe threshold, found by stepping through the rise, the peak, and the early decline minute by minute, then finishing the tail with the exact decay formula of the half life times log base 2 of the level divided by the threshold. It says tomorrow when that time lands past midnight, and it waits for a drink that is still absorbing or being sipped, since the level is still climbing then. At bedtime shows the milligrams that will be left at your next bedtime, green when it is at or under your threshold and amber when it is over. If your bedtime has already passed today, the next one is tomorrow. Today shows what you have logged since local midnight against the 400 mg guidance, with a bar that turns red once you pass it. A plain sentence under the number sums it up.

## The curve

An inline SVG draws the level from 6 hours back to 18 hours ahead, sampled every 10 minutes and joined with smooth Bezier segments so the rise and the decline read as one continuous curve. A dashed green line marks your threshold, a dotted marker shows now with a dot on the curve, and a lilac marker shows your next bedtime with the milligrams left then. Small dots on the baseline mark when each dose started, and a short bar runs from the start to the finish of any drink you sipped. The chart is built only from path, line, rect, circle, and text elements, with no libraries.

## Logging

The preset chips log a drink at the current time with one tap: Coffee 95, Espresso 64, Black tea 47, Green tea 28, Cola 34, and Energy drink 80 milligrams by default. Custom, or the Add dose button in the log, opens a form for a name, an amount from 1 to 1000 mg, the time you started, and an optional time you finished. Leave Finished blank for a drink you had all at once. A finish earlier than the start means it ran past midnight, a drink can run up to 12 hours, and a start later than now counts as yesterday. The log shows the start to finish span for sipped drinks. The dose log lists the last 48 hours, newest first, with how much of each dose is left. Tap any row to edit its name, amount, start, or finish, which reopens the same form with a Save changes button and a Finished just now shortcut. An edit keeps the dose on its own day, and the start has to stay within the last 48 hours. A drink that is still being sipped shows a Done button that closes its window at the current moment, so a sip time that guessed wrong costs one tap to fix. Every row also has a delete button. Doses older than 48 hours are dropped automatically, since less than 1 percent of a dose remains by then.

## Settings

The gear opens a settings dialog with the half life slider, your bedtime, your sleep safe threshold (5 to 200 mg, default 50), a sip time for the one tap chips (0 to 120 minutes in steps of 5, default 0, which counts the whole drink the moment you tap), an editor for the quick drinks (up to 8, each with a name and milligrams), and Clear all doses. Clear all takes two taps: the first arms the button and the second, within four seconds, removes every logged dose. Your settings and drinks stay.

## Data

Everything lives in this browser's localStorage under one key, `appaday-159-caffeine`, holding the dose log (each dose with its start, finish, amount, and name), the settings, and the quick drinks. Reads and writes are wrapped in try/catch, and stored data is validated on load, so a damaged or hand edited entry falls back to safe defaults instead of breaking the page. Nothing is sent anywhere. The level refreshes every 30 seconds and whenever the tab becomes visible again.

## Notes

This is a planning estimate, not medical advice. Real half lives run from about 3 to 7 hours and shift with genetics, medications, pregnancy, and liver health. The 400 mg daily figure is the commonly cited guidance for healthy adults.

## Stack

One self-contained `index.html` in plain HTML, CSS, and vanilla JavaScript, with Fraunces from Google Fonts. No frameworks, no libraries, and no build step. Accent color is the Health category rose, `#D14D72`.
