# *********************************************************************************************
# UVMF library directory:
# This variable points to the UVMF release where uvmf_base_pkg directory resides.
# This variable points to release code that is not user modified.
# This variable allows for UVMF release directories to reside independent of project related verification_ip and project_benches directories.
# This code below looks "upward" for directory starting with UVMF_* and returns first match for use with the release examples.
UVMF_HOME ?= $(firstword $(wildcard $(addsuffix /UVMF_*,. .. ../.. ../../.. ../../../.. ../../../../..)))

# Project(s) specific verification_ip library:
# Directory where reusable verification packages for interfaces, environments, utilities, etc. reside.
# This variable allows for verification_ip to reside independent of project bench and UVMF release directories.
# For examples deployed with UVMF this will be $(UVMF_HOME)/<example_group>/verification_ip
export UVMF_VIP_LIBRARY_HOME ?= $(UVMF_HOME)/base_examples/verification_ip

# Project specific bench:
# Directory where bench specific code is located.
# This variable allows for project_benches to reside independent of verification_ip and UVMF release directories.
# For examples deployed with UVMF this will be $(UVMF_HOME)/<example_group>/project_benches/<example_bench>
export UVMF_PROJECT_DIR ?= $(UVMF_HOME)/base_examples/project_benches/gpio_example

#
# *********************************************************************************************
# Set test case specific Variables
TEST_NAME            = test_top
TEST_SEED            = random
TEST_VERBOSITY       = UVM_HIGH
UVM_CLI_ARGS         = 


# Usage of Veloce, etc. to be input by the user (subject to defaults)
USE_VELOCE          ?= 0
USE_LEGACY_TBX_FLOW ?= 0

# Include Makefile that includes targets for UVM Framework packages
include $(UVMF_HOME)/scripts/Makefile

include $(UVMF_HOME)/base_examples/verification_ip/interface_packages/gpio_pkg/Makefile
include $(UVMF_HOME)/base_examples/verification_ip/environment_packages/gpio_example_env_pkg/Makefile

# Add to default compile/load/run arguments
VCOM_ARGS            += 
VLOG_ARGS            +=
VELANALYZE_ARGS      += 
VELANALYZE_HVL_ARGS  += 

BATCH_VOPT_ARGS      +=
DEBUG_VOPT_ARSG      +=

COMMON_VSIM_ARGS     +=
BATCH_VSIM_ARGS      += -permit_unmatched_virtual_intf
DEBUG_VSIM_ARGS      += -permit_unmatched_virtual_intf

# Do commands
DEBUG_DO_COMMANDS = -do "run 0; do wave.do; run -all"

# Project bench package source
GPIO_EXAMPLE_PARAMETERS_PKG =\
+incdir+$(UVMF_PROJECT_DIR)/tb/parameters \
$(UVMF_PROJECT_DIR)/tb/parameters/gpio_example_parameters_pkg.sv

GPIO_EXAMPLE_SEQUENCES_PKG =\
+incdir+$(UVMF_PROJECT_DIR)/tb/sequences \
$(UVMF_PROJECT_DIR)/tb/sequences/gpio_example_sequences_pkg.sv

GPIO_EXAMPLE_TEST_PKG =\
+incdir+$(UVMF_PROJECT_DIR)/tb/tests \
$(UVMF_PROJECT_DIR)/tb/tests/gpio_example_test_pkg.sv

.PHONY: all build common wrappers test run clean 

# Project bench package targets
COMP_GPIO_PARAMETERS_PKG_TGT_0 = q_comp_gpio_example_parameters_pkg
COMP_GPIO_PARAMETERS_PKG_TGT_1 = v_comp_gpio_example_parameters_pkg
COMP_GPIO_PARAMETERS_PKG_TGT = $(COMP_GPIO_PARAMETERS_PKG_TGT_$(USE_VELOCE))

comp_gpio_example_parameters_pkg: $(COMP_GPIO_PARAMETERS_PKG_TGT)

q_comp_gpio_example_parameters_pkg:
	$(HVL_COMP_CMD) $(GPIO_EXAMPLE_PARAMETERS_PKG)

v_comp_gpio_example_parameters_pkg: q_comp_gpio_example_parameters_pkg
	$(HDL_COMP_CMD) $(GPIO_EXAMPLE_PARAMETERS_PKG)

comp_gpio_example_sequence_pkg:
	$(HVL_COMP_CMD) $(GPIO_EXAMPLE_SEQUENCES_PKG)

comp_gpio_example_test_pkg:
	$(HVL_COMP_CMD) $(GPIO_EXAMPLE_TEST_PKG)

comp_gpio_example_dut:
	$(HDL_COMP_CMD) $(GPIO_DUT)

build: comp_uvm_pkg comp_fli_pkg comp_uvmf_base_pkg comp_gpio_pkg comp_gpio_example_env_pkg comp_gpio_example_parameters_pkg comp_gpio_example_sequence_pkg comp_gpio_example_test_pkg comp_test_bench

