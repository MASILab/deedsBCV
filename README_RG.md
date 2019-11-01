If you change source code applyLinearBCVfloat.cpp or applyLinearBCV.cpp, please compile via following command

g++ src/applyLinearBCVfloat.cpp -I src -lz -o applyLinearBCVfloat -std=c++11 -O3 -fopenmp -msse4a
g++ src/applyLinearBCV.cpp -I src -lz -o applyLinearBCV -std=c++11 -O3 -fopenmp -msse4a
