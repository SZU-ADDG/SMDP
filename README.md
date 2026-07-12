<div align="center">

# Synchronized Membrane Potential-driven Plasticity for Spiking Neural Networks

MATLAB implementation accompanying the paper accepted by<br>
**IEEE Transactions on Cognitive and Developmental Systems**

Junkai Ji, Haochang Jin, Lijia Ma, Qiuzhen Lin, Ka-Chun Wong, Zexuan Zhu, and Jianqiang Li

![MATLAB](https://img.shields.io/badge/MATLAB-R2018a-orange)
![Task](https://img.shields.io/badge/task-spiking_neural_networks-blue)
![Repository](https://img.shields.io/badge/repository-public-brightgreen)

</div>

## Overview

Synchronized Membrane Potential-driven Plasticity (SMDP) is a supervised learning algorithm for biologically plausible spiking neural networks (SNNs). It addresses two limitations of earlier membrane potential-driven aggregate-label learning methods: binary spikes carry limited information, and modifying only one spike per learning step leads to expensive training.

Inspired by biological burst firing, SMDP assigns a coefficient to every spike event. A strongly activated neuron can therefore emit multiple spikes simultaneously. During learning, SMDP can also adjust several membrane-potential points in one update, improving convergence speed and enabling multilayer SNNs to handle temporal credit-assignment and classification tasks.

## Highlights

- **Spike coefficients:** each event can represent one or more simultaneous spikes; the coefficient is computed directly from the membrane potential and firing threshold.
- **Synchronized updates:** multiple missing or redundant spikes can be corrected in a single learning step.
- **Multilayer learning:** gradients are propagated through fully connected hidden layers while retaining the full temporal dynamics of leaky integrate-and-fire neurons.
- **Broad evaluation:** experiments cover temporal credit assignment, three UCI datasets, MNIST, and Fashion-MNIST.
- **CPU implementation:** the reference experiments were implemented in MATLAB and do not require GPU-specific code.

## Model

<p align="center">
  <img src="images/model.png" width="95%" alt="Multilayer SMDP network, modified LIF neuron, membrane potential, and burst firing">
</p>

<p align="center"><em>
The fully connected multilayer SNN uses modified leaky integrate-and-fire neurons. Dense input can produce a burst event whose spike coefficient is greater than one.
</em></p>

For neuron $j$, the coefficient of an output spike event at time $\tilde{t}_j^k$ is determined by

$$
\tilde{C}_j^k = \left\lfloor \frac{V_j(\tilde{t}_j^k)}{\theta} \right\rfloor,
$$

where $V_j$ is the membrane potential and $\theta$ is the firing threshold. The coefficient is calculated from the neuron state and is not an additional learnable parameter.

When the actual spike count differs from the desired aggregate label, SMDP selects multiple subthreshold maxima or redundant spike events and minimizes their distance to the firing threshold. If the discrepancy contains $N$ spikes, a conventional one-point update requires approximately $O(NK)$ work under the paper's analysis. Updating $M$ points together reduces this to approximately $O(NK/M)$, with further empirical gains from aggregating their gradients.

## Main results

The paper reports mean test accuracy over 20 independent runs. Multilayer SMDP achieved the following results:

| Dataset | Architecture | Test accuracy |
| --- | ---: | ---: |
| Breast Cancer | 135-360-2 | **97.6 +/- 0.8%** |
| Liver Disorders | 150-360-2 | **66.9 +/- 2.9%** |
| Ionosphere | 231-360-2 | **93.5 +/- 1.6%** |
| MNIST | 784-800-10 | **98.5 +/- 0.3%** |
| Fashion-MNIST | 784-800-10 | **88.9 +/- 1.0%** |

The ablation study also found that spike coefficients improved performance on all five classification datasets. In the efficiency study, SMDP required the fewest epochs and the least runtime among SMDP, TDP, TDP-DL, Delay FE, and MPD-AL as the target spike-count gap increased.

### Temporal credit assignment

<p align="center">
  <img src="images/SMDP.png" width="82%" alt="Temporal credit-assignment results produced by SMDP">
</p>

<p align="center"><em>
SMDP learns to locate predictive clues embedded in background spike activity and responds with the desired number of output spikes.
</em></p>

## Repository structure

| Path | Contents |
| --- | --- |
| `single-MNIST/` | Single-layer SMDP experiments on MNIST |
| `muti-MNIST/` | Multilayer SMDP experiments on MNIST |
| `single-fashionMNIST/` | Single-layer SMDP experiments on Fashion-MNIST |
| `muti-fashionMNIST/` | Multilayer SMDP experiments on Fashion-MNIST |
| `single-UCI/` | Single-layer Breast Cancer, Liver Disorders, and Ionosphere experiments |
| `multi-UCI/` | Multilayer Liver Disorders and Ionosphere experiments |
| `images/` | Figures used in this README |

The original `muti-*` directory names and `*_muti_experiment.m` filenames are intentionally retained because the MATLAB scripts use relative paths.

## Requirements

The paper used the following environment:

- MATLAB R2018a
- Statistics and Machine Learning Toolbox, for functions including `normrnd` and `unifrnd`
- 13th Gen Intel Core i7-1360P CPU at up to 5.0 GHz

Other recent MATLAB versions should generally work, though they have not been validated as part of this repository release.

## Data

The repository includes:

- preprocessed MNIST data in `single-MNIST/MNIST.mat` and `muti-MNIST/MNIST.mat`;
- encoded UCI data in the corresponding `single-UCI/` and `multi-UCI/` directories;
- selected experiment result files from the original code package.

The Fashion-MNIST scripts expect `FashionMNIST.mat` in each Fashion-MNIST experiment directory. This file is currently absent. It should define:

- `train_pattern`: training images arranged by column;
- `train_labels`: training labels;
- `test_pattern`: test images arranged by column;
- `test_labels`: test labels.

## Running the code

Clone the repository and open MATLAB:

```bash
git clone https://github.com/SZU-ADDG/SMDP.git
cd SMDP
```

Run each experiment from its own directory so that relative data and helper-function paths resolve correctly. For example:

```matlab
cd('single-MNIST')
rng(1)                         % optional reproducible initialization
single_MNIST_experiment
```

Main entry points:

| Experiment | Entry point |
| --- | --- |
| Single-layer MNIST | `single-MNIST/single_MNIST_experiment.m` |
| Multilayer MNIST | `muti-MNIST/muti_MNIST_experiment.m` |
| Single-layer Fashion-MNIST | `single-fashionMNIST/single_FashionMNIST_experiment.m` |
| Multilayer Fashion-MNIST | `muti-fashionMNIST/muti_fa_mnist_experiment.m` |
| Single-layer Liver Disorders | `single-UCI/bupa_single_experiment.m` |
| Single-layer Ionosphere | `single-UCI/iono_single_experiment.m` |
| Single-layer Breast Cancer | `single-UCI/wbc_single_experiment.m` |
| Multilayer Liver Disorders | `multi-UCI/bupa_muti_experiment.m` |
| Multilayer Ionosphere | `multi-UCI/iono_muti_experiment.m` |

Learning rates, network sizes, epoch counts, and trial counts are defined near the top of each entry-point script. Check these values against the paper protocol before running a full reproduction, because the released scripts contain editable experiment presets.

## Reproducibility notes

- Set the MATLAB random seed before training to make weight initialization, sample ordering, dropout, and injected noise reproducible.
- The paper reports results from 20 independent runs; a single run should not be treated as the reported mean.
- MNIST and Fashion-MNIST use latency coding. UCI features use Gaussian receptive-field population coding.
- The target output neuron is trained to emit the desired aggregate spike count, while non-target output neurons remain silent.
- Paper experiments use dropout with probability 0.1 and Gaussian input noise with standard deviation 0.05 for MNIST and Fashion-MNIST.

## Citation

The final DOI, volume, issue, pages, and publication year will be added after the IEEE bibliographic record becomes available. Until then, please cite the accepted paper using the following metadata:

```bibtex
@article{ji_smdp,
  author  = {Junkai Ji and Haochang Jin and Lijia Ma and Qiuzhen Lin and
             Ka-Chun Wong and Zexuan Zhu and Jianqiang Li},
  title   = {Synchronized Membrane Potential-driven Plasticity for Spiking
             Neural Networks},
  journal = {IEEE Transactions on Cognitive and Developmental Systems},
  note    = {Accepted for publication}
}
```

## Acknowledgments

This work was supported by the National Natural Science Foundation of China under Grant 62476177 and the Shenzhen Science and Technology Program under Grant JCYJ20220531101614031.

## License

No open-source license has been specified for this release. All rights are reserved by the copyright holders unless a license is added later.
