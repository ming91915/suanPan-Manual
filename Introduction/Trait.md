Program Attributions and Dependency
===================================

Linear Algebra
--------------

suanPan is an FEM platform. The finite element method itself is one of numerical methods developed atop matrix. Hence linear algebra plays a vital role in the design of the framework.

The basic linear algebra operations/manipulations are fulfilled by the excellent [Armadillo](http://arma.sourceforge.net/) library. The syntax is very flexible and very similar to natural semantics. Anyone who is familiar with MATLAB can almost instantly master Armadillo.

As of 2017, Armadillo only supports two matrix types: dense (full storage) and sparse (CSC). They are not sufficient for the application of FEM. I thus wrote new containers to hold all global stiffness matrices with different storage schemes. The solver wrappers are also provided.

Database Storage
----------------

All suanPan outputs are stored in HDF5 format, which is based on [HDF5](https://support.hdfgroup.org/HDF5/).

Pre-Process
-----------

suanPan has no intent to develop any pre-processing functions. It only analyze problems with the given mesh, that's all. Users have to generate the mesh with other tools.

However, here I can recommend you a fantastic tool [Gmesh](http://gmsh.info/).

Post-Process
------------

suanPan supports to output all model data and store it in HDF5 format. Practically, there is almost no limit on the size and the step of the model. If simple plots like displacement v.s. load curves are required. Any handy tools can be used. To visualize the result. Users can use [Paraview](Paraview) to conduct some complicated post-processing.
