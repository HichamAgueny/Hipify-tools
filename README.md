# Translating GPU-accelerated applications

We present different tools to translate CUDA-based codes to target various GPU (Graphics Processing Unit) architectures (e.g. AMD and Intel GPUs). A special focus will be to cover the following tools: (i) [`hipify`](https://docs.amd.com/en-US/bundle/HIPify-Reference-Guide-v5.1/page/HIPify.html); (ii) [`syclomatic`](https://www.intel.com/content/www/us/en/developer/articles/technical/syclomatic-new-cuda-to-sycl-code-migration-tool.html#gs.o5pj6f), and (iii) [`clacc`](https://csmd.ornl.gov/project/clacc). These tools have been tested on the supercomputer [LUMI-G](https://lumi-supercomputer.eu/lumi_supercomputer/) in which the GPU partitions are of type [AMD MI250X GPU](https://www.amd.com/en/products/server-accelerators/instinct-mi250x).

The aim of this document is to guide users through a straightforward procedure for converting CUDA-based codes to other GPU-programming models, mainly, HIP, SYCL and OpenMP offloading. By the end of this document, we expect users to learn about:

- How to use the `hipify-perl` and `hipify-clang` tools to translate CUDA sources to HIP sources.
- How to use the `syclomatic` and `DPC++` tools to convert CUDA source to SYCL.
- How to use the `clacc` tool to convert OpenACC application to OpenMP offloading.
- How to compile the generated HIP, SYCL and OpenMP applications.

## Hipify 

In this section, we describe how to use `hipify-perl` and `hipify-clang` tools to translate a CUDA application to HIP.

### Hipify-perl

`hipify-perl` is 

- [ ] Step 1: Loading modules

On LUMI-G, the following modules need to be loaded:
- `module load CrayEnv`
- `module load rocm`
- `module load LUMI/21.12  partition/G`

- [ ] Step 2: Running hipify-perl

- To generate hipify-perl run the command `hipify-clang --perl`
- To convert a CUDA code to HIP code: `perl hipify-perl program.cu > program.cu.hip`
- To compile with `hipcc`: `hipcc --offload-arch=gfx90a -o program_hip program.cu.hip` 

### Hipify-clang

document guides users to the use of hipify tools.

Details about how to build `hipify-clang` can be found [here](https://github.com/ROCm-Developer-Tools/HIPIFY). Note that `hipify-clang` is available on LUMI-G. The issue is related to the installation of CUDA-toolkit.

- [ ] Step 2: test `hipify-clang`

Here you develop the necessary steps to run `hipify-clang` and compare the output with the one in the previous step.

Here are steps to run `hipify-clang` with CUDA container

- Step0: load a rocm module
`ml rocm`

- Step1: pull a cuda singularity container [cuda-11.3 lacks a library and cuda-12 lacks compatibility]
`singularity pull docker://nvcr.io/nvidia/cuda:11.4`

- Step2: launch the container

`singularity shell -B $PWD,/opt:/opt cuda_11.4.0-devel-ubuntu20.04.sif`

- Step3: run `hipify-clang`

`/opt/rocm-5.0.2/bin/hipify-clang program.cu --cuda-path=/usr/local/cuda-11.4 -I /usr/local/cuda-11.4/include`

This generates the converted hip code `program.cu.hip`

- Step4: compile the generated code. BUT first load the required module
`hipcc --offload-arch=gfx90a -o exec.hip program.cu.hip`

- Step5: submit a job to check the result

Some refs.

- https://github.com/ROCm-Developer-Tools/HIPIFY

- https://olcf.ornl.gov/wp-content/uploads/hip_for_cuda_programmers_slides.pdf

- https://github.com/olcf-tutorials/simple_HIP_examples/tree/master/vector_addition

- https://www.admin-magazine.com/HPC/Articles/Porting-CUDA-to-HIP

https://docs.amd.com/en-US/bundle/HIPify-Reference-Guide-v5.1/page/HIPify.html

## Syclomatic

## Clacc

# Conclusion

We have presented an overview of the usage of available tools to convert CUDA-based applications to HIP, SYCL and OpenMP offloading (for OpenACC C source). In general the translation process for large applications covers about 80-90% of the source code and thus requires manual modification to complet the porting application. It is however worth noting that the accuracy of the translation process requires that applications are written correctly according to the cuda syntax. 

