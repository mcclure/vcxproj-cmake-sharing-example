Imagine you have a program, and you decide you want to compile it with CMake on some platforms and Microsoft Visual Studio on others (say, CMake for Mac and Linux, VS for Windows). But the list of cpp files in the program is the same in both build systems. Isn't it annoying to be maintaining two lists?

It turns out it's possible to just keep a list of filenames in a .txt file, and load that list from both CMake and a Visual Studio vcxproj in the same repo. I did this recently and it is simple, but the way to do it was not very obvious, so I decided to post my sample code.

## CONTENTS

The program consists of some trivial C++ files in `src/`, a CMake build script in `CMakeLists.txt`, a MSBuild build script in `Project.vcxproj`, and a list of filenames in `SharedFiles.txt`. The process to compile the contents of `SharedFiles.txt` is in lines 43-57 on Project.vcxproj and lines 7-11 in `CMakeLists.txt`. It's simpler in CMake.

### TEST IT: CMAKE

Run:

	(mkdir -p build && cd build && cmake .. && cmake --build . && ./CmakeSharingExample)

### TEST IT: VISUAL STUDIO

Double-click `Project.vcxproj`. Let it upgrade if it wants (this may remove the vcxproj). Select "Build"->"Build Solution".

Now run `.\x64\Debug\Project.exe`.

## LICENSE

The contents of this directory were created by Andi McClure for Love Conquers All Games. It is made available to you under the Creative Commons Zero license (effectively public domain).

## ROOM FOR IMPROVEMENT?

Things I wonder but do not care enough to find out:

* Could the three "steps" in Project.vcxproj be collapsed into one? Would that make the MSBuild code look cleaner?
* Could one publish some sort of MSBuild "library" so that you could just collapse the vcxproj "steps" into a function, and then just write `<ClCompile Include="$([FileInclude]::List('SharedFiles.txt', 'src/''))" />` or something?

