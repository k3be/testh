# TestH

*TestH* is an ANSI C library that allows one to study **self-similar processes** by computing the **Hurst parameter** (also known as the Hurst exponent). The library has been made in an effort to provide researchers with means to estimate the Hurst parameter and to generate sequences with the self-similarity property embedded in the data. *TestH* implements several estimators of the Hurst parameter and generators of self-similar sequences that are already available in the literature. However, it does so in a single software package in a normalized and comprehensive way not found in other tools.

*TestH* provides a friendly API that allows writing customized programs with ease and few programming skills. The library provides generic methods to instantiate processes (by means of any of the generators or by reading data files), compute statistical information, and submit the processes to estimators of the Hurst parameter while pretty-printing the output. 

*TestH* has been written for researchers and for research purposes. With that in mind, the library can be used in a wide array of research fields.

# Example

Below is an example showing a *fractional Brownian motion* (fBm) process being generated with a pre-defined target Hurst parameter, which is then transformed to a *fraction Gaussian noise* (fGn) process. Afterwards, the aggregate processes that are needed by the procedure to estimate the Hurst parameter are computed and finally the Variance Time (VT) method is called upon.

```
#include "process.h"
#include "generators.h"
#include "estimators.h"
#include "io.h"

// fBm-SGA
#define N		100000
#define H		(double) 0.80
#define S		16

// scales
#define MIN		2
#define MAX		20
#define STEP	2

int main (void) {
  TestHVerbosity = TestH_HIGH;

  proc_Process *proc = gen_fBmSequentialGenerationAlgorithm (N, H, S, TestH_fBm);
  proc_FractionalGaussianNoise (proc);
  
  proc_ScalesConfig *conf = 
  proc_CreateScalesConfig (TestH_POW, 7, 11, 2);
  proc_CreateScales (proc, conf);
  proc_PrintProcess (proc);
  
  est_VarianceTime (proc);
  
  proc_DeleteProcess (proc);
  io_PrintDone ();
  return EXIT_SUCCESS;
}

```

# References and Materials

For detailed information about *TestH*, please refer to the following publications:

* Diogo A. B. Fernandes, Miguel Neto, Liliana F. B. Soares, Mário M. Freire, and Pedro R. M. Inácio. A Tool for Estimating the Hurst Parameter and for Generating Self-Similar Sequences. In *Proc. of the 46th Summer Computer Simulation Conference (SCSC) (part of the Summer Simulation Multi-Conference (SummerSim))*, pages 1–8, Monterey, CA, USA, Jul. 6–10 2014. SCS. Acceptance ratio: 69%.
* Diogo A. B. Fernandes, Miguel Neto, Liliana F. B. Soares, Mário M. Freire, and Pedro R. M. Inácio. On the Self-Similarity of Traffic Generated by Network Traffic Simulators. In Mohammad S. Obaidat, Faouzi Zarai, and Petros Nicopolitidis, editors, *Modeling and Simulation of Computer Networks and Systems*, pages 285–311. Elsevier, 2015.

For detailed insight about self-similarity and the Hurst Parameter, please refer to the following Ph.D. thesis:

* Pedro R. M. Inácio, Study of the Impact of Intensive Attacks in the Self-Similarity Degree of the Network Traffic in Intra-Domain Aggregation Points, Ph.D. Thesis, Ph.D. in Computer Science and Engineering, Department of Computer Science, University of Beira Interior, Covilhã, December, 2009. 