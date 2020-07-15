<script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>
# LSTMs and HMMs to predict lithium-ion battery lifetimes

## Introduction

### Background and motivation

{% include_relative background.md %}

### Dataset
For our data, we used NASA's publicly accessible repository of Lithium Ion Aging Datasets [NASA's publicly accessible repository of Lithium Ion Aging Datasets](https://catalog.data.gov/dataset/li-ion-battery-aging-datasets). These data were collected from a testbed at the NASA Ames Prognostics Center of Excellence (PCoE).  A set of four Li-ion batteries were run through 3 different operational profiles (charge, discharge and impedance) at room temperature.

Charging was carried out in a constant current (CC) mode at 1.5A until the battery voltage reached 4.2V and then continued in a constant voltage (CV) mode until the charge current dropped to 20mA.

Discharge was carried out at a constant current (CC) level of 2A until the battery voltage fell to 2.7V, 2.5V, 2.2V and 2.5V for batteries 5 6 7 and 18 respectively.

Impedance measurement was carried out through an electrochemical impedance spectroscopy frequency sweep from 0.1Hz to 5kHz.

Repeated charge and discharge cycles result in accelerated aging of the batteries while impedance measurements provide insight into the internal battery parameters that change as aging progresses.
The experiments were stopped when the batteries reached end-of-life (EOL) criteria, which was a 30% fade in rated capacity (from 2Ahr to 1.4Ahr).

Initially, we looked at the Impedance cycle to see if we could discern any kind of pattern that would suggest the age of the batteries. However, after some discussion, we decided to use the discharge cycle instead.

To clean and prepare the data for consumption by our LSTM and HMM code (written in python), we had to convert the MATLAB data files into csv files.

To do this we wrote [MATLAB scripts](https://cs4641team4summer2020.github.io/matlab) to traverse the data structures and extract the data we required. We found that most of the data was already pretty clean except for some areas where initial readings were abnormal or non-existent. We removed the ones that were anomalous.



#### csv files of batteries (discharge and impedance)

[B0005 discharge](https://cs4641team4summer2020.github.io/datasets/B0005-discharge.csv)  
[B0005 impedance](https://cs4641team4summer2020.github.io/datasets/B0005-impedance.csv)  
[B0006 discharge](https://cs4641team4summer2020.github.io/datasets/B0006-discharge.csv)  
[B0006 impedance](https://cs4641team4summer2020.github.io/datasets/B0006-impedance.csv)  
[B0007 discharge](https://cs4641team4summer2020.github.io/datasets/B0007-discharge.csv)  
[B0007 impedance](https://cs4641team4summer2020.github.io/datasets/B0007-impedance.csv)  
[B0018 discharge](https://cs4641team4summer2020.github.io/datasets/B0018-discharge.csv)  
[B0018 impedance](https://cs4641team4summer2020.github.io/datasets/B0018-impedance.csv)

## Results

### LSTM

{% include_relative lstmresults.md %}

### HMM

{% include_relative hmmresults.md %}

## Conclusion

### Comparison of models

{% include_relative comparison.md %}

### Further work

{% include_relative furtherwork.md %}

## References

Y. Zhang, R. Xiong, H. He and Z. Liu, "A LSTM-RNN method for the lithuim-ion battery remaining useful life prediction," 2017 Prognostics
and System Health Management Conference (PHM-Harbin), Harbin, 2017, pp. 1-4, doi: 10.1109/PHM.2017.8079316.

## Credits
The data was cleaned by Luca and Daniel, the HMM model was created by Frank and Angad, and the LSTM model was created by Joel.
