### ParallelECG_Embedded
======================
To transplant the paralleling program of computer simulation of electrocardiogram(ECG) from PC/Server based on x86 to TK1/TX1 based on ARM CPU-Nvidia GPU integrated architecture, I make this project.

#I want to make some following changes:

1. To simplify the raw program beacause of too many codes were commented out and ambiguous code comments.
2. To optimize the raw code to fit the integrated architecture of embedded System on Board(SoC) which has the CPU-GPU unified memory instead of traditional way of memory copy.
3. To improve the raw algorithm in this program to fit the embedded SoC and i hope that the performance could hold the line to ordinary PCs.
4. To implement a high energy efficient parallel electrocardiogram on embedded SoC like TK1 or TX1, and to miniature the device of simulating computing of ECGs.
