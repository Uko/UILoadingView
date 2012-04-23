UILoadingView
=============

UILoadingView is an iOS drop-in class that displays a full-screen status indicator with a white background and a spinner with a word: "Loading…", while work is being done in a background thread. Just like the one in YouTube and iTunes native apps. Especially useful with **UITableView**s

[![](http://i1078.photobucket.com/albums/w486/UkoDragon/UILoadingView/th_iOSSimulatorScreenshot232012191132.png)](http://i1078.photobucket.com/albums/w486/UkoDragon/UILoadingView/iOSSimulatorScreenshot232012191132.png)

Requirements
------------
iOS5

Adding UILoadingView to your project
------------------------------------

The simplest way to add the UILoadingView to your project is to directly add the `UILoadingView.h` and `UILoadingView.m` source files to your project.

1. Download the latest code version from the repository (you can simply use the Download Source button and get the zip or tar archive of the master branch).
2. Extract the archive.
3. Open your project in Xcode, than drag and drop `UILoadingView.h` and `UILoadingView.m` to your classes group (in the Groups & Files view). 
4. Make sure to select Copy items when asked. 

If you have a git tracked project, you can add UILoadingView as a submodule to your project. 

1. Move inside your git tracked project.
2. Add UILoadingView as a submodule using `git submodule add git://github.com/Uko/UILoadingView.git` .
3. Open your project in Xcode, than drag and drop `UILoadingView.h` and `UILoadingView.m` to your classes group (in the Groups & Files view). 
4. Don't select Copy items and select a suitable Reference type (relative to project should work fine most of the time).

Usage
-----

The main guideline you need to follow when dealing with UILoadingView while running long-running tasks is keeping the main thread work-free, so the UI can be updated promptly. The recommended way of using UILoadingView is therefore to set it up on the main thread and than spinning the task, that you want to perform, off onto a new thread.

Here is an example assuming you write the code in the controller:

```Objective-C
[self.view addSubview:[[UILoadingView alloc] initWithFrame:self.view.bounds]];
dispatch_queue_t downloadQueue = dispatch_queue_create("downloader", NULL);
dispatch_async(downloadQueue, ^{
    //your data loading
    dispatch_async(dispatch_get_main_queue(), ^{
        [[self.view.subviews lastObject] removeFromSuperview]; //removes the UILoadingView
    });
});
dispatch_release(downloadQueue);
```

You just add UILoadingView as a subview to your main view before running the code in the "loading queue" and remove it (as the last object of your main view subviews array) when your long-running task is done.