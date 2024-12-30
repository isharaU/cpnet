# Paths and Environment Variables
CUDA_HOME := /usr/local/cuda-11.8  # Update this to match your CUDA version

TF_INC = $(shell python3 -c "import tensorflow as tf; print(tf.sysconfig.get_include())")
TF_LIB = $(shell python3 -c "import tensorflow as tf; print(tf.sysconfig.get_lib())")

LIBFLAGS = -L$(CUDA_HOME)/lib64 -lcudart -L$(TF_LIB) -ltensorflow_framework -lcublas

ARCH = sm_75  # Update this based on your GPU architecture
NVCC = $(CUDA_HOME)/bin/nvcc
CXX = g++

LD_LIBRARY_PATH := $(CUDA_HOME)/lib64

# Compiler Flags
CXXFLAGS = -std=c++11 -shared -fPIC -O2 -D_GLIBCXX_USE_CXX11_ABI=0 -I$(TF_INC)
NVCCFLAGS = -std=c++11 -DGOOGLE_CUDA=1 -x cu -Xcompiler -fPIC -arch=$(ARCH) --use_fast_math

# Targets
all: knn_op.so 

knn_op.so: knn_op.cc knn_cuda.cu.o
	$(CXX) $(CXXFLAGS) -o $@ knn_op.cc knn_cuda.cu.o $(LIBFLAGS)

knn_cuda.cu.o: knn_cuda.cu.cc
	$(NVCC) $(NVCCFLAGS) -o $@ $< -I$(TF_INC)

clean:
	rm -f *.o *.so
