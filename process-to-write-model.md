1. write UI: ttk -> paraview -> EventDataConverter -> EventDataConverter.xml \
In the xml file, write the parameters and source for the model
2. write ttk functions: 
- VTK first.  
<pre>
// ttk -> core -> vtk -> ttkEventDataConverter -> ttkEventDataConverter.cpp and ttkEventDataConverter.h
//receive the parameter from the local paraview 
core/vtk/ttkEventDataConverter/ttkEventDataConverter.h
// input and output of vtk, send data to the functions implemented in TTK
core/vtk/ttkEventDataConverter/ttkEventDataConverter.cpp
</pre>
- TTK second
<pre>
//namesspace "ttk", define functions which will be implemented in cpp
core/base/EventDataConverter.h
// write the code to do something
core/base/EventDataConverter.cpp
</pre>

3. copy an existing model to write a new model
<pre>
 // clone the existing one "HelloWorld" to learn. under "ttk-tukl" folder.
 ./scripts/cloneTTKmodule.sh HelloWorld EventDataConverter
 </pre>
 
 4. use script to run and test the new module
 <pre>
 //under built folder. 
 ccmake ..
 make -j35 install
 // go to the install folder to run the debugging script. Under the build folder
 ../install/bin/ttkGenericScriptCmd 
 </pre>

5.open paraview to  call the ttk module
<pre>
// go to Documents/projects/hotspot/paraview/build/bin
cd Documents/projects/hotspot/paraview/build/bin
./paraview
</pre>
