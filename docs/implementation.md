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
| `min_value`                | Returns the minimum value in a vector.      |
| `max_value`                | Returns the maximum value in a vector.      |
| `ptp`                      | Returns the peak-to-peak value in a vector. |
| `arange`                   | Generates a range of values.                |
| `linspace`                 | Generates a linearly spaced range of values.|
| `auto_phase`               | Generates a phase vector.                   |
| `auto_max_min_period`      | Generates the maximum and minimum period.   |
| `auto_period`              | Generates a period vector.                  |
| `spec_generator`           | Generates a vector of SPEC parameters.      |
| `spec_generator_gambiarra` | Generates a vector of SPEC parameters.      |
| `compute_trel`             | Computes the relative time vector.          |
| `normalize`                | Normalizes a vector.                        |
| `compute_weights`          | Computes the weights vector.                |
| `model`                    | Computes the model for the BLS algorithm.   |
| `bls`                      | Computes the BLS algorithm.                 |
| `readCSV`                  | Reads a CSV file.                           |

### Detailed Function Descriptions

---

#### `min_value`

```c
template <typename T> T min_value(const std::vector<T> &v);
```

- **Description**  
Get the minimum element in a vector.

- **Parameters:**  
    - **`T`** Type of the elements in the vector.
    - **`v`** The vector from which to get the minimum value

- **Returns:**  
  The minimum element in the vector.

```c
template <typename T> T max_value(const std::vector<T> &v);
```

```c
template <typename T> T ptp(const std::vector<T> &v);
```

```c
std::vector<double> arange(double start, double end, double step);
```

```c
std::vector<double> linspace(double start, double end, size_t num);
```

```c
std::vector<double> auto_phase(double period, double duration);
```

```c
PERIODParameters auto_max_min_period(std::vector<double> &time);
```

```c
std::vector<double> auto_period(double minimum_period = -1,
                                double maximum_period = -1,
                                double total_duration = -1);
```

```c
std::vector<SPECParameters> spec_generator(std::vector<double> &time);
```

```c
std::vector<SPECParameters> spec_generator_gambiarra(std::vector<double> &time);
```

```c
std::vector<double> compute_trel(std::vector<double> &time);
```

```c
std::vector<double> normalize(std::vector<double> &flux);
```

```c
std::vector<double> compute_weights(std::vector<double> &flux_err);
```

```c
double model(std::vector<double> &t_rel, std::vector<double> &flux,
             std::vector<double> &weights, double period, double duration,
             double phase);
```

```c
BLSResult bls(std::vector<double> &time, std::vector<double> &flux,
              std::vector<double> &flux_err,
              std::vector<SPECParameters> &s_params);
```

```c
void readCSV(const std::string &filename, std::vector<double> &time,
             std::vector<double> &flux, std::vector<double> &flux_err);
```

## Sequential Implementation

## OpenMP Implementation

## MPI Implementation

## STAPL Implementation

## PyTorch Implementation
