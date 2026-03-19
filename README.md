# HIJ-Vision

This is a LabVIEW Vision library designed for the needs of a laser lab. It is based on the LabVIEW Actor Framework. Please read this [whitepaper](https://www.ni.com/de-de/shop/labview/ni-labview-virtual-user-group--introduction-to-actor-framework.html) first. 

HIJ-Vision contains four parts:
- Camera driver actor based on IMAQdx
- Image Display actor adapted for laser operation
- Analysis actor and analysis classes
- Application actor

## Camera Actor

Provides control over most camera attributes. You may need to set some special attributes in NI MAX or even in vendor software like Pylon Viewer (Basler) or VimbaViewer (Allied Vision). From the Image Display context menu, you can open the camera settings dialog in NI MAX style. All your changes will be saved and remain consistent with NI MAX.

Because the IMAQdx Event "new image" did not work reliably in LabVIEW 2014, I scan the frame number in ActorCore.vi every few milliseconds and send the image and frame number to the application actor.

It is possible that your analysis is slower than image acquisition. You can decrease the image rate or manage the situation in the application actor.

## Image Display Actor

- Shows images in different color palettes
- Manages Regions of Interest (ROIs). You can save ROIs by name (e.g., focus with reduced energy, full-power focus, etc.) and load them as needed.
- Target cross and center of gravity cross (deviation is typically included by default)
- Results of analysis via overlays. You can disable this in the context menu. Analysis is typically performed only on ROI-selected areas.
- Saves background image and enables subtraction.
- Triggers saving of the raw image with all metadata important for result provenance.
- Manages display settings (calibration in px/µm, target and centroid crosses, threshold, simple intensity graph)

## Analysis Actor and Classes

- First, the actor performs preprocessing, e.g., background subtraction, rotation, threshold setting, etc.
- Then it calls the analysis VI of the analysis classes, e.g., Energy, Fluence, Deviation, q-Factor, etc.
- You can easily add your own evaluations by creating new child classes of `Analyze BaseClass`.
- Composite processing is also possible, e.g., first calculate energy, then analyze fluence (energy / area).

## Application Actor

Glues the components together. Here you can decide how to proceed when the acquisition rate is faster than the evaluation speed.

## How to Start

It may be a good idea to start with the [example project](https://github.com/Vision-For-Laserlab/HIJ-Vision-Example-Project). In general:

- Create a Git repository for your application
- Add HIJ-Vision and ViewableActor as submodules to your project (I recommend placing them in the Submodules folder).
- Set submodules to read-only to avoid unwanted automatic changes by LabVIEW. (Of course, you can make reasonable changes, but it should always be a conscious decision.)
- Create a LabVIEW project
- Take a look at the example user applications

Happy coding!

It may be difficult to understand this library. It took me several iterations to arrive at this form. If you have any questions, I will be glad to answer them.

**Contact:** Alexander Kessler, a.kessler@hi-jena.gsi.de
