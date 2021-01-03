## The order of CATranforma3D

One of the commont thing when working with CATransform3D is manipulate the transform vectors.
And as you may know, the order of vectors does matter. What I mean is, `v1 * v2` is NOT equal to `v2 * v1`.

So be sure that you are manipulating transform vectors by multiply them in a correct order.

For example this method:

`CATransform3DScale(_ t, _ sx, _ sy, _ sz)`

By checking its documentation, you may know that this method return a new vector which is computed by 
multiply the vector created by `sx`, `sy` and `sz` with `t`. Or in short: `result = scale(sx, sy, sz) * t`.

So what about the order of transformations which will be applied to the view? Well, it will be applied from the left to the right.
In this case, the view will be transformed by `t` then continously transformed by `scale(sx, sy, sz)`.

The same rule is applied for all other manipulation methods of the CATransform3D.
