# u50_xdma design 
Since the U50 Vivado flow is available, folks from hardare may be interested in u50 xdma design with Vivado flow. Here is the same example of XDMA and HBM to demostrate how to do data DMA from HBM and host memory.

# Build the bitstream and MCS

## Environment requirements
* Ubuntu 22.04
* Vivado 23.2
* Required licenses of used IPs

## Build command
```
$git clone https://github.com/gaojon/u50_xdma
$cd u50_xdma/hw
$make
```
This will lead to Vivado GUI to run through synthesis, place and route to generate bitstream and MCS flash file which you could program the flash later.

## Program the flash

u50 has one area of flash space which has been written proected. The new flash program need to offset to 0x01002000. The MCS file generated has already applied this requirement. You could use hardare manager GUI to program the flash on U50 as following guideline. 

https://docs.amd.com/r/en-US/ug1371-u50-reconfig-accel/MCS-File-Generation-and-Alveo-Card-Programming

* Select Add Configuration Device and select the mt25qu01g-spi-x1_x2_x4 part
* After programming has completed, disconnect the card in the hardware manager, and disconnect the JTAG programming cable from the Alveo accelerator card.
* Perform a cold reboot on the host machine to complete the card update


# Driver and testing on host

Here is the details of how to compile and run the test with XDMA drivers

https://github.com/Xilinx/dma_ip_drivers/tree/master/XDMA/linux-kernel

```
$git clone https://github.com/Xilinx/dma_ip_drivers
$cd dma_ip_drivers/XDMA/linux-kernel/xdma
$sudo make install
$cd ../tools
$make

$sudo modprobe xdma
$cd ../tests

$sudo ./load_driver.sh
$sudo  ./run_test.sh

```