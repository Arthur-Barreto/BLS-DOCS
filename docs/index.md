# Homepage

This page provides an overview of my Summer Research Project at the University of Illinois at Urbana-Champaign, where I worked under the guidance of Professor Lawrence Rauchwerger. The project focuses on optimizing the Box Least Squares (BLS) algorithm for planetary transit detection. This two-month research effort marks the beginning of a longer-term project, and I'm excited to share our progress and results.

![SRP](img/arthur_foto.JPG){: width="80%", style="display: block; margin: 0 auto;"}

The primary goal of this work is to implement a parallel version of the BLS algorithm, which is essential for analyzing data from space missions like Kepler and K2. By leveraging parallel computing frameworks, including OpenMP, MPI, and GPU acceleration through PyTorch, we aim to improve the efficiency of the BLS algorithm. Additionally, the STAPL library, which abstracts the use of OpenMP and MPI, plays a crucial role in our approach.

One of the suggested improvements for this project is optimizing the candidate generation process for the BLS algorithm. This involves ensuring that key parameters, such as the transit period, are accurately represented in a comprehensive set of candidates for testing.

## Documentation

For more detailed information about the project, please refer to the following documentation:

- [The BLS Algorithm](bls.md)
- [Implementation in Different Technologies](implementation.md)

Each page offers a comprehensive explanation of the respective implementation, including detailed descriptions of key functions and algorithms.

## How to contribute

If you're interested in contributing to this project, please visit the [contributing](contributing.md) page for detailed information on how to get involved.

## Credits and Attributions

This project makes use of the following open-source projects:

- **lightkurve**: A friendly package for Kepler & TESS time series analysis in Python.
  - License: [MIT License](https://opensource.org/license/mit)
  - Source: [GitHub Repository](https://github.com/lightkurve/lightkurve.git)

- **Astropy**: A comprehensive library for astronomy-related computations and data manipulation, providing tools for working with astronomical data and performing complex calculations.
  - License: [BSD-3-Clause License](https://opensource.org/licenses/BSD-3-Clause)
  - Source: [GitHub Repository](https://github.com/astropy/astropy)
