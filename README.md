# Exploring-CodeCarbon
Diving into the world of Tracking CO2 Emissions from our software or code.  Code Carbon is a lightweight open-source Python Library that lets you track the Co2 emissions produced by running code. 

<b>Reference</b>
- https://codecarbon.io/
- Energy Usage Reports: Environmental awareness as part of algorithmic accountability: https://arxiv.org/pdf/1911.08354
- https://medium.com/@ilievski.vladimir/track-the-co2-emissions-of-your-python-code-the-same-way-you-time-it-afd5688a8645

## Code Carbon 
Code Carbon is a lightweight open-source Python Library that lets you track the Co2 emissions produced by running code.

```txtfile
CodeCarbon is a lightweight software package that seamlessly integrates into your Python codebase.
It estimates the amount of carbon dioxide (CO2) produced by the cloud or personal computing resources
used to execute the code
```

```txtfile
It then shows developers how they can lessen emissions by optimizing their code or by hosting their
cloud infrastructure in geographical regions that use renewable energy sources
```
#### Methodology
Carbon dioxide (CO₂) emissions, expressed as kilograms of CO₂-equivalents [CO₂eq], are the product of two main factors :

- `C = Carbon Intensity of the electricity consumed for computation: quantified as g of CO₂ emitted per kilowatt-hour of electricity.`

- `E = Energy Consumed by the computational infrastructure: quantified as kilowatt-hours.`

Carbon dioxide emissions (CO₂eq) can then be calculated as `C * E`

#### Carbon Intensity 
Carbon Intensity of the consumed electricity is calculated as a `weighted average` of the emissions from the `different energy sources` that are used to generate electricity, including fossil fuels and renewables. In this toolkit, the fossil fuels coal, petroleum, and natural gas are associated with specific carbon intensities: a known amount of carbon dioxide is emitted for each kilowatt-hour of electricity generated. Renewable or low-carbon fuels include solar power, hydroelectricity, biomass, geothermal, and more. 

```txtfile
Based on the mix of energy sources in the local grid,
this package calculates the Carbon Intensity of the electricity consumed.
```

#### Power Usage 
Power supply to the underlying hardware is tracked at frequent time intervals. This is a configurable parameter `measure_power_secs`, with default value `15 seconds`, that can be passed when instantiating the emissions’ tracker.

`GPU`: Tracks Nvidia GPUs energy consumption using pynvml library (installed with the package).

`RAM`: CodeCarbon uses a 3 Watts for 8 GB ratio source. 

`CPU`: On Windows or Mac (Intel), Tracks Intel processors energy consumption using the Intel Power Gadget.

`Apple Silicon Chips (M1, M2)`: Apple Silicon Chips contain both the CPU and the GPU. Codecarbon tracks Apple Silicon Chip energy consumption using powermetrics. It should be available natively on any mac. 

