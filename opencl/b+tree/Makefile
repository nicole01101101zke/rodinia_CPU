include ../../common/make.config

#Can be changed by `make TYPE=CPU`
TYPE = GPU

#Library
ifeq ($(TYPE),GPU)
	OPENCL_INC = /usr/local/cuda/include
	OPENCL_LIB = /usr/local/cuda/lib64
else
	OPENCL_INC = $(INTEL_OPENCL_INC)
	OPENCL_LIB = $(INTEL_OPENCL_LIB)
endif

C_C = gcc
OCL_LIB = -lOpenCL

# ====================================================================================================100
#	NVCC
# ====================================================================================================100

# CUD_C = nvcc
# OMP_FLAG = 	-Xcompiler paste_one_here

# ====================================================================================================100
#	PGCC
# ====================================================================================================100

# C_C = pgcc
# OMP_FLAG = -mp
# ACC_FLAG = 	-ta=nvidia \
			# -Minfo \
			# -Mbounds

# ====================================================================================================100
#	END
# ====================================================================================================100

# ========================================================================================================================================================================================================200
#	EXECUTABLES (LINK OBJECTS TOGETHER INTO BINARY)
# ========================================================================================================================================================================================================200

b+tree.out:	./main.o \
		./kernel/kernel_gpu_opencl_wrapper.o \
		./kernel/kernel_gpu_opencl_wrapper_2.o \
		./util/timer/timer.o \
		./util/num/num.o \
		./util/opencl/opencl.o \
        ./timing.o
	$(C_C)	./main.o \
			./kernel/kernel_gpu_opencl_wrapper.o \
			./kernel/kernel_gpu_opencl_wrapper_2.o \
			./util/timer/timer.o \
			./util/num/num.o \
			./util/opencl/opencl.o \
            ./timing.o \
			-lm \
			-L$(OPENCL_LIB) \
			-lOpenCL \
	                -o b+tree.out

run: b+tree.out
	./b+tree.out file ../../data/b+tree/mil.txt command ../../data/b+tree/command.txt -p 0 -d 0

# ========================================================================================================================================================================================================200
#	OBJECTS (COMPILE SOURCE FILES INTO OBJECTS)
# ========================================================================================================================================================================================================200

# ======================================================================================================================================================150
#	MAIN FUNCTION
# ======================================================================================================================================================150

main.o:	./common.h \
		./main.h \
		./main.c
	$(C_C)	$(KERNEL_DIM) ./main.c \
			-c \
		    -I$(OPENCL_INC) \
			-o ./main.o \
			-Wno-unused-result \
			-O3

# ======================================================================================================================================================150
#	KERNELS
# ======================================================================================================================================================150

./kernel/kernel_gpu_opencl_wrapper.o:	./common.h \
										./kernel/kernel_gpu_opencl_wrapper.h \
										./kernel/kernel_gpu_opencl_wrapper.c
	$(C_C)	$(KERNEL_DIM) ./kernel/kernel_gpu_opencl_wrapper.c \
			-c \
			-o ./kernel/kernel_gpu_opencl_wrapper.o \
			-O3 \
			-I../util \
			-DTIMING \
			-I$(OPENCL_INC)

./kernel/kernel_gpu_opencl_wrapper_2.o:	./common.h \
										./kernel/kernel_gpu_opencl_wrapper_2.h \
										./kernel/kernel_gpu_opencl_wrapper_2.c
	$(C_C)	$(KERNEL_DIM) ./kernel/kernel_gpu_opencl_wrapper_2.c \
			-c \
			-o ./kernel/kernel_gpu_opencl_wrapper_2.o \
			-O3 \
			-I../util \
			-DTIMING \
			-I$(OPENCL_INC)

timing.o: ../util/timing.h ../util/timing.c
	$(C_C) ../util/timing.c -I../util -c -o timing.o -I$(OPENCL_INC)


# ======================================================================================================================================================150
#	UTILITIES
# ======================================================================================================================================================150

./util/timer/timer.o:	./common.h \
						./util/timer/timer.h \
						./util/timer/timer.c
	$(C_C)	./util/timer/timer.c \
			-c \
			-o ./util/timer/timer.o \
			-O3

./util/num/num.o:	./common.h \
					./util/num/num.h \
					./util/num/num.c
	$(C_C)	./util/num/num.c \
			-c \
			-o ./util/num/num.o \
			-O3

./util/opencl/opencl.o:	./common.h \
						./util/opencl/opencl.h \
						./util/opencl/opencl.c
	$(C_C)	./util/opencl/opencl.c \
			-c \
			-o ./util/opencl/opencl.o \
			-O3 \
			-I$(OPENCL_INC)

# ======================================================================================================================================================150
#	END
# ======================================================================================================================================================150

# ========================================================================================================================================================================================================200
#	DELETE
# ========================================================================================================================================================================================================200

clean:
	rm	*.o *.out \
		./kernel/*.o \
		./util/timer/*.o \
		./util/num/*.o \
		./util/opencl/*.o \
                output.txt 2>/dev/null || true
# ========================================================================================================================================================================================================200
#	END
# ========================================================================================================================================================================================================200
