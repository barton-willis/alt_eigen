# The alt_eigen package

## Introduction

The Maxima language `alt_eigen` package expresses eigenvectors in terms of eigenvalues.

To use this package, you will need to load it manually. Assuming the file
`alt_eigen.mac` is in a path that Maxima can find, load the package using the command

~~~
 load("alt_eigen.mac")$
~~~

## Examples

For our first example, the characteristic polynomial of `matrix([1 , 2],[3 ,-4])` factors over the gaussian integers into a product of first degree terms. So for this matrix, the command `alt_eigen` explicitly finds each eigenvalue and eigenspace:

~~~
(%i3) M : matrix([1,2],[3,-4]);
(%o3) matrix([1,2],[3,-4])

(%i4) alt_eigen(M,'var = z, 'maxdegree = 1);
(%o4) [[z=-5,sspan(matrix([-1],[3]))], [z=2,sspan(matrix([-2],[-1]))]]
~~~

The last two arguments to `alt_eigen` are optional and can be in any order.
The argument `'var = z` tells Maxima to use the name `z` for the eigenvalue, and the argument `maxdegree = 1` instructs the code to solve every factor of the characteristic polynomial that has degree at most one, where the characteristic polynomial is factored over the gaussian integers.

The output is a list of lists, with each list member of the form `[eigenvalue, eigenspace]`, where `eigenvalue` is a polynomial in the variable `var` that has been solved for its highest power.

Each eigenspace is represented as the span of a list of column vectors.
For the span, the `alt_eigen` package defines a simplifying object `sspan`.
For the same purpose, the linear algebra package represents a span using a _nonsimplifying_ `span` object.

For this $3 \times 3$ matrix, one factor of the characteristic polynomial has degree two and the other degree one. Thus, when `maxdegree = 1`, one eigenspace is expressed
in terms of the eigenvalue and the other eigenspace is given explicitly. Changing
to `maxdegree = 2`, all three eigenspaces are given explicitly:

~~~
(%i6) M : matrix([1,2,3],[4,5,6],[7,8,9])$

(%i7) alt_eigen(M,'var = z, 'maxdegree = 1);
(%o7) [[z^2=15*z+18,sspan(matrix([26-2*z],[2-z],[-22]))], 
         [z=0,sspan(matrix([-1],[2],[-1]))]]

(%i8) alt_eigen(M,'var = z, 'maxdegree = 2);
(%o8) [[z=(3*sqrt(33)+15)/2,sspan(matrix([22-6*sqrt(33)],[-(3*sqrt(33))-11],[-22]))],  
        [z=-((3*sqrt(33)-15)/2),sspan(matrix([6*sqrt(33)+22],[3*sqrt(33)-11],[-22]))],
  [z=0,sspan(matrix([-1],[2],[-1]))]]
~~~

When required, the output is enclosed an `assuming` object that includes assumptions about parameters that are necessary for the validity of the answer. Especially when
used non interactively, a user will need to check if the output is an `assuming` object.

 The matrix entries can be symbolic; here is an example

~~~  
 (%i1) M : matrix([1,2],[-1/8,q])$
 (%i2) xxx : alt_eigen(M,'var = z,'maxdegree = 1);
 (%o2) assuming(notequal(-(64*(q-2)*q),0),[[z^2=-((-(4*q*z)-4*z+4*q+1)/4),
    sspan(matrix([2],[z-1]))]])
 ~~~

 Maxima determined that $-64(q-2) q = 0$ is a special case. Substituting $q=2$
 into the above yields `unknown`:

 ~~~
(%i3) subst(q = 2,xxx);
(%o3) unknown
 ~~~

 To find the eigenspace when $q = 2$, you must substitute $q = 2$ before calling `alt_eigen.` But if $q \neq 2$, say $q = 5$, we can make that substitution in the previous result and find that

 ~~~
(%i5) subst(q=5,xxx);
(%o5) [[z^2=-((21-24*z)/4),sspan(matrix([2],[z-1]))]]
 ~~~

For an eigenspace with dimension two or greater, we can optionally apply the
Gram-Schmidt process to find an orthogonal basis for the eigenspace. Here is
an example

~~~
(%i2) M : matrix([15714, 24872, 12436], [-1450, -2151, -1160],[-7025, -11240, -5451])$

(%i3) alt_eigen(M,'var = z);
(%o3) [z=169,[matrix([-8],[5],[0]),matrix([0],[1],[-2])],
         z=7774,[matrix([3109],[-290],[-1405])]]
~~~

To apply Gram-Schmidt to the eigenspace, use the optional argument
`'orthogonal = true`

~~~
(%i1) alt_eigen(M,'var = z,'orthogonal = true);
(%o1) [[z=169,sspan(matrix([-8],[5],[0]),
             matrix([20],[32],[-89]))],
   [z=7774,sspan(matrix([3109],[-290],[-1405]))]]
~~~

We end with a $5\times 5$ example. For this example, we anticipate that
$q = 0$ will be a special case, we start with issuing the Maxima command
`assume(notequal(q,0))`:

~~~
(%i4) M : genmatrix(lambda([a,b], if a=b then 1 elseif abs(a-b)=1 then -q else 0),5,5);
(%o4) matrix([1,-q,0, 0,0],
   [-q,1,-q,0,0],
   [0,-q,1,-q,0],
   [0,0,-q,1,-q],
   [0,0,0,-q,1])
(%i5) assume(notequal(q,0))$

(%i6) alt_eigen(M, 'var = z,maxdegree = 1);
(%o6) [[z^2=2*z+3*q^2-1,sspan(matrix([-q],[z-1],[-2*q],[z-1],[-q]))],
         [z=1-q,sspan(matrix([1],[1],[0],[-1],[-1]))],
   [z=q+1,sspan(matrix([1],[-1],[0],[1],[-1]))],
   [z=1,sspan(matrix([-1],[0],[1],[0],[-1]))]]
~~~

## Testing

Assuming the file `alt_eigen.mac` is in a path that Maxima can find, to run the tests for this package, issue the command
~~~
 batch(rtest_alt_eigen,'test);
~~~
Maxima version 5.47.0 should complete all the tests with no failures. If you find that some tests fail, please report them to the `alt_eigen` discussion list.

## Name of this package

The package name `alt_eigen` isn't particularly descriptive. As you likely suspect, the `alt` in the package name is a shortening of the word `alternative,` meaning an alternative to the standard Maxima package for eigenvalues and eigenvectors. 
