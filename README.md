# The Main Title

This is the introduction

## Sub title

Interesting stuff 1

## Sub title

Interesting stuff 2 in **bold** or _italic_ or ~strikethrough~.

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

## Conclusion

Bla bla <https://nullptr.nl> enzo


### References

* <https://github.com/janwilmans/>
* <https://nullptr.nl>





