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

A 3 by 3 case:
 ~~~
 (%i21)	M : matrix([1,2,3],[4,5,6],[7,8,9]);
 (%i21)	matrix([1,2,3],	[4,5,6],[7,8,9])

(%i22)	alt_eigen(M, 'var = z, 'maxdegree = 1);
(%o22)	[z^2=15*z+18,[matrix([26-2*z],[2-z],[-22])],z=0,
        [matrix([-1],[2],[-1])]]

(%i23)	alt_eigen(M, 'var = z, 'maxdegree = 2);
(%o23)	[z=(3*sqrt(33)+15)/2,[matrix([11-3*sqrt(33)],[2-(3*sqrt(33)+15)/2],
		[-22]
	)],z=-(3*sqrt(33)-15)/2,[matrix([3*sqrt(33)+11],[(3*sqrt(33)-15)/2+2],[-22]
	)],z=0,[matrix([-1],[2],[-1])]]
 ~~~
An example with a symbolic matrix entry
 ~~~
(%i49)	M : matrix([1,2],[-1/8,q]);
(%o49)	matrix([1,2],[-1/8,q])

(%i50)	alt_eigen(M, 'var = z, 'maxdegree = 1);
(%o50)	assuming(notequal(-64*(q-2)*q,0),[z^2=-(-4*q*z-4*z+4*q+1)/4,
          [matrix([2],[z-1])]])

(%i51)	subst(q=5,%);
(%o51)	[z^2=-(21-24*z)/4,[matrix([2],[z-1])]]
 ~~~
