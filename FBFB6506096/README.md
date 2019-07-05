#  Xcode fails to compile a project with SPM package and build configuration other than Debug/Release

When dealing with multiple server environments, it's useful to have additional build configurations other than Debug/Release to match them with actual server environment build is running on.
For example, we use following environments:
```
DebugDevelop - Debug build with development server
DebugStaging - Debug build with staging server
DebugProduction - Debug build with production server
ReleaseStaging - Release build with staging server, used for testing via TestFlight
ReleaseProduction - Release build with production server, used for testing via TestFlight and releasing to AppStore
```

When trying to integrate SwiftPM package into a project that uses those build configurations, Xcode fails to compile a project, and throws following error:

```
No such module '<Package name>'
```

## Observations

This might be related to a fact, that Xcode seems to ignore DebugDevelop configuration name, and continues to build for Debug configuration instead.
If you go into DerivedData folders, specifically Build/Products folder, you can see that there are two folders present : `DebugDevelop-iphonesimulator`, in which app build products are located, and `Debug-iphonesimulator` folder, in which package build products are located.
This leads us to believe that DebugDevelop build configuration is ignored when building dependencies using SwiftPM.


## Steps to Reproduce

1. Open the sample project in Xcode 11
2. Build a project

## Expected Behavior

The project successfully builds.

## Actual Behavior

The project never builds because of `No such module '<Package name>'` error.
