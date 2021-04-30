# CMPE 283: Assignment 2

Team: Gabrielle Viray (012340068)

## Steps to Complete the Assignment

### Initial Steps
1. Make sure Linux git repository is cloned.
2. Building the Kernel<br>
  a. Root user mode
  ```
  sudo bash
  ```
  b. Install
  ```
  apt-get install build-essential kernel-package fakeroot libncurses5-dev libssl-dev ccache bison flex libelf-dev 
  ```  
  c. Note down Kernel Version
  ```
  uname -a
  ```
  d. 
  ```
  cp /boot/config-5.8.0-50-generic ./.config
  ```
  e. Hold down enter for default settings
  ```
  make oldconfig
  ```
  f. Make
  ```
  sudo make -j
  ```
  ```
  sudo make modules && sudo make install && sudo make modules install
  ```
  g. Reboot
  ```
  reboot
  ```
    
3. Verify newer kernel after reboot using: 
  ```
  uname -a
  ```
    
### Implementation

1. Modified CPUID emulation code in ** cpuid.c**:

