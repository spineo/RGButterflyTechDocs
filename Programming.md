## Programming Considerations
 
[![RGButterfly Logo](images/RGButterfly_Logo.png)](https://spineo.github.io/RGButterflyTechDocs/) This section gives an overview of the program structure and development process.

### Platform

The App is currently implemented to execute on the ___iPhone 6, 6s, 6s Plus, 6 Plus, 7, 7 Plus, 5, 5s, and SE___ devices running [_iOS_](https://en.m.wikipedia.org/wiki/IOS) v10.x and using the [_XCode_](https://developer.apple.com/xcode/) 8.2.x IDE for the development/builds. Consideration has been given to extending this App to other platforms and venturing into multi-platform environments such as [_Xamarin_](https://www.xamarin.com/) for future Apps development.

### Language

Work started on this application back in 2014, shortly after I got into iOS development. Though Apple was just switching to the [_Swift_](https://developer.apple.com/swift/) programming language, I decided to continue implementing in [_Objective-C_](https://en.m.wikipedia.org/wiki/Objective-C) since, at the time, Swift was still relatively untested and provided fewer online resources. Fortunately Apple was expected to continue supporting Objective-C into the forseeable future and the Swift API allowed its libraries to be easily integrated into Objective-C applications making a gradual transition easier (I have since started going in this direction and the current apps I am working on use Swift)

### XCode IDE

The experience of using XCode for App development has been largely positive and I have found this IDE to be superior to many of the others used in the past including Eclipse, IntelliJ, and Visual Studio. It took a while getting used to Objective-C and the many IDE abstractions (such as XCode _groups_ used to structure the project tree) especially coming from a Software Engineering background where much of my work was at the Linux OS level using scripting/interpretive languages.

A couple of notes about XCode. It is better run on MacOS systems and is easier to use on a larger screen. That being said, while I prefer XCode development on my iMac it also works fine on my 13 inch MacBook Pro with a bit of juggling.

### Development Environments

As mentioned, most of my development involves switching between an iMac and MacBook Pro. The App Website (http://rgbutterfly.com) and Jenkins CI application (primarily used for database builds and the REST service) are hosted on Amazon AWS instances.

### GitHub Integration

The main project (currently private) integrates a number of private GitHub repos as well as libraries from the [_IOS Utilities_](https://github.com/spineo/ios-utilities), [_IOS Algorithms_](https://github.com/spineo/ios-algorithms), [_IOS Custom Cells_](https://github.com/spineo/ios-custom-cells), and [_RGButterfly Tests (Swift)_]( https://github.com/spineo/rgbutterfly-tests) public repos. Over time, as I refactored the code, it made sense create these separate frameworks of potentially resuable code (the plan is to eventually make all repos public). The down side of this approach has been needing to now integrate more than one GitHub repo into the application (something that is fortunately automatically done in XCode) and tagging the many repos. Since I am the only developer on this project, I have generally worked off of the _Master_ branch except for a few occasions earlier on in the project.

### Issue Tracking

One of the first tasks was to integrate into the development process Issue tracking. __This is something I would highly recommend for any medium to large project__. Though in the past I have used many commercial/open source issue tracking systems any of which would do the trick in multi-user environments, I felt that using such a system for this one-man project was overkill. The system I came up with uses a simple but effective Excel Spreadsheet with five sheets named _Active_, _Pre-Release_, _Post-Release_, _Completed_, and _Next Release_.

For each sheet I included six columns:
* __Task ID__: A numeric id prefixed by _F_ (Feature) or _B_ (Bug) (the Task ID should also be referenced in the Git commits)
* __Component__: Structural or functional component(s) the Feature or Bug affects
* __Description__: Brief description of the problem
* __Status__: Completions Status (i.e., PEND, IN PROG, DONE, or ABAND)
* __Priority__: Work priority starting at 20 with 1 assigned to DONE/ABAND (rows are _desc_ sorted by this column ensuring that high priority items bubble to the top and DONE/ABAND ones to the bottom)
* __Comments__: Additional comments such as completion status or references

The workflow is simple: New issues are generally added to the _Active_ sheet and DONE/ABAND ones get transferred to _Completed_. Issues tied to the Pre/Post-Release time-frames or slated for a future release get moved to the appropritate sheet or added directly to those sheets. To enhance readability selected rows are also color coded (i.e., yellow IN PROG and gray DONE/ABAND).

### App Design and Prototyping

__The several months I spent up front on the App Design and Prototyping paid off during development time__. The process that I followed is outlined below:

1. ___Design the initial Wire Diagram___: The 'Wire Diagram' was a schematic drawn on a large whiteboard. While the initial diagram did not take long to complete (maybe about an hour) it was the result of weeks of brainstorming and successfully outlined my vision. I would often tweak the drawing as I worked out the main features.

2. ___Detail the Main Features___: This step involved writting down in a spreadsheet the key features I wanted to implement. I often refered to this document (along with the _Issue Tracking Spreadsheet_) while implementing the _Proof-of-Concept_ outlined below.

2. ___Implement a Proof-of-Concept Prototype (POC)___: My first questions was _Can the key features be implemented and produce the expected results?_ The best way to find out was to implement a simple POC that integrated these features, among them the [_Match Algorithms_](https://github.com/spineo/ios-algorithms). My target was an _iPad_ App and, despite a few bugs, it produced the results I hoped for and lay down the ground work for the next step. 

3. ___Create the Mockup Application Prototype (MAP)___: By now I just about knew exactly what I wanted but instead of diving into the implementation, I was hoping that creating a MAP would allow me to more easily set up the UI, Control, and Navigation for a "finished" App without the time expenditure generally involved in developing. For this I turned to [___AppCooker___](https://itunes.apple.com/us/app/appcooker-prototyping-mockup-studio-for-ios/id418861662?mt=8), a MAP tool for the iPad. The $20 I invested on this App was well worth the cost. The tool allowed me to lay out the various controllers and UI elements in a way similar to an XCode [_Storyboard_](https://developer.apple.com/library/content/documentation/General/Conceptual/Devpedia-CocoaApp/Storyboard.html) and simulate many of the real App interactive elements such as  _Actions_, [_Segues_](https://developer.apple.com/library/content/featuredarticles/ViewControllerPGforiPhoneOS/UsingSegues.html) and _Navigation_. Even though the MAP took several weeks to complete the finished framework served as an invaluable reference that helped, without a doubt, speed up the development process. An example MAP PDF generated earlier in the process is shown [___here___](images/MAP.png). 

### Data Model

_Data_ is key to this application and this App is built on [_Core Data_](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/CoreData/index.html?utm_source=iosstash.io). A significant amount of time was spent developing the data model (and the many interfacing [_NSManagedObject_](https://developer.apple.com/reference/coredata/nsmanagedobject) sub-classes) even before diving, at least to a significant extent, into the UI controllers and views. Again, time well spent since __changes to the Data Model tend to be costlier down the road than changes to the UI and Controllers__.

The data model is currently at major version 63 and is composed of 20 entities (each mapping to a _table_ in the backend [_SQLite_](https://www.sqlite.org) store). The NSManagedObject classes provide many of the key operations (i.e., Object or NSSet get/set, add, remove) against the _Entities_, _Attributes_, and/or _Relations_. Additional custom methods were also implemented with a few  generic ones packaged in the [_CoreDataUtils_](https://github.com/spineo/ios-utilities/blob/master/CoreDataUtils.m) class but most currently private.

Since most of the controllers in this App are [_TableView_](https://developer.apple.com/library/content/documentation/UserExperience/Conceptual/TableView_iPhone/AboutTableViewsiPhone/AboutTableViewsiPhone.html) Controllers the [_NSFetchedResultsController_](https://developer.apple.com/reference/coredata/nsfetchedresultscontroller) API is used frequently as it provides the built-in callback methods to populate the _TableViews_.

The initial [_ER_](https://en.m.wikipedia.org/wiki/Entity–relationship_model) relations were captured using a graphing application. After the initial implementation, additions/modifications were implemented directly by using the CoreData built-in Data Modelling tool.

## UI Layout and Implementation

Most of the 12 controllers are TableView Controllers as I found this type to generally be more versatile for scrolling and laying out the different types of subviews. All controllers but one are embedded in _Navigation_ controllers and [_BarButtonItems_](https://developer.apple.com/reference/uikit/uibarbuttonitem) are generally the widget of choice for actions or navigation between controllers.

[_Toolbars_](https://developer.apple.com/ios/human-interface-guidelines/ui-bars/toolbars/) are used instead of [_TabBars_](https://developer.apple.com/ios/human-interface-guidelines/ui-bars/tab-bars/) (due to the "deeper" hierarchy of controllers which favors the former type) and _Push Modal_ and Unwind [_Segues_](https://developer.apple.com/library/content/featuredarticles/ViewControllerPGforiPhoneOS/UsingSegues.html) are used to communicated between controllers.

Throughout the App Design and Prototyping and into the implementation process I often had to come up with ways to effectively use the limited Navigation and Toolbar real estate available. In general, I opted for the following choices:
* ___Less is Better___: I tried not to display a widget unless I felt it was essential (modifying behaviour in _Settings_ often presented a better alternative to keep the UI uncluttered)
* ___Consistency is Better___: Widgets that appear in multiple controllers are located in the same place (i.e., _Back_ button top left, _Edit/Done_ button top right, _Settings_ button bottom right, and _Home_ button bottom left)
* ___Group Related Functionality___: To group related functionality, I often tied a [_UIAlertController_](https://developer.apple.com/reference/uikit/uialertcontroller) to multiple [_UIAlertActions_](https://developer.apple.com/reference/uikit/uialertaction) and linked the controller to a BarButtonItem (a good example of this is the use of a _Photo Icon_ to group Camera and Photo Library actions)

## Testing

For the most part, I haven't adhered to test-driven development and the initial suite of Unit tests written in Objective-C have recently been replaced with a Swift re-implementation. For now, these test are in their own stand-alone public repo but will eventually be re-integrated into the _rgbutterfly_ repo.

The [current](https://github.com/spineo/rgbutterfly-tests) set includes UI and Unit tests that integrate the [XCTest](https://developer.apple.com/reference/xctest) framework. The Unit tests are divided into RGButterflyBaseTests (common to all Controllers), Controller, and Model Tests. The former two categories cover Controller components and their connnecting points. The Model tests cover datamodel entities and relations.

## Troubleshooting

Developing the App has often been challenging and there have been times when I have spent hours or, in some cases, days trying to troubleshoot a problem. XCode alone has so many features and configuration options that three years later I continue to learn how to use many new ones.

The times when I have often encountered problems are during upgrades. __Incompatibilites between the versions of XCode, the underlying OS, and/or the IOS deployment device can cause problems__.

New features resulting from upgrades might also break the App due to configuration changes. A recent example of this was when the upgrade to IOS 10 broke access to the Camera and Photo Library. It was not until I fixed one of the XCode configuration files that I was able to get this working again.

Most of the day-to-day problems fortunately are associated with the development process and usually a trip to [_Stackoverflow_](http://stackoverflow.com/tour) or some other online help site provides the tips needed to resolve the problem in short order.

## Resources

* For learning about IOS development I found the Stanford University (available on iTunes) and Apple's Worldwide Developers Conference videos are great resources.
* [_The IOS Human Interface Guidelines_](https://developer.apple.com/ios/human-interface-guidelines/overview/design-principles/) is a good resource for guidelines on designing the UI.


[![RGButterfly Logo](images/RGButterfly_Logo.png)](https://spineo.github.io/RGButterflyTechDocs/)
