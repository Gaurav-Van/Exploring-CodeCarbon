# Exploring-CodeCarbon
Diving into the world of Tracking CO2 Emissions from our software or code.  Code Carbon is a lightweight open-source Python Library that lets you track the Co2 emissions produced by running code. 

<b>References</b>
- https://codecarbon.io/
- https://www.co2signal.com/
- https://mlco2.github.io/codecarbon/usage.html
- Energy and Policy Considerations for Deep Learning in NLP: https://arxiv.org/pdf/1906.02243
- Energy Usage Reports: Environmental awareness as part of algorithmic accountability: https://arxiv.org/pdf/1911.08354
- https://medium.com/@ilievski.vladimir/track-the-co2-emissions-of-your-python-code-the-same-way-you-time-it-afd5688a8645 [Code from this article is used for practicing]

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

<hr>

## How CodeCarbon Works
CodeCarbon uses a scheduler that, by default, calls for a measure every 15 seconds, so it has no significant overhead. The scheduler is started when the first start method is called and stopped when stop method is called.

1. Tracks the electricity consumption of the machine on which the code is executed. This is measured in kilowatts (kWh).
2. Estimates the CO2 emissions per kWh of the electricity in the same geolocation where the machine resides.

The first task is less prone to errors as the environment is predictable. CodeCarbon is measuring the energy consumption of the CPU, GPU (if available) and the RAM memory by taking samples every 15 seconds by default.

To compute the carbon intensity, the library relies on the `CO2 Signal API`. This API gives the sources of energy in the region where the computation is taking place. For cloud-based computing, this is even more relevant because there is a precise geo-location and sources of energy that the computing center is using.
Finally, using the hardware energy consumption and the CO2 emissions of the electricity we calculate the total carbon footprint as a simple multiplication between them.

## Install 
```bash
pip install codecarbon
```
#### Dependencies 
The following packages are used by the CodeCarbon package, and will be installed along with the package itself
```txtfile
arrow
pandas
pynvml
requests
psutil
py-cpuinfo
click
rapidfuzz
prometheus_client
```
<hr>

## Exploring CodeCarbon Using Keras IMDb Dataset
```python
from codecarbon import EmissionsTracker
```
The IMDb sentiment analysis dataset in Keras is a widely-used dataset for training models to classify movie reviews as positive or negative. It contains 50,000 reviews, split evenly into training and testing sets. Each review is preprocessed into a sequence of integers, where each integer represents a specific word in the dictionary. This preprocessing step helps in standardizing the input for neural network models, making it easier to handle text data.

When you print the train and test data, you see arrays of numbers because the text reviews have been converted into sequences of integers. Each integer corresponds to a word’s index in a dictionary of the most frequent words in the dataset. 

#### Code Skeleton
```python
from codecarbon import EmissionsTracker
tracker = EmissionsTracker()
tracker.start()
try:
     # Compute intensive code goes here
     _ = 1 + 1
finally:
     tracker.stop()
```

#### Analysis of Loading Dataset 
```python
tracker = EmissionsTracker(project_name="codeCarbon_IMDbSentimentData_LoadingData_Analysis")
tracker.start_task("load Dataset")
(x_train, y_train), (x_test, y_test) = imdb.load_data(num_words=max_features)
print(f"Length of Training Data: {len(x_train)}\n\n{x_train}")
print("\n==============\n")
print(f"Length of Test Data: {len(x_test)}\n\n{x_test}")
tracker.stop_task()
tracker.stop()
```
![image](https://github.com/user-attachments/assets/009dc2a9-725c-4488-b37c-a167f2049950)

#### Analysis of Model Training
```python
tracker = EmissionsTracker(project_name="codeCarbon_IMDbSentimentData_carbonAnalysis")
tracker.start()
train_model()
tracker.stop()
```
![image](https://github.com/user-attachments/assets/659dc8ca-e9a1-49ca-a5af-980665f6a03b)

The summary of every run is saved as one row in a file named emissions.csv by default. The library also comes with command line tool named `carbonboard` that produces a dashboard showing equivalents of the carbon emission produced by the experiment.


