# WebGLGraph

Problem: do force-directed semantic graph layout on the GPU


## The only thing WebGL cares about...

The word traditionally is important. It's only a tradition to do it this way. It is in no way a requirement. WebGL doesn't care how we do it, it only cares that our vertex shaders assign clip space coordinates to gl_Position

https://webglfundamentals.org/webgl/lessons/webgl-drawing-without-data.html

## Provisions of WebGL

*from https://webglfundamentals.org/webgl/lessons/webgl-fundamentals.html*

1. Attributes and Buffers

Buffers are iterated through by the vertext shader -- they are *not* random access.
Each vertex shader iteration the next value is pulled out and assigned to an attribute.
Buffers are binary data are uploaded to the GPU such as positions, normals, texture
coordinates, etc.  Attributes specify the type and layout of the buffers.

2. Uniforms

Global variables set before executing the shader program.

3. Textures

Random access arrays of data.  Commonly image data but can be anything.

4. Varyings

Varyings are typically how the vertex shader sends data to the fragment shader.
Depending on what is being rendered, points, lines, or triangles,
the values set on a varying by a vertex shader will be interpolated
while executing the fragment shader.  ITERATED or INTERPOLATED?

## Two Shaders

### Vertex Shader

Normally this generates geometry from data.

The data we have to start off with is vertex
connectivity data naively expressed as triples.
This situation bears some relation to HDT which
succeeds in improving triple rendering by highly
optimizing the storage of the triples.
HDT keeps an sorted list of the URIs in the dataset.
HDT expresses triples internally as, not the uris,
but the indices into the sorted list of uris.

So, as a first draft, or first approximation, perhaps
we should express each of subject, predicate and object
in the triples as the ordinal position of the associated
uri in the list of all uris.  So

#### Uris
1. uri:Fred
2. uri:hasParent
3. uri:Pebbles

But, is there even a need for the URIs to be known by the GPU?
What can it do with them? Render them supposedly.
Perhaps this suggests that the URIs being accessble to the GPU is optional --
perhaps as a texture.


#### Triples

Here is a naive triple encoding.

```
3 2 1. # Pebbles hasParent Fred
```

##### Possible encodings

###### [NetString](https://en.wikipedia.org/wiki/Netstring)

*in this example `<number>` represents a binary representation of the length of the following string*

```
<8>uri:Fred,
<13>uri:hasParent,
<11>uri:Pebbles,
```
	* pro: simple
	* pro: can skip through the data serially, like linked list
	* pro: offset into packed netstrings can be an random access id, sort of
	* con: not fixed width
	* con: not great for random access
	* eg: ``

###### [HDT](http://www.rdfhdt.org/hdt-binary-format/)

Wow, it would be great if an HDT (Header Data Triples) representation could be accessed directly by the GPU.
HDT is already essentially optimal for various semantic processing purposes.
It could be that the low-level nature of GPU coding could be even more compatible with
the HDT structure than Javascript is.
(Ruben complained of challenges with JS wrt HDT.)
Of course it is probable that some subset of HDT be used for GPU rendering. Ideally a legal subset of HDT, ie that can still be read or better, also generated by HDT tooling.
It would be a win even if all we did wrt HDT was borrow data structures and/or algorithms.
In the limit, the biggest win would be if the GPU could simply render raw HDT.



#### Node attributes

#### Edge attributes


### Questions
1. Can textures be accessed by both Vertex and Fragment Shaders?
1. Is yasiv.com rendering node ids (uris, etc) by directly or with help from JS?


### Fragment Shader

Normally this paints the geometry.


## Tips

### run a web server to experience this directory

#### use impatient-mode (if you are running emacs)

These pages were developed in Emacs (impatient-mode)[https://github.com/v2e4lisp/impatient-mode]
because of how fast and fluid the experience is.

    M-x package-install simple-httpd # first
    M-x httpd-start                  # just once per emacs session
    M-x impatient-mode               # in each HTML file you'd like to live edit
    # http://localhost:8080/imp/       to see the files you are impatiently editing
    M-x imp-toggle-htmlize           # if you see raw html instead of rendered

#### other simple servers

[simple web servers](https://gist.github.com/willurd/5720255)

## Resources

* https://antongerdelan.net/opengl/webgl_starter.html
* https://webglfundamentals.org/webgl/lessons/webgl-fundamentals.html
* https://developer.mozilla.org/en-US/docs/Web/API/WebGL_API
* https://webglfundamentals.org/webgl/lessons/webgl-less-code-more-fun.html
* https://github.com/gfxfundamentals/webgl-fundamentals/blob/master/webgl/resources/webgl-utils.js
* https://stackoverflow.com/questions/41450341/webgl-2d-fragment-shader-with-dynamic-data
* https://threejs.org/examples/#webgl_custom_attributes_lines
* https://stackoverflow.com/a/61053059
  * https://www.youtube.com/watch?v=mOAYN3wDvtg
  * https://github.com/vincenzopalazzo/SpyJSBlock.react
* https://github.com/anvaka/VivaGraphJS
  * claims to use WebGL
  * seems to be the basis of yasiv.com
  * https://www.youtube.com/watch?v=xwHSCabvYmY
  * http://www.yasiv.com/graphs#Bai/rw496
