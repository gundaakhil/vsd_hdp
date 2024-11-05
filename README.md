# VLSI System Design - Hardware Design Program

<details>

 <summary>Day 0 - Tools Installation</summary>

## System Details

- **RAM**: 8GB
- **Storage**: 256GB SSD
- **OS**: macOS

## Yosys

```
$ git clone https://github.com/YosysHQ/yosys.git
$ brew install cmake gcc gawk tcl-tk libtool bison flex make
$ brew install graphviz
$ cd yosys
$ git submodule update --init
$ make
$ yosys --version
```
```
$ sudo apt-get update
$ git clone https://github.com/YosysHQ/yosys.git
$ cd yosys
$ sudo apt install make (If make is not installed please install it) 
$ sudo apt-get install build-essential clang bison flex \
    libreadline-dev gawk tcl-dev libffi-dev git \
    graphviz xdot pkg-config python3 libboost-system-dev \
    libboost-python-dev libboost-filesystem-dev zlib1g-dev
$ make config-gcc
$ make 
$ sudo make install
```

Getting Some Issue - Need to show

## Iverilog

```
brew install icarus-verilog
iverilog
```
![iverilog](/images/Day0/iverilog.png)

## GTKWave

```
brew install gtkwave
gtkwave
```
![gtkwave](/images/Day0/gtkwave.png)

## Introduction

</details>
