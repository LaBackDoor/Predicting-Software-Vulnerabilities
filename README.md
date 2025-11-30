# Predicting Software Vulnerability Trends with Multi-Recurrent Neural Networks

This repository contains the datasets for the paper **"Predicting Software Vulnerability Trends with Multi-Recurrent Neural Networks: A Time Series Forecasting Approach"**, published in the *Proceedings of the 1st International Conference on NLP & AI for Cyber Security* (NLPAICS 2024).

## 📄 Abstract

Predicting software vulnerabilities effectively is crucial for enhancing cybersecurity measures in an increasingly digital world. Traditional forecasting models often struggle with the complexity and dynamics of software vulnerability data, necessitating more advanced methodologies. 

This paper introduces a novel approach using **Multi-Recurrent Neural Networks (MRN)**, which integrates multiple memory mechanisms and offers a balanced complexity suitable for time-series data. We compare MRNs against traditional models like ARIMA, Feedforward Multilayer Perceptrons (FFMLP), Simple Recurrent Networks (SRN), and Long Short-Term Memory (LSTM) networks. Our results demonstrate that MRNs consistently outperform these models, especially in settings with limited data or shorter forecasting horizons.

## 📂 Repository Structure
The `data` folder contains the time-series datasets used for the experiments in the paper, as well as additional cybersecurity attack datasets.

```text
Predicting-Software-Vulnerabilities/
└── data/
    ├── Google_Chrome_Soft_Vuln_2007_2024.csv        # Primary Dataset (Paper)
    ├── MacOS_Soft_Vuln_1998_2024.csv                # Primary Dataset (Paper)
    ├── 2020_2021_Hackmageddon_Daily_Attack_Dataset.csv
    ├── African Countries Attack Dataset.csv
    └── Attack Intensity Scraped.csv
```

## Dataset Descriptions
Primary Datasets (Used in Paper)
These datasets were used to evaluate the MRN model against benchmarks (ARIMA, LSTM, etc.).
Google Chrome (2007-2024): Contains 3,398 data points representing monthly vulnerability counts.
MacOS (1998-2024): Contains 2,626 data points representing monthly vulnerability counts.

Additional Datasets
Extra datasets included for broader cybersecurity trend analysis:
Hackmageddon Daily Attack Dataset (2020-2021): Daily logs of cyber attacks.
African Countries Attack Dataset: Regional specific attack data.
Attack Intensity Scraped: Data focusing on the intensity/volume of attacks over time.

