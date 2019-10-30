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
 
 #### 4. write the PA, VTK, TTK
 <pre>
 //UI
 paraview -> GraphRefinement -> GraphRefinement.xml
 //VTK, input, output(content is empty at the beginning)
 core -> vtk -> ttkGraphRefinement -> ttkGraphRefinement.h and ttkGraphRefinement.cpp
 </pre>
 
 ////////The above process is used for paraview version 5.6.1, the below notes are used for paraview version 5.7.0
 #### 5. start paraview server
 <pre>
 // after cloning the new version of ttk under the build folder in my server,
 // build the new ttk under "build" folder, for some reason, it make ccmake twice to work
 ccmake ..
 make -j35 install
 // Now my ttk should be deployed into J's server paraview. 
 // To simplify the operation for starting the server paraview. add alias " startParaViewServer" in bashrc 
 nano ~/.bashrc
 startParaViewServer
 </pre>
 
 #### 6. clone others to write my own module
 <pre>
 // clone the existing one "HelloWorld" to learn. under "ttk-tukl" folder.
 ./scripts/cloneTTKmodule.sh HelloWorld EventDataConverter
 </pre>
 
 #### 7. proceess to write my own model
 - write paraview first. eg: paraview/EventDataConverter/EventDataConverter.xml
 - wirte core(VTK and TTk)
    - VTK first.  
    <pre>
    //receive the parameter from the local paraview 
    core/vtk/ttkEventDataConverter/ttkEventDataConverter.h
    // input and output of vtk, send data to the functions implemented in TTK
    core/vtk/ttkEventDataConverter/ttkEventDataConverter.cpp
    </pre>
    - TTK
    <pre>
    //namesspace "ttk", define functions which will be implemented in cpp
    core/base/EventDataConverter.h
    // write the code to do something
    core/base/EventDataConverter.cpp
    </pre>
    
 #### 8. use the independent c++ code to debug the changes generated in the core.
 - standalone/GenericScript/CMakeLists.txt. add your module to the project name
 - standalone/GenericScript/main.cpp. manully write the sequence of module to operate. 
 
 #### 9. build my code and run it for debug
 <pre>
 //under built folder. Whent he options appear, turn on the TTK_BUILD_STandalone_apps
 ccmake ..
 make -j35 install
 ./standalone/GenericScript/cmd/ttkGenericScriptCmd 
 </pre>
    
 #### 10. push the new work to road branch
 <pre>
 // add folders or files
 git add nameofmaterials
 // commit
 git commit -a
 // the material list, edit the commit
 eg: eventDataconverter + RoadDataConverter
 // press ctrl+O to write out, then exit. Finally to push the materials
 git push origin road
 //username :rzhan100@asu.edu
 //code : raytao2528
 </pre>
 
 #### 11. check the point using paraview
 out the point data rathen then the cell of point. open the data folder then call "TTK SphereFromPoint"
    
 #### 12. send paparmeter between fundction in vtk and ttk(base)
 You can't send the vtkunstructedGrid data type to ttk. You have to split the data to point and connectivitis using 
 <pre>
 input->GetPoints()->GetVoidPointer(0)); //get the points
 input->GetCells()->GetPointer(); //get the head of cells
 // get #points, #edges
 input->GetNumberOfPoints();
 input->GetNumberOfCells();
 </pre>
 
 
 #### 13. add property or file to grid data
 <pre>
 // if the data type is integer, this example uses unsigned char 
  auto categoryIndex = vtkSmartPointer<vtkUnsignedCharArray>::New();
  categoryIndex->SetName("CategoryIndex"); // except point and cell, for other datatype created by vtkSmartPointer have to contain ths line to name it. 
  categoryIndex->SetNumberOfComponents(1); // how many postions to store for one componets, such as cell need 3 for one component.
  categoryIndex->SetNumberOfTuples(nPoints); 
  // then send the categoryIndex to parseCoords function for assigning value to the property
  // at the end, add the property to the output, actually  the point data in the example
  output->GetPointData()->AddArray(categoryIndex);
 </pre>
 <pre>
 // If you would like to add a file to the whole data, not just add property to the point of the grid data. For example, create a dictionary to loopup. Then attach it to the whole grid data structure.
 std::vector<string> categoryDictionary;
 output->GetFieldData()->AddArray(categoryDictionaryArray);
 </pre>

 #### 14. configuration
 <pre>
                                                      Page 1 of 2
 BUILD_SHARED_LIBS                ON                                                                                                                                                                   
 CMAKE_BUILD_TYPE                 Release                                                                                                                                                              
 CMAKE_INSTALL_PREFIX             /home/local/ASUAD/rzhan100/ttk-tukl/install                                                                                                                          
 Eigen3_DIR                       /usr/lib/cmake/eigen3                                                                                                                                                
 GRAPHVIZ_CDT_LIBRARY             /usr/lib/libcdt.so                                                                                                                                                   
 GRAPHVIZ_CGRAPH_LIBRARY          /usr/lib/libcgraph.so                                                                                                                                                
 GRAPHVIZ_GVC_LIBRARY             /usr/lib/libgvc.so                                                                                                                                                   
 GRAPHVIZ_INCLUDE_DIR             /usr/include                                                                                                                                                         
 GRAPHVIZ_PATHPLAN_LIBRARY        /usr/lib/libpathplan.so                                                                                                                                              
 PARAVIEW_PLUGIN_ENABLE_Topolog   ON                                                                                                                                                                   
 ParaView_DIR                     /home/local/ASUAD/jlukascy/ParaView-v5.7/install/lib/cmake/paraview-5.7                                                                                              
 TTK_BUILD_DOCUMENTATION          OFF                                                                                                                                                                  
 TTK_BUILD_PARAVIEW_PLUGINS       ON                                                                                                                                                                   
 TTK_BUILD_STANDALONE_APPS        ON     
 
  TTK_BUILD_VTK_WRAPPERS           ON                                                                                                                                                                   
 TTK_ENABLE_EIGEN                 OFF                                                                                                                                                                   
 TTK_ENABLE_GRAPHVIZ              OFF                                                                                                                                                                   
 TTK_ENABLE_MPI                   OFF                                                                                                                                                                  
 TTK_ENABLE_OPENMP                ON                                                                                                                                                                   
 TTK_ENABLE_SQLITE3               ON                                                                                                                                                                   
 TTK_ENABLE_ZFP                   OFF                                                                                                                                                                  
 TTK_ENABLE_ZLIB                  ON                                                                                                                                                                   
 VTK_DIR                          /home/local/ASUAD/jlukascy/ParaView-v5.7/install/lib/cmake/paraview-5.7/vtk                                                                                          
 VTKm_DIR                         /home/local/ASUAD/jlukascy/ParaView-v5.7/install/lib/cmake/paraview-5.7/vtk/vtkm                                                                                     
 ZFP_DIR                          ZFP_DIR-NOTFOUND    
 
 </pre>
 
 
 #### 15. c++ optimization
 - static <b> inline </b> functionName. when other place to call the function, it actually copyed the content of the function to that place to run, rather then send object flightly, save the memory and speed up.
 - for a variable, for example it is an interger, when you call it in some function. use <pre>int &variableName </pre>. The system will not to create a new memory to the reference. just use the content. save memory
 
 
