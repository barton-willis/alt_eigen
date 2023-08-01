# alt eigen

 Maxima language code for expressing eigenvectors in terms of eigenvalues.

To use this package, you will first need to manually load it:
 ~~~
(%i1)	load("alt_eigen.mac")$
~~~
Example of simple usage
~~~
(%i19)	M : matrix([1,2],[3,-4]);
(%o17)	matrix([1,2],[3,-4])
(%i20)	alt_eigen(M, 'var = z, 'maxdegree = 1);
(%o20)	[z=-5,[matrix([-1],[3])],z=2,[matrix([-2],[-1])]]
 ~~~
 The optional argument `'maxdegree = 1` only solves degree one factors of the characteristic polynomial. For this example, both factors are degree one, so `alt_eigen` explicitly finds both eigenvectors.
