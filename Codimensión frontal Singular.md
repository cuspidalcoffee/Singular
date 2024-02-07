Hola Christian y Raúl,

He encontrado una forma para calcular la codimensión frontal y también el número de Milnor frontal (si la conjetura de Mond es cierta en este caso) a partir de un desdoblamiento estable.

La idea es usar la misma construcción que hacemos en el caso de A-equivalencia en mi artículo con Bobadilla y Guille sobre la conjetura de Mond.

Sea f un frontal y F un desdoblamiento estable. Sea G una ecuación reducida de la imagen de F, J(G) el ideal jacobiano y $J_{rel}(G)$ el ideal jacobiano relativo (generado solo por las derivadas parciales respecto de las variables y no de los parámetros). Entonces la fórmula para la codimensión frontal es la dimensión vectorial del cociente

$$\frac{J(G)+(G)}{J_{rel}(G)+(G)}$$


tensorizado con el anillo de parámetros (es decir, haciendo los parámetros igual a cero).

La fórmula para el número de Milnor frontal es muy parecida, aunque está fórmula sirve solo si la conjetura de Mond frontal se cumple (yo creo que sí): es la dimensión vectorial del cociente

$$\frac{J(G)+(G)}{J_{rel}(G)}$$


tensorizado con el anillo de parámetros (es decir, haciendo los parámetros igual a cero).

He hecho el ejemplo de la curva E8 $(x^3,x^5)$ y me da codimensión frontal 2 y número de Milnor frontal 2, o sea, que da el resultado esperado.

Aquí os envío el código de Singular, para que lo probéis. También estaría bien hacer otros ejemplos. El desdoblamiento estable lo he calculado previamente con Mathematica con el procedimiento habitual, tomar la deformación A-versal y frontalizar. Los comandos están comentados para que sepáis lo que hace cada comando:


Calculo de la codimension frontal mediante un desdoblamiento estable

```
ring R=0,(X,Y,a,b),ds;                          /* Anillo de llegada */
ring r=0,(x,a,b),ds;                              /* Anillo de partida */
map f=R,a*x + x^3, -5*a^2*x + 6*a*b*x^2 + 9*b*x^4 + 9*x^5,a,b;             /* Definimos la aplicacion */
ideal Z=0;                                           /* Ideal cero para calcular la imagen */
setring R;
ideal Im=preimage(r,f,Z);                    /* Calculamos el ideal de la imagen */
poly g=Im[1];                                      /* Ecuacion implicita de la imagen */
ideal j=jacob(g);                                 /* Ideal Jacobiano */
ideal i=j,g;	                                          /* Ideal Jacobiano más la imagen */
ideal jr=j[1..2];                                    /* Jacobiano relativo */
ideal ir=jr,g;                                        /* Jacobiano relativo mas la imagen */
module m=modulo(i,ir);                      /* Modulo cociente de los dos ideales */
module m0=subst(m,a,0,b,0);            /* Parametros igual a cero */
ring R0=0,(X,Y),ds;                            /* Anillo de llegada sin parametros */
module m0=imap(R,m0);                   /* Importamos el modulo */
vdim(std(m0));                                   /* Calculamos la dimension vectorial, la Fe-codimension */
```
Calculo del numero de Milnor frontal mediante la deformacion estable
```
setring R;
module k=modulo(i,jr);			/* Modulo cociente sin la imagen abajo */
module k0=subst(k,a,0,b,0);		/* Parametros igual a cero */
setring R0;
module k0=imap(R,k0);
vdim(std(k0));					/* Calculamos la dimension vectorial, el número de Milnor frontal */
```

----
Hola de nuevo,

También he comprobado que realmente se cumple la Conjetura de Mond en este ejemplo.

Para ello solo hay que probar el módulo cociente

$$\frac{J(G)+(G)}{J_{rel}(G)}$$

(sin tensorizar) es Cohen-Macaulay. Tal como expliqué en el seminario, se puede usar la formula de Auslander-Buchsbaum: si la codimensión es n y admite una resolución libre de longitud n, entonces es Cohen-Macaulay.

En nuestro caso:

```
setring R;
dim(std(k)); 
```
Da dimensión 2 y como R tiene dimensión 4, la codimensión es 2
```
mres(k,0);

 5      7      2      
R <--  R <--  R

0      1      2      
```

Es una resolución libre de longitud 2, luego es Cohen-Macaulay.

