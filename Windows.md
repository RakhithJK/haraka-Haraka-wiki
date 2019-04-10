Imported from @superman20's comment in [#2628](https://github.com/haraka/Haraka/issues/2628)

I've been running on Windows for years without any issues. The instructions work if you have the 3 required prerequisites installed:

1) Node.js
2) Python
3) Visual Studio 2015 or 2017.

My personal experience is:

- I've had success with both Node.js LTS or Current versions
- You should install Visual Studio manually (the free community edition is fine). Years ago, I do recall having trouble getting a properly working Visual Studio installation with the npm windows-build-tools package. Once you have a working VS installation, there will be hundreds of warnings that you can ignore during the Haraka install/compile phase.
- Years ago, I recall having trouble using Python 3, but Python 2 seemed to work best. I've not had time to diagnose those issues (if they even still exist now). I still use Python 2 to avoid the hassle.

I compile Haraka on a development machine and copy the final result to the production machine. You don't need Visual Studio or Python where you run Haraka (just the Microsoft Visual C++ runtime that pairs with the Visual Studio version you used and Node.js).
Hope that helps you some.