## 🔗 Link to Paper
[Read the full paper on ACL Anthology](https://aclanthology.org/2024.nlpaics-1.5.pdf)

---

## 🏗️ Architecture: Multi-Recurrent Neural Networks (MRN)

The MRN is designed with a unique architecture that includes multiple memory banks, each tailored to capture and store historical data at different time scales. The architecture comprises three main layers: input, hidden, and output, each enhanced with layer-specific recurrent connections.

### Memory States Equations
The computation of memory states for hidden and output layers integrates layer-level and self-recurrency:

**Hidden Layer Memory ($M_h^t$):**
$$M_h^t = \left(\frac{1}{n_h}\right) \cdot H_{t-1} + \left(1 - \frac{1}{n_h}\right) \cdot M_{t-1,h}$$

**Output Layer Memory ($M_o^t$):**
$$M_o^t = \left(\frac{1}{n_o}\right) \cdot O_{t-1} + \left(1 - \frac{1}{n_o}\right) \cdot M_{t-1,o}$$

Where $n_h$ and $n_o$ represent the number of memory banks for the hidden and output layers, respectively.

### Sliding Window Technique
We employ a sliding window approach to transform the time-series data:
$$W_t = [x_{t-n+1}, x_{t-n+2}, . . . , x_t]$$

---

## 📊 Datasets

We assess the effectiveness of MRNs using data from the **National Vulnerability Database (NVD)**, focusing on two prominent software projects. Data was compiled from initial release up to February 2024.

| Dataset | Years Covered | Total Data Points |
| :--- | :--- | :--- |
| **MacOS** | 1998 - 2024 | 2626 |
| **Google Chrome** | 2007 - 2024 | 3398 |

---

## 📈 Experimental Results

We compared the MRN against ARIMA, FFMLP, SRN, and LSTM across different horizons ($t+1$ to $t+12$) and window sizes (WS). The values below represent the **Root Mean Squared Error (RMSE)**.

### 1. Benchmark: ARIMA
| Model | Platform | RMSE |
| :--- | :--- | :--- |
| ARIMA(3,0,3) | MacOS | 54.07447 |
| ARIMA(3,0,3) | Google Chrome | 25.58836 |

### 2. MacOS Results (RMSE)

**FFMLP (MacOS)**
| H / WS | t + 1 | t + 3 | t + 6 | t + 12 |
| :--- | :--- | :--- | :--- | :--- |
| 60 | 0.40336 | 0.43422 | 0.53392 | 0.73032 |
| 120 | 0.44657 | 0.49573 | 0.49588 | 0.60113 |
| 240 | 0.42006 | 0.43383 | 0.46002 | 0.57298 |
| **AVG** | **0.42333** | **0.45459** | **0.49660** | **0.63481** |

**SRN (MacOS)**
| H / WS | t + 1 | t + 3 | t + 6 | t + 12 |
| :--- | :--- | :--- | :--- | :--- |
| 60 | 0.31397 | 0.33528 | 0.32541 | 0.31656 |
| 120 | 0.28952 | 0.27212 | 0.31372 | 0.39398 |
| 240 | 0.29760 | 0.27874 | 0.28957 | 0.33560 |
| **AVG** | **0.30036** | **0.29538** | **0.30957** | **0.34871** |

**LSTM (MacOS)**
| H / WS | t + 1 | t + 3 | t + 6 | t + 12 |
| :--- | :--- | :--- | :--- | :--- |
| 60 | 0.21716 | 0.25819 | 0.30493 | 0.26870 |
| 120 | 0.24409 | 0.27300 | 0.24903 | 0.27651 |
| 240 | 0.16732 | 0.17508 | 0.18613 | 0.19833 |
| **AVG** | **0.20952** | **0.23543** | **0.24670** | **0.24785** |

**MRN (MacOS) - Proposed Model**
| H / WS | t + 1 | t + 3 | t + 6 | t + 12 |
| :--- | :--- | :--- | :--- | :--- |
| 60 | 0.01807 | 0.02489 | 0.02013 | 0.01902 |
| 120 | 0.00941 | 0.00803 | 0.00505 | 0.00570 |
| 240 | 0.14571 | 0.09890 | 0.02127 | 0.02815 |
| **AVG** | **0.05773** | **0.04394** | **0.01548** | **0.01763** |

### 3. Google Chrome Results (RMSE)

**MLP (Google Chrome)**
| H / WS | t + 1 | t + 3 | t + 6 | t + 12 |
| :--- | :--- | :--- | :--- | :--- |
| 60 | 0.25870 | 0.24368 | 0.26848 | 0.31967 |
| 120 | 0.27080 | 0.25844 | 0.29180 | 0.27403 |
| **AVG** | **0.26475** | **0.25106** | **0.28014** | **0.29685** |

**SRN (Google Chrome)**
| H / WS | t + 1 | t + 3 | t + 6 | t + 12 |
| :--- | :--- | :--- | :--- | :--- |
| 60 | 0.23157 | 0.25067 | 0.24441 | 0.22767 |
| 120 | 0.17548 | 0.20764 | 0.24820 | 0.23612 |
| **AVG** | **0.20352** | **0.22916** | **0.24631** | **0.23190** |

**LSTM (Google Chrome)**
| H / WS | t + 1 | t + 3 | t + 6 | t + 12 |
| :--- | :--- | :--- | :--- | :--- |
| 60 | 0.20461 | 0.20982 | 0.21128 | 0.21762 |
| 120 | 0.13460 | 0.13705 | 0.13880 | 0.11176 |
| **AVG** | **0.16961** | **0.17343** | **0.17504** | **0.16469** |

**MRN (Google Chrome) - Proposed Model**
| H / WS | t + 1 | t + 3 | t + 6 | t + 12 |
| :--- | :--- | :--- | :--- | :--- |
| 60 | 0.16779 | 0.17252 | 0.16196 | 0.15946 |
| 120 | 0.12160 | 0.13700 | 0.12153 | 0.12568 |
| **AVG** | **0.14469** | **0.14706** | **0.14948** | **0.14049** |

---

## 📚 Citation

If you use this code or dataset in your research, please cite our paper:

```bibtex
@inproceedings{orojo-etal-2024-predicting,
    title = "Predicting Software Vulnerability Trends with Multi-Recurrent Neural Networks: A Time Series Forecasting Approach",
    author = "Orojo, Abanisenioluwa  and
      Elumelu, Webster  and
      Orojo, Oluwatamilore  and
      Donnahoo, Micheal  and
      Hutton, Shaun",
    booktitle = "Proceedings of the 1st International Conference on NLP & AI for Cyber Security",
    month = jul,
    year = "2024",
    address = "Macau, China",
    publisher = "Association for Computational Linguistics",
    url = "[https://aclanthology.org/2024.nlpaics-1.5.pdf](https://aclanthology.org/2024.nlpaics-1.5.pdf)",
    pages = "42--47",
}
