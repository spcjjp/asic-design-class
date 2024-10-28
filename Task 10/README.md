<details>
<summary>Static Timing Analysis for a Synthesized RISC-V Core with OpenSTA </summary>
<br>

### openSTA
```c
 cd
sudo apt-get install cmake clang gcc tcl swig bison flex
git clone https://github.com/parallaxsw/OpenSTA.git
cd OpenSTA
cmake -DCUDD_DIR=/home/likith/cudd-3.0.0
make
app/sta
```
![image](https://github.com/user-attachments/assets/143fbbec-b471-4467-b925-608e87655e20)

