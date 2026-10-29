# Recommendation (for a distribution utility)

## What the model can do

Using only 1-minute whole-house meter data, a random forest estimates when the washing machine runs. Tested on homes
it had never seen (6 UK homes), it finds about **2 out of 3 washing-machine minutes**, but only about **1 in 5** of its
"on" predictions is correct. Its estimate of washing-machine energy per home was off by **4 % to 64 %** (average size
about 30 %).

**Good for:** rough trends over weeks or months, and comparisons across many homes (e.g. "how much laundry happens
at peak time in this area"), where errors partly average out.

**Not good for:** billing or exact per-appliance numbers, messages about a single wash, real-time use (the model uses
15 minutes of future data), or homes very different from the training homes without testing first.

## Opportunity: move washing out of the evening peak

- **17.5 % of the 2,893 washes start between 16:00 and 20:00**, the busiest time on the grid (31 % in House 9).
- A wash uses about 0.5 kWh, mostly in a ~2 kW heating block, and most machines have a delay timer.
- House 8 already washes mostly at night (56 % of washes), so shifting is realistic.

**Idea:** send a "use your delay timer / wash off-peak" message (optionally with a time-of-use tariff) to homes that
the model shows washing in the evening over several weeks.

**How to measure it:** a randomised trial. Split those homes randomly into a group that gets the message and a group
that doesn't, and compare the change in **average household load between 16:00 and 20:00** over 8–12 weeks. This
comes straight from the meter, so model errors can't bias the result. The goal is less peak load, not less energy.

## Limitations

- **Overlapping signatures:** kettles, dishwashers, tumble dryers, electric heaters and washer-dryers all have ~2 kW
  heaters and cause false alarms. Cold washes without heating are almost invisible.
- **Missing data:** 11–25 % of minutes are missing per home, outages last up to weeks, and the official data is
  forward-filled, so some gaps look like "off".
- **Household differences:** wash duration (33–97 min) and energy (0.29–0.81 kWh) vary a lot between homes; with only
  6 homes, accuracy varies a lot too (F1 0.19–0.48).
- **UK → India:** many Indian washing machines (top-loaders, semi-automatics) have **no heater** and use cold water,
  so the 2 kW pattern the model relies on is often missing. Big loads differ (air conditioners, geysers, water pumps,
  inverters), and voltage drops and power cuts are common. The model would need to be retrained and tested on Indian
  data; the method (cleaning, cycle rule, leave-one-house-out testing) can be reused.

## With only 15- or 30-minute data

Each reading would be the average of everything in 15–30 minutes. A 15-minute 2 kW heating block would become a small
bump mixed with the kettle, oven and everything else, so the shape the model uses disappears. Finding individual
washes and their start time would become unreliable. Only rough estimates over long periods (e.g. monthly laundry
energy, or typical washing times across many homes) would still be possible, and they would need many more homes.
