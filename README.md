# JUCEMake

This is a template for building a JUCE-based plugin using CMake; it is designed
to be as minimal and straightforward as possible, such that you only have to
add to it when using it, rather than tearing irrelevant parts away.

## Building

Configure and build the project with CMake as you normally would; using CMake
is outside of the scope of this README.

The main thing to note is that this project does not use submodules. It is
expected that you have JUCE installed "globally" on your system. CMake will
automatically try to find JUCE via `find_package`. If you have JUCE installed
in a non-standard location, you may have to set the `CMAKE_PREFIX_PATH`
environment variable (or pass `-DCMAKE_PREFIX_PATH=...`) when configuring.
