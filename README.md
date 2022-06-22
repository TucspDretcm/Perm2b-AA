# Perm2a-Algebra Abstracta

**Integrantes:**

*   Gabriel Ivan Rodriguez Postigo
*   Jaime Mateo Gutiérrez Muñoz
*   Alexander Carpio Mamani

**Como correr el codigo:**
* Para poder ver y probar el trabajo selecione el archivo "perm2a.ipynb" y luego clickear en "Open in colab" para poder correr el programa.

**El mejor "s":**
* **El mejor "s" para correr el algoritmo es 3, fue encontrado haciendo 3 tipos de pruebas que se pueden encontran en el "perm2a.ipynb"**, pero en la prueba donde nos dimos cuenta que sufre más fue en la de generar el siguiente primo con un valor de "s" bajo y mientas menor sea el "s" generaba primos incorrectos de 1000 primos que debia generar:

```python
def GEN_PRIMO_SIGUIENTE(n, s):
  n = n + 1 - (n % 2)
  while MILLER_RABIN(n, s) == False:
    n = n + 2
  return n

numero_pruebas = 1000

for s in range(0, 20):   # probamos para "s" del 0 al 19.
  print("s: ", s, end="")

  parte = [GEN_PRIMO_SIGUIENTE(2, s)]
  for i in range(numero_pruebas - 1):
    parte.append(GEN_PRIMO_SIGUIENTE(parte[-1] + 1, s))

  primos = []
  for p in parte:
    if MILLER_RABIN(p, 100):    # evaluamos el número generado con un s=100 que es muy confiable
      primos.append(p)

  print("  generados:", len(primos))
```

**output:**
```
s:  0  generados: 302
s:  1  generados: 991
s:  2  generados: 998
s:  3  generados: 1000
s:  4  generados: 1000
s:  5  generados: 1000
s:  6  generados: 1000
s:  7  generados: 1000
s:  8  generados: 1000
s:  9  generados: 1000
s:  10  generados: 1000
s:  11  generados: 1000
s:  12  generados: 1000
s:  13  generados: 1000
s:  14  generados: 1000
s:  15  generados: 1000
s:  16  generados: 1000
s:  17  generados: 1000
s:  18  generados: 1000
s:  19  generados: 1000
```

Definimos el mejor "s":

```python
best_s = 3
```

**1) Generar todos los primos de 3, 4 y 5 cifras.**
* Comenzará desde la minima cifra que le demos, ejemplo, si queremos cifra de 3 lo que hará es 10**(3-1) lo cual es 100 siendo el minimo numero de 3 cifras. Luego entra un bucle while que mientras que el número primo generado gracias a la funcion GEN_PRIMO, que genera el siguiente o el mismo primo a el número enviado o sea al anterior primo más 1, convertido en string y viendo la longitud de la cadena si es menor a 5 seguira generando numeros, donde cada número generado lo guardamos en una lista para luego imprimirla.

```python
desde = 3   # número de cifras
hasta = 5
primos = []

x = 10**(desde-1)    # 10**2 = 10**(3 cifras - 1) = 3 cifras,   10**4 = 10**(5 cifras - 1) = 5 cifras
while len(str(x)) <= hasta:
  x = GEN_PRIMO_SIGUIENTE(x + 1, best_s)
  if len(str(x)) <= hasta:
    primos.append(x)

anterior = primos[0]
for primo in primos:
  if len(str(primo)) > len(str(anterior)):
    print()
  print(primo, end=" ")
  anterior = primo
```
**output:**
```
101 103 107 109 113 127 131 137 139 149 151 157 163 167 173 179 181 191 193 ...
1009 1013 1019 1021 1031 1033 1039 1049 1051 1061 1063 1069 1087 1091 1093 1097 ...
10007 10009 10037 10039 10061 10067 10069 10079 10091 10093 10099 10103 10111 10133 ...
```
