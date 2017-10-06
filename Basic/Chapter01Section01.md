Quick Start
===========

Launch the Program
------------------

**suanPan** is an independent terminal application. Users can launch the program in terminal (command prompt) by simply typing in `./suanPan` or `suanPan.exe`. By default, the program enters CLI mode and waits for user input commands.

``` text
+--------------------------------------------------+
|   __        __        suanPan is an open source  |
|  /  \      |  \          FEM framework (64-bit)  |
|  \__       |__/  __   __          Acrux (0.1.0)  |
|     \ |  | |    |  \ |  |                        |
|  \__/ |__| |    |__X |  |     maintained by tlc  |
|                             all rights reserved  |
+--------------------------------------------------+

suanPan -->
```

Start the Interaction
---------------------

**Commands: version, quit, exit.**

Unlike OpenSees, the command parser in **suanPan** is written from scratch and does not support scripting. You can check the version information by typing in command `version`, the output will be printed on the screen.

``` text
suanPan --> version
suanPan is an open source FEM framework.
        version Acrux 0.1.0
        compiled with MSVC 191125508
        date Fri Oct  6 02:15:37 2017

[From Wikipedia] Alpha Crucis is a multiple star system located 321 light years from the Sun in the
constellation of Crux and part of the asterism known as the Southern Cross.
```

To quit the program you can use either `quit` or `exit`. The total execution time will also be printed.

``` text
suanPan --> exit
Finished in 1.186 seconds.
```

A Simple Model
--------------

**Commands: node, element, fix, cload, step, analyze, peek.**

In **suanPan**, no additional configuration is required so users can directly define the model.

### Node

As an illustration, we will show a simple model of a truss bar under uniaxial tension. Assume the truss bar is $$1.6$$ unit long, two nodes are required to define the element, first we could use `node` command to define two nodes.

``` text
node 1 0 0
node 2 1.6 0
```

Almost every model related command in **suanPan** starts with a command name, here is `node`, followed by a unique tag to distinguish this object from others. In above commands, we defined node 1 located at $$(0,0)$$ and node 2 located at $$(1.6,0)$$. Please note fractions cannot be recognized so instead of writing $$8/5$$, we can only use $$1.6$$.

### Material

Since this is an illustration, we could simply use elastic material with an elastic modulus of $$42$$ as the material model. The material can be defined as follows.

``` text
material Elastic1D 1 42
```

### Element

Now we can use `element` to define a truss member using the nodes and material previously defined. Assume the cross section area is $$27$$, the command then should look like this.

``` text
element Truss2D 1 1 2 1 27
```

The above command simply defines a 2D truss element with tag 1 connecting node 1 and node 2, using the material model 1 and has an area of $$27$$. Different commands may have different definitions so may need different parameters. Some commands have default values that do not need to be explicitly claimed. The `Truss2D` element has a full definition

``` text
element Truss2D $tag {$node_tag...} $material_tag $area [$nonlinear_switch] [$constant_area_switch] [$log_strain_switch]
```

The last three switches control the nonlinear behavior. Here we do not need to consider nonlinear geometry. By default, it is turned off. Please check the commands manual for each command definition.

### Step

Very similar to the approach used by ABAQUS, **suanPan** also defines the analysis based on steps. By using `step` command, we can define a static analysis.

``` text
step static 1
```

The above command defines a static step with tag 1. The time period of this step is by default $$1$$.

### Fix

We have defined nodes, material and the element required. Now we can apply the boundary conditions and loads. Assume node 1 is pinned and node 2 is roller supported, we can use `fix` to fix target DoFs.

``` text
fix 1 1 1
fix 2 2 1 2
```

The first command defines a fix constraint with tag 1, fixing DoF 1 of node 1. The second command defines a fix constraint with tag 2, fixing DoF 2 of node 1 and node 2.

### Cload

To apply a concentrated load, use `cload` command.

``` text
cload 1 0 25 1 2
```

As the system is solved incrementally, we also need to define the relationship between the load magnitude and pseudo time. Here we simply assume it is linear. The above command defines a concentrated load with tag 1, using amplitude 0, the magnitude of this load is $$25$$. The load is applied on DoF 1 of node 2. The tag 0 for amplitude is an indicator to inform the solver to use a linear amplitude.

### Analyze

Everything is ready, now we could use `analyze` to run the analysis. If required, we can use `peek` command to check the result.

``` text
analyze

peek node 2
```

### Result

The analytical result can be easily computed.

$$\begin{gather} \Delta{}U=\dfrac{P}{EA}\cdot{}l=\dfrac{25}{42\cdot27}\cdot{}1.6=0.0352734\end{gather}$$

The output of `peek` will be directly printed on the screen. The complete model is summarized as follows.

``` text
+--------------------------------------------------+
|   __        __        suanPan is an open source  |
|  /  \      |  \          FEM framework (64-bit)  |
|  \__       |__/  __   __          Acrux (0.1.0)  |
|     \ |  | |    |  \ |  |                        |
|  \__/ |__| |    |__X |  |     maintained by tlc  |
|                             all rights reserved  |
+--------------------------------------------------+

suanPan --> node 1 0 0
suanPan --> node 2 1.6 0
suanPan --> material Elastic1D 1 42
suanPan --> element Truss2D 1 1 2 1 27
suanPan --> step static 1
suanPan --> fix 1 1 1
suanPan --> fix 2 2 1 2
suanPan --> cload 1 0 25 1 2
suanPan --> analyze
suanPan --> peek node 2
Node 2:
   1.6000        0
Displacement:
   0.0353        0
Velocity:
        0        0
Acceleration:
        0        0

suanPan --> exit
Finished in 3.005 seconds.
```

Some Notes
----------

Now you have run the first model with suanPan. If you are familiar with the syntax of OpenSees, you may find that the command styles are very similar. You can easily adapt existing OpenSees models to suanPan models.

Some may find the feedback information of the execution of every command is annoying. So suanPan does not print anything no matter the command succeeds or fails. Users can use debug version to get more feedbacks from the program.

In the next post, the basic structure of an input file will be introduced.
