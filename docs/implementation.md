# Implementation

This page provides a guide for each version of the BLS (Box Least Squares) algorithm, including implementations in sequential, OpenMP, MPI, STAPL, and PyTorch. Each section offers a detailed explanation of the respective implementation, including descriptions of functions and key components.

## Overview

The following structs are used throughout the implementation, for defining functions types:

```c
struct PERIODParameters {
  double minimum_period;
  double maximum_period;
  double total_duration;
};
```

- `PERIODParameters` holds the range of periods and the total time duration for the analysis.

```c
struct BLSResult {
  double best_period;
  double best_duration;
  double best_phase;
  double best_d_value;
};
```

- `BLSResult` stores the results of the BLS algorithm, including the best period, best duration, best phase, and the lowest d-value (the metrics used to evaluate the best fit).

Additionally, the following typedef is used in the implementation:

```c
typedef std::tuple<double, double, double> SPECParameters;
```

### Core Functions

| Function name              | Description                                 |
| -------------------------- | ------------------------------------------- |
| [`min_value`](#id_min_value)                | Returns the minimum value in a vector.      |
| [`max_value`](#id_max_value)                | Returns the maximum value in a vector.      |
| [`ptp`](#id_ptp)                      | Returns the peak-to-peak value in a vector. |
| [`arange`](#id_arange)                   | Generates a range of values.                |
| [`linspace`](#id_linspace)                 | Generates a linearly spaced range of values.|
| [`auto_phase`](#id_auto_phase)               | Generates a phase vector.                   |
| [`auto_max_min_period`](#id_auto_max_min_period)      | Generates the maximum and minimum period.   |
| [`auto_period`](#id_auto_period)              | Generates a period vector.                  |
| [`spec_generator`](#id_spec_generator)           | Generates a vector of SPEC parameters.      |
| [`spec_generator_gambiarra`](#id_spec_generator_gambiarra) | Generates a vector of SPEC parameters.      |
| [`compute_trel`](#id_compute_trel)             | Computes the relative time vector.          |
| [`normalize`](#id_normalize)                | Normalizes a vector.                        |
| [`compute_weights`](#id_compute_weights)          | Computes the weights vector.                |
| [`model`](#id_model)                    | Computes the model for the BLS algorithm.   |
| [`bls`](#id_bls)                      | Computes the BLS algorithm.                 |
| [`readCSV`](#id_readCSV)                  | Reads a CSV file.                           |

### Detailed Function Descriptions

<a name="id_min_value">

#### `min_value`

```c
template <typename T> T min_value(const std::vector<T> &v);
```

**Description**  
Get the minimum element in a vector.

**Parameters:**  

- **`T`** Type of the elements in the vector.
- **`v`** The vector from which to get the minimum value

**Returns:**  
  The minimum element in the vector.

---

<a name= "id_max_value">

#### `max_value`

```c
template <typename T> T max_value(const std::vector<T> &v);
```

**Description**
Get the maximum element in a vector.

**Parameters:**  

- **`T`** Type of the elements in the vector.
- **`v`** The vector from which to get the maximum value

**Returns:**
  The maximum element in the vector.

---

<a name="id_ptp">

#### `ptp`

```c
template <typename T> T ptp(const std::vector<T> &v);
```

**Description**
Get the peak-to-peak value in a vector, which is the difference between the maximum and minimum values.

**Parameters:**  

- **`T`** Type of the elements in the vector.
- **`v`** The vector from which to get the peak-to-peak value

**Returns:**
  The peak-to-peak value in the vector.

---

<a name="id_arange">

#### arange

```c
std::vector<double> arange(double start, double end, double step);
```

**Description**
Generates a range of values from `start` to `end` with a given step size. It's similar to the `numpy.arange` function in Python.

**Parameters:**  

- **`start`** The starting value of the range.
- **`end`** The ending value of the range, not inclusive.
- **`step`** The step size between values.

**Returns:**
  A vector of values from `start` to `end` with a step size of `step`.

---

<a name="id_linspace">

#### linspace

```c
std::vector<double> linspace(double start, double end, size_t num);
```

**Description**
Generates a linearly spaced range of values from `start` to `end` with a given number of elements. It's similar to the `numpy.linspace` function in Python.

**Parameters:**

- **`start`** The starting value of the range.
- **`end`** The ending value of the range.
- **`num`** The number of elements in the range.

**Returns:**
  A vector of `num` values from `start` to `end`.

---

<a name="id_auto_phase">

#### auto_phase

```c
std::vector<double> auto_phase(double period, double duration);
```

**Description**
Generates a vector os phases for a given period and duration. It's the cpp version of the `auto_phase` function from `astropy`.

**Parameters:**

- **`period`** The period of the signal.
- **`duration`** The duration of the signal.

**Returns:**
  A vector of phases.

---

<a name="id_auto_max_min_period">

#### auto_max_min_period

```c
PERIODParameters auto_max_min_period(std::vector<double> &time);
```

**Description**
Generates the maximum and minimum period for a given time vector. It's the cpp version of the `auto_max_min_period` function from `astropy`.

**Parameters:**

- **`time`** The time vector.

**Returns:**
  A `PERIODParameters` struct with the minimum period, maximum period, and total duration of the time vector (it's the `ptp` of the time vector).

---

<a name="id_auto_period">

#### auto_period

```c
std::vector<double> auto_period(double minimum_period = -1,
                                double maximum_period = -1,
                                double total_duration = -1);
```

**Description**
Generates a vector of periods, given the minimum period, maximum period, and total duration. It's the cpp version of the `auto_period` function from `astropy`.

**Parameters:**

- **`minimum_period`** The minimum period.
- **`maximum_period`** The maximum period.
- **`total_duration`** The total duration.

**Returns:**
  A vector of periods.

---

<a name="id_spec_generator">

#### spec_generator

```c
std::vector<SPECParameters> spec_generator(std::vector<double> &time);
```

**Description**
Generates a vector of `SPECParameters` for a given time vector. This function is used to generate the candidates for running the BLS algorithm. However, it currently needs improvements as it generates a large number of candidates.

**Parameters:**

- **`time`** The time vector.

**Returns:**
  A vector of `SPECParameters`, which are tuples of `(period, duration, phase)`, the candidates for the BLS algorithm.

---

<a name="id_spec_generator_gambiarra">

#### spec_generator_gambiarra

```c
std::vector<SPECParameters> spec_generator_gambiarra(std::vector<double> &time);
```

**Description**
It's the simple version of the `spec_generator` function. It generates a vector of `SPECParameters` for a given time vector. This function is used to generate the candidates for running the BLS algorithm.

**Parameters:**

- **`time`** The time vector.

**Returns:**
  A vector of `SPECParameters`, which are tuples of `(period, duration, phase)`, the candidates for the BLS algorithm.

---

<a name="id_compute_trel">

#### compute_trel

```c
std::vector<double> compute_trel(std::vector<double> &time);
```

**Description**  
The `compute_trel` function calculates the relative time vector. It subtracts the minimum value of the time vector (`t0`) from each element, returning a vector of relative times. This is useful for normalizing the time data for further processing.

**Parameters:**

- **`time`**  
  A vector of time values (`std::vector<double>`), typically representing the time data.

**Returns:**  
  A vector of relative time values (`std::vector<double>`), where each value is the difference between the corresponding time and the minimum value in the input time vector.

---

<a name="id_normalize">

#### normalize

```c
std::vector<double> normalize(std::vector<double> &flux);
```

**Description**  
The `normalize` function takes a vector of flux values and normalizes them by subtracting the mean and dividing by the standard deviation. This is useful for transforming the flux data to have a mean of 0 and standard deviation of 1, which is common in many data analysis techniques.

**Parameters:**

- **`flux`**  
  A vector of flux values (`std::vector<double>`), representing the signal data.

**Returns:**  
  A normalized vector of flux values (`std::vector<double>`), where each value is the original flux value minus the mean, divided by the standard deviation.

---

<a name="id_compute_weights">

#### compute_weights

```c
std::vector<double> compute_weights(std::vector<double> &flux_err);
```

**Description**  
The `compute_weights` function calculates the weights based on the provided flux errors. The weights are computed as the inverse square of the flux errors and are normalized so that their sum is 1. This is commonly used in weighted averaging or fitting algorithms where data points have different uncertainties.

**Parameters:**

- **`flux_err`**  
  A vector of flux error values (`std::vector<double>`), representing the uncertainty in the flux measurements.

**Returns:**  
  A vector of normalized weights (`std::vector<double>`), where each weight is inversely proportional to the square of the corresponding flux error and normalized so that the sum of the weights is 1.

---

<a name="id_model">

#### model

```c
double model(std::vector<double> &t_rel, std::vector<double> &flux,
             std::vector<double> &weights, double period, double duration,
             double phase);
```

**Description**  
The `model` function is crucial as it evaluates the metrics to determine the presence of a transit. It assesses a transit model using the provided time, flux, weights, period, duration, and phase. By calculating the occurrence of a transit at each point in the time vector and applying weighted averages, it computes a goodness-of-fit metric (`d_value`). This metric is essential for identifying the best-fitting transit model.

**Parameters:**

- **`t_rel`**  
  A vector of relative time values (`std::vector<double>`), which represents the time data normalized by subtracting the minimum value.
  
- **`flux`**  
  A vector of flux values (`std::vector<double>`), representing the observed light curve data.

- **`weights`**  
  A vector of weights (`std::vector<double>`), computed from the flux errors.

- **`period`**  
  The period of the transit event (in the same units as the time vector).

- **`duration`**  
  The duration of the transit event.

- **`phase`**  
  The phase of the transit event, specifying where in the period the transit occurs.

**Returns:**  
  A `double` representing the calculated goodness-of-fit metric (`d_value`) for the given transit model. This value is useful for determining how well a transit model fits the data.

---

<a name="id_bls">

#### bls

```c
BLSResult bls(std::vector<double> &time, std::vector<double> &flux,
              std::vector<double> &flux_err,
              std::vector<SPECParameters> &s_params);
```

**Description**  
The `bls` function implements the Box Least Squares (BLS) algorithm for detecting periodic transit-like events in a time series of flux measurements. It evaluates various candidate models based on the input parameters (period, duration, and phase) and returns the model that best fits the data, according to a goodness-of-fit metric (`d_value`).

**Parameters:**

- **`time`**  
  A vector of time values (`std::vector<double>`), representing the time data for the light curve.

- **`flux`**  
  A vector of flux values (`std::vector<double>`), representing the observed light curve data.

- **`flux_err`**  
  A vector of flux error values (`std::vector<double>`), representing the uncertainty in the flux measurements.

- **`s_params`**  
  A vector of `SPECParameters` (`std::vector<SPECParameters>`), which are tuples of `(period, duration, phase)` representing the candidate transit models to evaluate.

**Returns:**  
  A `BLSResult` structure, which contains the following:

- **`best_d_value`**  
  The best goodness-of-fit metric (`double`) found during the search.

- **`best_period`**  
  The period (`double`) of the best-fitting transit model.

- **`best_duration`**  
  The duration (`double`) of the best-fitting transit model.

- **`best_phase`**  
  The phase (`double`) of the best-fitting transit model.

---

<a name="id_readCSV">

#### readCSV

```c
void readCSV(const std::string &filename, std::vector<double> &time,
             std::vector<double> &flux, std::vector<double> &flux_err);
```

**Description**  
The `readCSV` function reads a CSV file and extracts the time, flux, and flux error values into the provided vectors. The function assumes that the CSV file contains at least three columns (time, flux, and flux error) and that the first line is a header.

**Parameters:**

- **`filename`**  
  A `string` representing the path to the CSV file.

- **`time`**  
  A vector of time values (`std::vector<double>`), which will be filled with the time data from the file.

- **`flux`**  
  A vector of flux values (`std::vector<double>`), which will be filled with the flux data from the file.

- **`flux_err`**  
  A vector of flux error values (`std::vector<double>`), which will be filled with the flux error data from the file.

**Returns:**  
  This function does not return a value, but it populates the provided `time`, `flux`, and `flux_err` vectors with data from the CSV file. If the file cannot be opened, an error message is printed to `stderr`.

---

## Sequential Implementation

## OpenMP Implementation

## MPI Implementation

## STAPL Implementation

## PyTorch Implementation
