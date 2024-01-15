all: OpenCL/leukocyte 

OpenCL/leukocyte:
	cd OpenCL; make
	mv OpenCL/leukocyte.out .

run:
	./leukocyte.out ../../data/leukocyte/testfile.avi 100

clean:
	cd OpenCL; make clean
	rm -f meschach_lib/meschach.a
	rm -f leukocyte.out
