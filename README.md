# A Comparative Analysis of Image Processing Implementations 

> **Author:** Nadav Izhaki
> **Institution:** Holon Institute of Technology (HIT)
> **Course:** Classical Image Processing

## 📌 Overview
Digital image processing requires a constant balance between mathematical precision and computational efficiency. This project explores the transition from algorithmic theory to practical engineering by comparing custom-built image processing algorithms against industry-standard, hardware-optimized libraries.

The analysis evaluates point operations (Binarization, Gamma Correction) and spatial operations (Convolution) to highlight the impact of different programming paradigms on execution time, boundary condition handling, and scalability.

##  Tech Stack
* **Language:** Python
* **Libraries:** NumPy (Array Processing), OpenCV (Baseline & Comparison), `time` module (Benchmarking)

##  Methodology
The project implements and benchmarks algorithms across two main categories:

### 1. Point Operations (Binarization & Gamma Correction)
Evaluated using three distinct approaches:
* **Nested Loops:** A brute-force, iterative pixel-by-pixel approach with strict data-type casting (`uint8` -> `float32` -> `uint8`).
* **Vectorization:** Parallel operations across the entire image matrix using NumPy's built-in boolean indexing and power functions.
* **Look-Up Tables (LUT):** Fetching pre-calculated results for all 256 grayscale intensities to bypass real-time mathematical computations.

### 2. Spatial Operations (Convolution)
* **Custom Filter (`my_imfilter`):** A sliding window implementation supporting variable kernel sizes, dynamic Zero-Padding, and floating-point overflow control (clipping to [0, 255]).
* **Library Baseline:** Benchmarked against `cv2.filter2D`.

## 📊 Key Results

### Performance & Efficiency
* **Point Operations:** Replacing nested loops with Vectorization and LUTs reduced execution time drastically—from **~223ms** to just **2-4ms**. OpenCV's native function performed at <0.1ms.
* **Spatial Operations (Stress Test):** Scaling the kernel from 3x3 to 15x15 caused a severe exponential slowdown in the custom `my_imfilter` (which operates at O(K^2) complexity), while `cv2.filter2D` remained highly stable under the same load.

### Accuracy & Sanity Checks
* **Point Operations:** All three custom implementations yielded strictly identical mathematical and visual outputs.
* **Spatial Operations:** A Dilated Binary Heatmap revealed that mathematical discrepancies between the custom filter and OpenCV occurred *exclusively* at the image boundaries. This was due to the difference in padding strategies (Zero-Padding vs. Reflection Padding).

## 💡 Conclusions & Future Work
* **Transparency vs. Efficiency:** Implementing algorithms from scratch provides invaluable "X-ray vision" into the black box of image processing (exposing nuances like value clipping and data-type casting). It is essential when highly specific custom logic is needed.
* **The Reality of Scaling:** For real-world, large-scale computer vision applications, integrating optimized industrial libraries (like OpenCV) is an absolute necessity to avoid crippling computational bottlenecks.
* **Future Work:** Further optimization of the custom spatial convolution could be achieved by implementing Separable Filters or utilizing GPU acceleration.
