# cmpe283-hw2

Team: Gabrielle Viray (012340068)

## Initial Steps:
    1. Make sure Linux git repository is cloned.
    2. Building the Kernel
        a.
        ```sudo bash```
        b.
        ```apt-get install build-essential kernel-package fakeroot libncurses5-dev libssl-dev ccache bison flex libelf-dev ```
        c. 
        ```uname -a```
        d. 
        ``` cp /boot/config-5.8.0-50-generic ./.config```
        e.
        ``` make oldconfig```
        f.
        ```
        make && make modules && make install && make modules-install```
        g.
        ```
        reboot
        ```
    3. Verify newer kernel after reboot using:
        ```uname -a```
    
## Implementation:
      1.
