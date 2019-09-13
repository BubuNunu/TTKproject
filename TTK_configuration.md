#### 1.depoly my TTK on the server to J's PV on the server \br
 my client PV connects to J's PV on the server which is plugined by the TTk.
 My own TTK on the server could also plugin J's PV on the server.
 <pre>
 //clone the github of road branch, rather then master
 git status
 git checkout road // change to road branch
 git pull origin road 
 
 // create folder under my folder
 mkdir build
 // under my build folder, ccmake to scane the parent folder for generating makefile
 ccmake ..
 // Then, you will find some warning and error which tells you that you miss VTK in your folder.
 //I only need to build TTk in my folder. The VTK and Paraview are in J's folder.
 //press e to exit the error 
 e
 //Then there are options for you to change to solve the error
 //you need to enter the fist dir to create a install folder in your folder
 /home/local/ASUAD/rzhan100/TTKproject/ttk/install
 // papaview url should be j's folder
 /home/local/ASUAD/Jonus/Paraview/lib/make/paraview.../install
 // VTK url should be J's folder
 /home/local/ASUAD/Jonus/VTK/lib/make/vtk.../install
 //There still are other options to turn on or turn off.
 //When all the configuration is done, scan again
 c
 //Then got the build folder, build TTK in my server. j takes in charge of threads for parallel things
 make -j38 install // without install, just build the thing in my local server(actually is packgeing everything of my folder). with install, deploy the packaged thing to J's PV.
 </pre>
 
 #### 2. run TTK from the PV in my local computer. 
 <pre>
 //connect to J's PA url
 file -> connet -> port should be same with the port which is used to start the PV on the server
// under the path rzhan100@en4146172l:/home/local/ASUAD/jlukascy/scripts$ to start the PA on the server
./startParaViewServer.sh 11111
 </pre>
 
 
 #### 3. copy the exiting model and rename it for your own model
 <pre>
 // under your folder, get a new moduel "GraphRefinement". Then you could do your things in the new module.
 ./scripts/cloneTTKmodule.sh DepthImageBasedGeometryApproximation GraphRefinement
 </pre>
 
