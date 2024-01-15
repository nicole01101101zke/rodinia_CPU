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

#C compiler
CC = g++

FLAGS = -O3
SRC = euler3d.cpp ../util/timing.c

EXE = euler3d.out

release:$(SRC)
	$(CC) $(KERNEL_DIM) $(SRC) -o $(EXE) -I$(OPENCL_INC) -L$(OPENCL_LIB) -lOpenCL -I../util -DTIMING $(FLAGS)

errmsg:$(SRC)
	$(CC) $(KERNEL_DIM) $(SRC) -o $(EXE) -I$(OPENCL_INC) -L$(OPENCL_LIB) -lOpenCL -D ERRMSG $(FLAGS)

ptx:$(SRC)
	$(CC) $(KERNEL_DIM) $(SRC) -o $(EXE) -I$(OPENCL_INC) -L$(OPENCL_LIB)  -lOpenCL -D PTX_MSG $(FLAGS)

res:$(SRC)
	$(CC) $(KERNEL_DIM) $(SRC) -o $(EXE) -I$(OPENCL_INC) -L$(OPENCL_LIB)  -lOpenCL -D RES_MSG $(FLAGS)

run:$(EXE)
	./$(EXE) ../../data/cfd/fvcorr.domn.097K -t gpu -d 0

clean: $(SRC)
	rm -f $(EXE) $(EXE).linkinfo result*
