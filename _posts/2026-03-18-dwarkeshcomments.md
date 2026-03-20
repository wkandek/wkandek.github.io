---
layout: post
title: "Transcription of Dylan Patel at Dwarkesh"
date: 2026-03-18
---
# Dylan Patel from SemiAnalysis at Dwarkesh

Dwarkesh interviews Dylan. Dylan is the CEO of a company that tracks the AI industry and sells the information called SemiAnalysis.

He argues that the real long‑run bottleneck to scaling AI is semiconductor manufacturing capacity—especially EUV tools from ASML—rather than power or data centers, and that Nvidia has moved fastest to lock up that constrained supply.
​
Hyperscalers (Google, Microsoft, Amazon, Oracle) ’ AI CapEx is heading toward roughly 600 Billion USD with a trillion dollars across the supply chain, with about 20 GW of new critical IT capacity coming online in the US this year.
Current US datacenter capacity is 75 GC, so about a 25% increase in 1 year.
The frontier labs  like OpenAI, Anthropic and DeeMind are racing to secure multi‑gigawatt fleets via long‑term GPU rental deals at these Hyperscalars.
Prices for the 3 year old H100 chip are now over 2 USD/hour. This is unexpected because the new NVidia chips are way more powerful, whcih shoudl make
the price go down. But there is so much demand that prices are now going up rather than down.

Meta and X also build DC capacity, but they use it internally and do not rent it out.

There are also the "Neoclouds", Corewease, Nebius, Crusoe, etc that are in the same business but at smaller scale.

Technically H100 have gotten better. GPT‑5.4 is both better and cheaper to serve than GPT‑4, so a given H100 can now produce far more and higher‑value tokens than a few years ago.
Its economic value has actually risen over time rather than depreciated quickly. There are investors/pundits (Michael Burry for example) that argue that these chips are close to worthless now
​
Dylan says DC construction and power is "solved". It is ugly, but the US will build DCs in remote locations and generate power by burning fossil fuels. Gasturbines are also sold out, but his company tracks
15 other technologies that can be used to generate power. 75% of all AI datacenters are in the US, but there is some movement internationally. I have a friend from Brazil who works at the DC company Scala
who says they have 5 GW of power available, but equipment prices in Brazil are to high due to Brazilian import taxes.

Eventually nuclear power will come online, or renewables and batteries.

To rent 1 GW of AI processing from a Hyperscaler costs about 12 Billion USD/year. Anthropic has about 35% margin on their services. They added 6 B USD in revenue in February, which means they need to rent
4B additional AI capacity from Hyperscalars. Same for OpenAI. The capacity is not really available, so Anthropic will have to go and get capacity where it can get it - Neoclouds for example. 
Meta just committed 27 B USD with Nebius (Nebius stock up 14%), apparently even though they build their own datacenters, they need more capacity faster.

The expectation that at year end both OpenAI will have 6 GW of DC  AI power. Up from the the current (2025) 2 GW. OpenAI had 800MW in 2024.
For scale - the new datacenters that are being built are trying to get to 1 GW in 1 location. X Colossus for example or Stargate's effort (Oracle in Abilene, TX.

The bottleneck is really building the chips needed (TSMC mostly) and ultimately in the machinery from ASML needed to make the chips
New "fabs" are slow to build. Elon Musk says he will build his own "TeraFab" now in response to that. XAi needs readily available chips, but also each car needs these chips fro self driving.

The needed capacity has partly come from  the mobile area, i.e. TSMC and other are making less mobile chips and more AI chips. Phoens are getting more expensive (at the high end) or are not being built in
the low and mid range - Xiaomi is cutting production by 50% for exmaple.

NVidia has an advantage as they locked in GPU and memory contracts for several years.

Some numbers:  To build 1 GW of Rubin‑class Nvidia GPUs you need ~55k 3 nm wafers, ~6k 5 nm wafers, and ~170k DRAM wafers. This translates to about 2 million EUV passes by an ASML tool and roughly 3.5 EUV scanners for that single gigawatt of capacity. There are currently 300 scanners in use. ASML currently ships about 70 EUV tools per year and may reach a bit over 100 by 2030, implying on the order of 200 GW of AI compute per year at the high end.
OpenAI wants a “gigawatt‑a‑week” growth for them alone, so about 25% of all production capacity, Anthropic similar.

China: China does not have EUV yet, maybe in the labs so their chips are 7nm technology rather than TSMC 2nm. This is a big disadvantage for Chinese chips that will lag behind in speed and memory integration. China is working on EUV, but it is a difficult technology with high precision optics (from Carl Zeiss) and lasers, etc that took ASML 10 years to get from the lab to production. The process has to be largely flawless, if not the wafer is useless... China has EUV in the lab, maybe or even likely by now. Remains to be seen when they will get into production. 
 
Memory: High Bandwidth Memory (HBM) consumes 3–4x more wafer area per bit than commodity DRAM, but offers vastly higher bandwidth density (≈2.5 TB/s per stack versus ~64–128 GB/s of DDR per comparable edge area), so the true scarce resource is bandwidth per wafer, not bits per wafer.
​
Because memory vendors under‑invested in fabs during the downcycle, and new DRAM fabs take ~2 years to build, there’s now a multi‑year HBM and DRAM crunch where roughly one‑third of Big Tech CapEx is going into memory; Dylan expects smartphone shipments to fall sharply and phones/PCs to get more expensive or worse‑specced as bits are diverted from consumers to AI accelerators.

Space GPUs: Dwarkesh had interviewed Elon 2 weeks ago and Space GPUs and a TeraFab where things that came up. Dylan does not think that is feasible. He thinks the complexity of SpaceGPU outweighs the benefits and the that building a fab is not very straightforward...
