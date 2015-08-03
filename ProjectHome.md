## Introduction ##

This software is used for educational purposes in order to teach how to implement the various Hidden Markov Model (HMM) algorithms in C and CUDA. It contains the necessary files to run the following algorithms:
  * Forward Algorithm (fo)
  * Viterbi Algorithm (vit)
  * Baum-Welch Algorithm (bwa)
Additionally, it contains various test files needed to test the algorithms in both C and CUDA along with timer functions so that the user can see the time difference between C and CUDA. It is not recommended to use the algorithms in a production environment as they have not been completely optimized.

Note - This code was originally based on the work done by Chuan Liu, which can be found here:

http://liuchuan.org/pub/cuHMM.pdf

http://code.google.com/p/chmm

## Installation ##
Prerequisites:
  * NVIDIA graphics card (CUDA-capable)
  * Linux (I used Ubuntu 10.04 32-bit)

1) Install the CUDA SDK
  * There are many guide on the Internet on how to do this. I recommend:
  * NVIDIA's site: http://developer.nvidia.com/cuda-downloads
  * My personal blog: http://sgmustadio.wordpress.com/2011/02/02/guide-to-installing-nvidia-cuda-sdk-on-linux/

2) Make a link to libcutil:
> $ cd ~/NVIDIA\_GPU\_Computing\_SDK/C/lib
> $ sudo ln -s libcutil\_i386.a libcutil.a
> > (Note: libcutil\_i386.a might be named differently depending on your
> > architecture)

3) Download and unzip the tar file

> $ tar xfvz hmm\_cuda\_v1.1.tar.gz
> $ cd hmm\_cuda\_v1.1

4) Change the CUDA SDK Path in the makefile
> $ nano Makefile
> Change the path of CUDA\_SDK\_PATH to reflect the CUDA SDK installation path
> Save and exit

5) Compile
> $ make
> All the executables will be placed into the ./build directory and any config
> files can be found in ./resources

6) Run a test program (you need to pass in HMM parameters from a config file)
> $ ./build/test\_fo\_cublas -c resources/config\_hmm\_2N3S.txt

7) Run a timer program (Note that the timer programs have set HMM parameters)
> $ ./build/time\_bwa\_cublas

## Using the Source Files ##

You can find the source code in ./src/main. This consists of forward, Viterbi, and Baum-Welch in both C and CUBLAS forms. A note on CUBLAS: CUBLAS is simply a CUDA-enabled version of BLAS, so many of the optimizations are already done. You just need to make sure your makefile contains the extra links to the cublas libraries.

To use any of the algorithms, you need link to the following:
  * hmm.h
  * hmm.c

If you are using any of the testing functions (like, printing to the console),
you need:
  * hmm\_test.h
  * hmm\_test.c

So, to use the CUBLAS version of the Viterbi algorithm (for example), you need
to include:
  * hmm.h
  * hmm\_vit\_cublas.h
Then, fill out the appropriate fields in the hmm struct and obs struct. Allocate a state\_sequence array and pass pointers to all three to the run\_hmm\_vit() function:

> run\_hmm\_vit(in\_hmm, in\_obs, state\_seq);

Magic will happen, and the most likely state sequence will appear in state\_seq. Note that which version of hmm\_vit (_c vs._cublas) you include will determine how the Viterbi is run (CPU vs. GPU). The other algorithms are called in a similar manner.

## More Information ##

This code served as the basis for my thesis, which can be found here:

http://scholar.lib.vt.edu/theses/available/etd-12082011-204951/

Additionally, you can follow my other works on my blog:

http://shawnhymel.com/