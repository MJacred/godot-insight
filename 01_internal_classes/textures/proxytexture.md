# ProxyTexture

**Researched or last verified:** February 2020


**ProxyTexture is still exposed, so why listed here?**  
As its nature is mostly internal, it will get unexposed soon. And deleted, if it becomes obsolete: https://github.com/godotengine/godot/pull/36275


## Brief Description

Represents and references a [Texture2D], which can be changed anytime, and has its own [RID].

A [ProxyTexture] is a representative for any [Texture2D] with its own [RID]. It will take the [Texture2D] as a reference, but it will not copy the data, i.e. they share the same texture data. You can keep the same [ProxyTexture], but change the referenced [Texture2D] whenever you like.  
By default, the [ProxyTexture] will not be a render target.


```cxx
// Returns the width of its referenced [Texture2D].
// If [ProxyTexture.base] is `NULL`, it will return `1`.
// If this [ProxyTexture] has a placeholder [Texture2D], it will return `4`.
const int get_width();


// Returns the height of its referenced [Texture2D].
// If [ProxyTexture.base] is `NULL`, it will return `1`.
// If this [ProxyTexture] has a placeholder [Texture2D], it will return `4`.
const int get_height();


// If [ProxyTexture.proxy] is `0` (ie. [ProxyTexture.base] is `NULL`), it will create a new [Texture2D] placeholder and reference it in its member [ProxyTexture.proxy_ph]. This placeholder has the size of 4x4 pixels in color `Color(1, 0, 1, 1)` and `Image::FORMAT_RGBA8`. Thereafter, a proxy will be created and set to member [ProxyTexture.proxy], therefore we get a valid [RID] for our [ProxyTexture] object.
// This also means: If this object has a valid [Texture2D] reference and you set a new invalid reference: As soon as [method get_rid] is called, this object will receive a new [RID].
const RID get_rid();


// Returns the referenced [Texture2D]'s alpha value.
// If member [ProxyTexture.base] is `NULL`, it will always return `false`.
const bool has_alpha();


// The first time the setter method is called and passed a valid [Texture2D] reference, this [ProxyTexture] gets its [RID] written to member [ProxyTexture.proxy].
// Every subsequent call of the setter method (while passing a valid [Texture2D] reference) will overwrite its referenced [Texture2D], but keep its original [RID]. If it had a placeholder [Texture2D], the allocated memory will be freed.
// If the given texture is `NULL` the member [ProxyTexture.base] will be set to `NULL`.
// Passing this [ProxyTexture] as a texture reference will fail.
Texture get_base();
set_base(Texture2D t);


// [Texture2D] placeholder. Is generated when [method get_rid] is called, but [ProxyTexture.proxy] has the [RID] value `0`.
RID proxy_ph;
```
