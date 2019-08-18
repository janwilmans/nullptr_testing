# The Main Title

This is the introduction

## Sub title

* option
* option

Interesting stuff 1

## Sub title

Interesting stuff 2 in **bold** or _italic_ or ~strikethrough~.

![Sky Logo](/arg/sky.png)

Now we show this code:

```cpp
#include <vector>
#include <memory>

namespace foo {

class IO
{
public:    
    virtual int read() const = 0;
    virtual void write(int) = 0;
    virtual bool iswriteable() const = 0;
};

class Image {};

class Camera
{
public:
    virtual Image getImage() const = 0;
};

class Owner {
public:
    virtual ~Owner() = default;
};

class CameraBrandA : Owner, Camera
{
    
};

class CameraBrandB : Owner, Camera, IO
{
    
};

class CameraID {};

class CameraProvider
{
public:
    std::vector<CameraID> Discover() const;
    std::unique_ptr<Owner> GetCamera(const CameraID);
};

template <class S, class T>
S interface_cast(T t)
{
    return dynamic_cast<S>(t);
}

template <class S, class T>
S interface_cast(const std::unique_ptr<T>& t)
{
    return dynamic_cast<S>(t.get());
}

template Camera* interface_cast<Camera*>(const std::unique_ptr<Owner>&);
template IO* interface_cast<IO*>(Camera*);

} // foo

int main()
{
    std::vector<std::unique_ptr<foo::Owner>> cameras;

    foo::CameraProvider provider;
    auto ids = provider.Discover();
    auto owner = provider.GetCamera(ids[0]);
    foo::Camera* camera = interface_cast<foo::Camera*>(owner); 
    foo::IO* io = interface_cast<foo::IO*>(camera); 
    cameras.emplace_back(std::move(owner));

    return 0;
}
```

```batch
@echo off
::
:: Copyright 1999-2017 Intel Corporation All Rights Reserved.
:: 

:: Cache some environment variables.
set IPPROOT=%~d0%~p0
set IPPROOT=%IPPROOT:\ipp\bin\=%\ipp
set SCRIPT_NAME=%~nx0

:: Set the default arguments
set IPP_TARGET_ARCH=
set IPP_TARGET_PLATFORM=

:ParseArgs
:: Parse the incoming arguments
if /i "%1"==""        goto Build
if /i "%1"=="ia32"         (set IPP_TARGET_ARCH=ia32)    & shift & goto ParseArgs
if /i "%1"=="intel64"      (set IPP_TARGET_ARCH=intel64) & shift & goto ParseArgs
if /i "%1"=="vs2013"       shift & goto ParseArgs
if /i "%1"=="vs2015"       shift & goto ParseArgs
if /i "%1"=="vs2017"       shift & goto ParseArgs
if /i "%1"=="linux"        (set IPP_TARGET_PLATFORM=linux)      & shift & goto ParseArgs
if /i "%1"=="android"      (set IPP_TARGET_PLATFORM=android)    & shift & goto ParseArgs
if /i "%1"=="quark"        (set IPP_TARGET_PLATFORM=quark)      & shift & goto ParseArgs
goto Error

:Build

:: target architecture is mandatory
if /i "%IPP_TARGET_ARCH%"=="" goto Syntax

if /i "%IPP_TARGET_PLATFORM%" == "android" (set IPP_TARGET_ARCH=%IPP_TARGET_ARCH%_and)
if /i "%IPP_TARGET_PLATFORM%" == "quark"   (set IPP_TARGET_ARCH=%IPP_TARGET_ARCH%_lin_quark)

:: main actions
set LIB=%IPPROOT%\lib\%IPP_TARGET_ARCH%;%LIB%
set LIBRARY_PATH=%IPPROOT%\lib\%IPP_TARGET_ARCH%;%LIBRARY_PATH%
if Exist "%IPPROOT%\..\redist\%IPP_TARGET_ARCH%\ipp" set PATH=%IPPROOT%\..\redist\%IPP_TARGET_ARCH%\ipp;%PATH%
set INCLUDE=%IPPROOT%\include;%INCLUDE%
set CPATH=%IPPROOT%\include;%CPATH%

goto End

:Error
echo Invalid command line argument: %1
echo.
exit /B 1

```

## Conclusion

Bla bla <https://nullptr.nl> enzo


### References

* <https://github.com/janwilmans/>
* <https://nullptr.nl>





