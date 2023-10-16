<!--
Copyright 2023, OpenHW Group
SPDX-License-Identifier: Apache-2.0 WITH SHL-2.1
-->
## Regression Scripts
This directory contains a set of bash scripts to support the execution of various simulation regressions of the CVA6.
The regression scripts do three things:
1. Allow the user to select either Verilator or VCS as the simulator.
2. Allow the user to select one of two testbenches to a given test-program on.
  - Note that the UVM environment can only be compiled and simulated using VCS at this time.
3. Build any required tools and/or utilities such as Verilator, Spike and riscv-dv.
4. Run all tests listed in a set of `testlists` on the selected test-bench.

For example, once it has set-up all the required tools (if necessary) the `smoke-tests.sh` script will run the following:
```
python3 cva6.py --testlist=../tests/testlist_riscv-tests-cv64a6_imafdc_sv39-v.yaml --test rv64ui-v-add --iss_yaml cva6.yaml --target cv64a6_imafdc_sv39 --iss=$DV_SIMULATORS $DV_OPTS
python3 cva6.py --testlist=../tests/testlist_riscv-tests-cv64a6_imafdc_sv39-p.yaml --test rv64ui-p-add --iss_yaml cva6.yaml --target cv64a6_imafdc_sv39 --iss=$DV_SIMULATORS $DV_OPTS
python3 cva6.py --testlist=../tests/testlist_riscv-compliance-cv64a6_imafdc_sv39.yaml --test rv32i-I-ADD-01 --iss_yaml cva6.yaml --target cv64a6_imafdc_sv39 --iss=$DV_SIMULATORS $DV_OPTS
python3 cva6.py --testlist=../tests/testlist_riscv-arch-test-cv64a6_imafdc_sv39.yaml --test rv64i_m-add-01 --iss_yaml cva6.yaml --target cv64a6_imafdc_sv39 --iss=$DV_SIMULATORS $DV_OPTS  --linker=../tests/riscv-arch-test/riscv-target/spike/link.ld
python3 cva6.py --testlist=../tests/testlist_custom.yaml --test custom_test_template --iss_yaml cva6.yaml --target cv64a6_imafdc_sv39 --iss=$DV_SIMULATORS $DV_OPTS
python3 cva6.py --target cv64a6_imafdc_sv39 --iss=$DV_SIMULATORS --iss_yaml=cva6.yaml --c_tests ../tests/custom/hello_world/hello_world.c --gcc_opts "-g ../tests/custom/common/syscalls.c ../tests/custom/common/crt.S -I../tests/custom/env -I../tests/custom/common -T ../tests/custom/common/test.ld"
python3 cva6.py --testlist=../tests/testlist_riscv-compliance-cv32a60x.yaml --test rv32i-I-ADD-01 --iss_yaml cva6.yaml --target cv32a60x --iss=$DV_SIMULATORS $DV_OPTS
python3 cva6.py --testlist=../tests/testlist_riscv-tests-cv32a60x-p.yaml --test rv32ui-p-add --iss_yaml cva6.yaml --target cv32a60x --iss=$DV_SIMULATORS $DV_OPTS
python3 cva6.py --testlist=../tests/testlist_riscv-arch-test-cv32a60x.yaml --test rv32im-cadd-01 --iss_yaml cva6.yaml --target cv32a60x --iss=$DV_SIMULATORS $DV_OPTS  --linker=../tests/riscv-arch-test/riscv-target/spike/link.ld
python3 cva6.py --target cv32a60x --iss=$DV_SIMULATORS --iss_yaml=cva6.yaml --c_tests ../tests/custom/hello_world/hello_world.c --linker=../tests/custom/common/test.ld --gcc_opts "-g ../tests/custom/common/syscalls.c ../tests/custom/common/crt.S -lgcc -I../tests/custom/env -I../tests/custom/common"
```

### The Testbenches
The CVA6 has two distinct testbenches, known at `uvm` and `testharness`.

#### UVM
The UVM environment is based on the OpenHW Group CORE-V-VERIF verification environment.
Base layers of this environment can be found in the CVA6 repository (_this_ repo) under the `verif` directory.
The CVA6 UVM environment also uses a set of UVM library components from CORE-V-VERIF which is brought into your working copy as a git submodule.

#### Testharness
The "testharness" is a simulation environment that mimics the hardware setup of the Genesys2 FPGA development board.
The source code for this testbench is available in the CVA6 repository (_this_ repo) under the `corev_apu` directory.
Currently, the scriptware only supports VCS and Verilator for this testbench.
