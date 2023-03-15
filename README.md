# fft2d_cpp
This is a simple header-only [fftw](https://fftw.org/) wrapper for C++, enabeling 2d Fourier transforms as commonly used in generic image processing tasks. It includes functions for frequency shifting and conversions from complex vectors to real-valued vectors and vice-versa. To get the power spectrum of an image just call:
```C++
FFT2D fft2d(ny, nx);                // Creates a FFT2D object/plan
fft2d.fft(data, data_fft);          // Execute the FFT, such that "data" contains the image and "data_fft" will hold the complex power spectrum
FFT2D::c2abs(data_fft, data_fft_f); // Get the absolute values of "data_fft" (e.g. for plotting)
```
This will do the equivalent of Matlabs:
```Octave
data_fft = abs(ifftshift(fft2(fftshift(data))));
```
The inverse works in the same fashion. Although including the fftshift and ifftshift directly into the 'fft'-method takes away some flexibility, I personally need exactly this implementation like 99% of the time for image processing. Perhaps people may appreciate this kind of pragmatism. It's designed to be as easy of a solution as possible. This work was originally inspired by: https://github.com/lschw/fftw_cpp (GPL3 license), but has moved away from this implementaion a fair bit over time. To use it in your project, simply copy the header file into your project. The code depends only on the C++ standard library and on an installation of the fftw library, built with single precision support. (fftw**f** instead of fftw). That means for building the library on Windows one would have to run:
```bash
curl.exe https://fftw.org/pub/fftw/fftw-3.3.5-dll64.zip --output fftw.zip
tar -xf fftw.zip
lib /machine:x64 /def:libfftw3f-3.def
```
On Linux it is sufficient to install libfftw3-dev, e.g.:
```bash
sudo apt install libfftw3-dev
```
Then in a CMakeLists.txt one would add something like this to link to the library:
```cmake
if (WIN32)
    target_link_libraries(YOUR_PROJECT PUBLIC libfftw3f-3)
else ()
    target_link_libraries(YOUR_PROJECT PUBLIC fftw3f)
endif (WIN32)
```
