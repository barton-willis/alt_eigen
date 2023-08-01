# alt eigen

 Maxima language code for expressing eigenvectors in terms of eigenvalues.

 ~~~
(%i1)	load("alt_eigen.mac")$
(%i19)	M : matrix([1,2],[3,-4]);
(M)	matrix([1,	2],	[3,	-4])
(%i20)	alt_eigen(M, 'var = z, 'maxdegree = 1);
(%o20)	[z=-5,[matrix([-1],[3])],z=2,[matrix([-2],[-1])]]
 ~~~
