# HIJ-Vision
That is a LabVIEW Vision library designed for the needs of a laser lab. It based on LabVIEW actor framework. (Still to add some remarks about actor ansatz..) 
Please read the [LV AF whitepaper] (https://www.ni.com/de-de/shop/labview/ni-labview-virtual-user-group--introduction-to-actor-framework.html)
first. The actors communicate via messages in asynchronous way.
HIJ-V contains four parts:
- Camera driver actor based on IMAQdx
- Image Display actor adapted for laser operation
- Analyses actor and analysis classes
- Application actor

## Camera Actor
provides control about most of camera attributes. May be you must set some very special attributes in NI-Max or even in vendor SW like Pylon Viewer (Basler) or VimbaViewer (Alied Vision). From Image Display context menu, you can open camera settings dialog in NI-Max style. All your changes will be saved and are consistent with NI-Max.

Because in LV2014 the IMAQdx Event "new image or so..." did not work reliable, I scan in ActorCore.vi the frame number each some ms and send the image and the frame number to the app actor.

It is possible, that your analysis is slower as image acquisition. You can decrease image rate or you can try to manage the situation in the app actor.

## Image Display Actor
- Shows image in different colour palettes
- Manages Regions Of Interest (ROIs). You can save ROIs by name (e.g. focus with reduces energy, full power focus etc.) and load them by need.
- Target cross and centre of gravity cross (deviation is typically included by default)
- Results of Analysis via overlays. You can disenable this in context menu. The analysis is typically done only for by ROIs selected areas.
- Saves background image and enable subtraction.  
- Triggers saving the raw image with (hopefully) all metadata important for result provenance.
- Manages display settings (calibration in px/µm, target and centroid crosses, treshold, simple intensity graph)

## Analisis Actor and Classes
- First, actor performs pre-processing, e.g. subtracts BG image, rotate, set threshold e.s.o.
- after this it calls the analyse VI of the analysis classes, e.g. Energy, Fluence, Deviation, q-Factor...
- you can easily add your one evaluations by creating new child classes of `Analyze BaseClass`.
- composed processing is also possible, e.g. first calculate energy, second analize fluence (energy / area)

## Application actor
glues the components together. Here you can decide e.g. how to proceed when acquisition rate is faster as evaluation speed.

## How to start
May be it is a good idea to start from the [example project](https://github.com/Vision-For-Laserlab/HIJ-Vision-Example-Project).

But in general:
- Create a git repo for your application
- Add HI-Vision and ViewableActor as submodules to your project (I recommend putting them in the Submodules folder).
- Set submodules to read-only to avoid unwanted automatic changes by LV. (Of course, you can make reasonable changes, but it should always be a conscious decision).
- Create a LV project
- take a glance at the example user applications 

Happy coding!

It may be difficult to understand this library. It took me several iterations to arrive at this form. If you have any questions, I will be glad to answer them.  


Contact: Alexander Kessler, a.kessler@hi-jena.gsi.de
