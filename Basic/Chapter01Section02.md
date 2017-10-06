Input File
==========

It is not very efficient to develop models in CLI mode, people prefer something similar to scripts or batch files. As like many other FEM packages, suanPan also allows users to define models with input files.

Encoding and Convention
-----------------------

The input files are no more than plain texts. Comments are supported within the input file. Any line starts with the sharp symbol `#` indicates a comment line. There is no extension requirement, but conventionally, suanPan picks `.supan` as the suffix to indicate the file is a suanPan model input file. This is not compulsory, though.

To run the input file, there are two approaches. Users can directly assign an input file as the program starting argument.

The truss model from previous post can be stored in `TRUSS2D.supan` file.

``` text
# TRUSS2D.supan
material Elastic1D 1 42
node 1 0 0
node 2 1.6 0
element Truss2D 1 1 2 1 27
step static 1
fix 1 1 1
fix 2 2 1 2
cload 1 0 25 1 2
analyze
peek node 2
exit
```

One can then run it by typing in

``` bash
./suanPan -f TRUSS2D.supan
```

The `-f` can be replaced by `--file`. Another way to run an input file is to use `file` command in CLI mode.

``` text
suanPan --> file TRUSS2D.supan
```

Input File Structure
--------------------

The input file can be split into three parts. Take the model from previous post as example.

``` text
             --> | material Elastic1D 1 42
             |   | 
             |   | node 1 0 0
SETUP    ----|   | node 2 1.6 0
             |   | ...
             |   | 
             --> | element Truss2D 1 1 2 1 27
-------- --------|-------------------------------
             --> | step static 1
             |   | 
             |   | fix 1 1 1
ANALYSIS ----|   | fix 2 2 1 2
             |   | cload 1 0 25 1 2
             |   | 
             --> | analyze
-------- --------|-------------------------------
             --> | 
OUTPUT   ----|   | peek node 2
             --> | 
-------- --------|-------------------------------
                 | exit
```

### Model Setup

In the first part, users are required to define nodes, elements, sections, materials, recorders, etc. or load files, external modules and/or other configurations. The order of commands **does not** really matter as everything will only be created but not initialized in this part. So it is legal to write something like follows, although element 1 depends on node 1 and node 2 and material 1.

``` text
element Truss2D 1 1 2 1 27
node 2 1.6 0
material Elastic1D 1 42
node 1 0 0
```

### Analysis

In the second part, users are asked to create analysis steps, similar to the analysis flow in ABAQUS, by default, a valid analysis flow starts with step 0. Users shall define multiple steps in a strictly increasing order. **Anything defined in between two steps belongs to the previous one and will persist in subsequent steps by default.** If this is not the behavior users want, it can be turned off manually.

``` text
           --> | ...
           |   |
STEP 0 ----|   |
           |   |
           --> | ...
---------------|-------------------------------
           --> | step static 1
           |   | ...
STEP 1 ----|   |
           |   |
           --> | ...
---------------|-------------------------------
           --> | step static 2
STEP 2 ----|   | ...
           --> | ...
---------------|-------------------------------
               | ...
```

So step 0 overlaps the first part. The step-specific configurations should be defined in corresponding step regions. This part ends with the `analyze` command, which triggers the analysis. Similar to ABAQUS, some prescribed conditions can be either defined in step 0 or step 1, so it is alright to write

``` text
fix 1 1 1
fix 2 2 1 2
cload 1 0 25 1 2
step static 1
analyze
```

### Output

The final part is output section. Users can check the value of specified object, or save the recorded data, or save the model for post-processing. Users can also continue analysis by defining more steps, or erase this model and create another one, or keep the existing model and switch to another model. Those functions will be introduced later.

A Modified Model
----------------

The order of commands is changed, check the difference and make sure you understand the flexibility of writing a model.

``` text
element Truss2D 1 1 2 1 27
node 2 1.6 0
material Elastic1D 1 42
node 1 0 0
fix 2 2 1 2
cload 1 0 25 1 2
fix 1 1 1
step static 1
analyze
peek node 2
exit
```
