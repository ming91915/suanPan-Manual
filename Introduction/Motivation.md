Motivation
==========

As majored in civil engineering, I started to learn finite element method (FEM) and model some simple, general solid mechanics problems using popular FEM packages like ANSYS and ABAQUS during my undergraduate study. At that time, I also tried some other modeling/designing tools such as SAP2000, ETABS, MSC softwares, and some simple CAD integrated tools. Later I got in touch with OpenSees, a popular platform for simulations in seismic engineering. It is indeed a tiny yet powerful application. Due to my personal needs, I started to study the source codes of OpenSees and try to write my own elements within the framework. As it is open source, there is no license issue.

The framework of OpenSees was initially designed in 1990's. Due to the limitations of CPUs at that time, the parallel (distributed) computation was implemented by MPI in OpenSees. With the development of CPU, now we have higher core frequency and more cores/threads, it is necessary to write some new tools to exploit the performance of modern CPUs.

With the release of new language standards (C++11, C++14 and the coming C++17 and C++20), now C++ is a super powerful language with all kinds of handy tools available.

**suanPan** is designed based on the new language standards, aims to provide a high performance computation platform for FEM simulation.
