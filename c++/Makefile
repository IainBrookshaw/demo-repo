# Demo Repo: C++ Makefile
# Copyright (c) 2019, Futurewei Inc. All Rights Reserved
# Iain Brookshaw

TARGET = hello_world
SRC    = ./src

CXXFLAGS = -std=c++0x

$(TARGET): hello_world.o
	$(CXX) $(CXXFLAGS) hello_world.o -o $(TARGET)

hello_world.o: $(SRC)/hello_world.cpp
	@echo "src = $(SRC)"
	$(CXX) $(CXXFLAGS) -c $(SRC)/hello_world.cpp