include ../../common/make.config
#===============================================================================
# User Options
#===============================================================================

# Compiler can be set below, or via environment variable
OPTIMIZE  = yes
DEBUG     = no
ARCH      = sm_60

#===============================================================================
# Program name & source code list
#===============================================================================

program = SobolQRNG

source = sobol.cu sobol_gold.cu sobol_gpu.cu sobol_primitives.cu

obj = $(source:.cu=.o)

#===============================================================================
# Sets Flags
#===============================================================================

# Standard Flags
HEC_NVCC_FLAGS := $(NVCC_FLAGS) -std=c++14

# Linker Flags
LINKER_FLAGS += 

# Debug Flags
ifeq ($(DEBUG),yes)
  HEC_NVCC_FLAGS += -g
  LINKER_FLAGS  += -g
endif

# Optimization Flags
ifeq ($(OPTIMIZE),yes)
  HEC_NVCC_FLAGS += 
endif

$(program): $(obj) 
	$(LINKER) $(obj) -o $@ $(LINKER_FLAGS)

sobol.o:sobol.cu
	$(NVCC) $(HEC_NVCC_FLAGS) -c $< -o $@

sobol_gold.o:sobol_gold.cu
	$(NVCC) $(HEC_NVCC_FLAGS) -c $< -o $@

sobol_gpu.o:sobol_gpu.cu
	$(NVCC) $(HEC_NVCC_FLAGS) -c $< -o $@

sobol_primitives.o:sobol_primitives.cu
	$(NVCC) $(HEC_NVCC_FLAGS) -c $< -o $@

clean:
	rm -rf $(program) $(obj)

run: $(program)
	./$(program) 1000000 1000 100
run_no_build:
	./$(program) 1000000 1000 100
