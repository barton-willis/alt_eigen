# alt eigen

 Maxima language code for expressing eigenvectors in terms of eigenvalues.

To use this package, you will need to load it manually. Assuming the file 
`alt_eigen.mac` is in a path that Maxima can find, load it using the command
 ~~~
(%i1)	load("alt_eigen.mac")$
~~~
Examples
~~~
(%i19)	M : matrix([1,2],[3,-4]);
(%o17)	matrix([1,2],[3,-4])

(%i20)	alt_eigen(M, 'var = z, 'maxdegree = 1);
(%o20)	[z=-5,[matrix([-1],[3])],z=2,[matrix([-2],[-1])]]
 ~~~
 This says that `-5` is an eigenvalue with an associated eigenvector of 
 `matrix([-1],[3])`; and that `2` is an eigenvalue with an associated eigenvector of
 `[matrix([-2],[-1]).`  The optional argument `'maxdegree = 1` only solves degree one factors of the characteristic polynomial. For this example, both factors are degree one, so `alt_eigen` explicitly finds _both_ eigenvectors.

A 3 by 3 case:
 ~~~
 (%i21)	M : matrix([1,2,3],[4,5,6],[7,8,9]);
 (%i21)	matrix([1,2,3],	[4,5,6],[7,8,9])
~~~
Since zero is the only degree one factor of the characteristic polynomial, the
other two eigenvectors are expressed in terms of the other factor, namely 
`z^2=15*z+18;` we have
~~~
(%i22)	alt_eigen(M, 'var = z, 'maxdegree = 1);
(%o22)	[z^2=15*z+18,[matrix([26-2*z],[2-z],[-22])],z=0,
        [matrix([-1],[2],[-1])]]
~~~
Setting `maxdegree` to two, we get explicit expressions for all three eigenvectors
~~~
(%i23)	alt_eigen(M, 'var = z, 'maxdegree = 2);
(%o23)	[z=(3*sqrt(33)+15)/2,[matrix([11-3*sqrt(33)],[2-(3*sqrt(33)+15)/2],
		[-22]
	)],z=-(3*sqrt(33)-15)/2,[matrix([3*sqrt(33)+11],[(3*sqrt(33)-15)/2+2],[-22]
	)],z=0,[matrix([-1],[2],[-1])]]
 ~~~

 The matrix entries can be symbolic; here is an example
 ~~~
(%i49)	M : matrix([1,2],[-1/8,q]);
(%o49)	matrix([1,2],[-1/8,q])

(%i50)	alt_eigen(M, 'var = z, 'maxdegree = 1);
(%o50)	assuming(notequal(-64*(q-2)*q,0),[z^2=-(-4*q*z-4*z+4*q+1)/4,
          [matrix([2],[z-1])]])
~~~
The method recognizes that `64*(q-2)q = 0` is a special case. Pasting in `q=5,` we get the eigenvectors of `matrix([1,2],[-1/8,5])`
~~~
(%i51)	subst(q=5,%);
(%o51)	[z^2=-(21-24*z)/4,[matrix([2],[z-1])]]
 ~~~

And a `5 x 5` case

~~~
(%i71)	M : matrix([86, 55, 85,  2, 36],[17, 40,  9, 60, 61],
                   [34, 90, 55, 69, 28],[63, 53, 43, 88, 74],
                   [97,  9, 63, 70, 16]);
(M)	matrix(
		[86,55,	85,2,36],
		[17,40,	9,60,61],
		[34,90,	55,69,28],
		[63,53,	43,88,74],
		[97,9,63,70,16])

(%i72)	alt_eigen(M,'var=z);
(%o72)	[z^5=285*z^4-8709*z^3+615921*z^2+25845020*z-841548,[matrix(
		[36*z^3-705*z^2+498837*z-9681036],
		[61*z^3-8665*z^2+231317*z-6706017],
		[28*z^3+5828*z^2-513901*z+12740025],
		[74*z^3-6689*z^2+297172*z+2073267],
		[z^4-269*z^3+15390*z^2-617117*z+3176427])]]
~~~
For a degenerate eigenvalue, each eigenvector appears in the list; for example
~~~
(%i2)	M : matrix([15714,  24872,  12436], [-1450, -2151,  -1160],[-7025,  -11240, -5451]);
(M)	matrix([15714,24872,12436],
		[-1450,-2151,-1160],
		[-7025,-11240,-5451])

(%i3)	alt_eigen(M,'var=z);
(%o3)	[z=169,[matrix([-8],[5],[0]),matrix([0],[1],[-2])],
         z=7774,[matrix([3109],[-290],[-1405])]]
~~~
To apply Gram-Schmidt to degenerate eigenvectors, use the optional argument 
`'orthogonal=true`
~~~
(%i6)	alt_eigen(M,'var=z,'orthogonal=true);
(%o6)	[z=169,[matrix([40/89],[64/89],[-2]),
                matrix([-8],[5],[0])],
		z=7774,[matrix([3109],[-290],[-1405])]]
~~~
