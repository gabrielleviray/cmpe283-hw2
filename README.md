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
  c. Note down Kernel Version after the command:
  ```
  uname -a
  ```
  d. Substitute version obtained in Step c(previous step) into the following command:
  ```
  cp /boot/config-5.8.0-50-generic ./.config
  ```
  e. Hold down enter for default settings while running the command:
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

1. Modified ```int kvm_emulate_cpuid(struct kvm_vcpu *vcpu)``` function in cpuid.c file:
```
int kvm_emulate_cpuid(struct kvm_vcpu *vcpu)
{
	u32 eax, ebx, ecx, edx;

	if (cpuid_fault_enabled(vcpu) && !kvm_require_cpl(vcpu, 0))
		return 1;

	eax = kvm_rax_read(vcpu);
	ecx = kvm_rcx_read(vcpu);

	// #######################################################
	// #                                                     #
	// #           CMPE 283 CPUID leaf function              #
	// #                                                     # 
	// #######################################################
	if (eax == 0x4FFFFFFF){

		pr_info("Leaf function");
		kvm_cpuid(vcpu, &eax, &ebx, &ecx, &edx, true);
		
		// return the total number of exits(all types) in %eax
		eax = atomic64_read(&exit_counter_value);
		pr_info("Total time spent processing all exits in eax=%u", eax);	

		// return the high 32 bits of the total time spent processing all exits in %ebx
		ebx = (atomic64_read(&exit_duration_value) >> 32);
		pr_info("Total time spent processing all exits in ebx=%u", ebx);	

		// return the low 32 bits of the total time spent processing all exits i n %ecx
		ecx = (atomic64_read(&exit_duration_value) & 0xFFFFFFFF);
		pr_info("Total time spent processing all exits in ecx=%u", ecx);

	} else {

		kvm_cpuid(vcpu, &eax, &ebx, &ecx, &edx, false);
		
	}

	kvm_rax_write(vcpu, eax);
	kvm_rbx_write(vcpu, ebx);
	kvm_rcx_write(vcpu, ecx);
	kvm_rdx_write(vcpu, edx);
	return kvm_skip_emulated_instruction(vcpu);
}
```
2. Compute the total number of exits and duration of exits in vmx.c ( Changes in vmx.c have a comment of '/* CMPE 283 Assignment2 */' )
3. Repeat steps 1a, 1f in 'Initial Steps' Section to rebuild kernel again.

### Nested VM

1. Installed virtual manager:
```
sudo apt-get install virt-manager
```
2. Download Ubuntu ISO file and create a nested virtual machine using virtual manager.
3. Install cpuid
```
sudo apt-get install cpuid
```
### Testing 
5. Build and execute test.c to see exit count and exit duration:
```
gcc test.c
```
```
./a.out
```
6. View register values
```
cpuid -l 0x4FFFFFFF
```

### Question 1: Comment on the frequency of exits – does the number of exits increase at a stable rate? Or are there more exits performed during certain VM operations?  

There isn't a stable rate at which the number of exits increases. More exits are performed during other VM operations such as ept violations and MSR exit reasons, I/O instruction, etc.


### Question 2:  Approximately how many exits does a full VM boot entail? 

A full VM boot entails roughly 347300 exits.


### Reference Links
- https://help.ubuntu.com/community/KVM/VirtManager
- https://ubuntu.com/#download




