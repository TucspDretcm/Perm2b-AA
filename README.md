# Perm2b-Algebra Abstracta

**Integrantes:**

*   Gabriel Ivan Rodriguez Postigo
*   Jaime Mateo Gutiérrez Muñoz
*   Alexander Carpio Mamani

**Como correr el codigo:**
* Para poder ver y probar el trabajo selecione el archivo "Permanente_2b.ipynb" y luego clickear en "Open in colab" para poder correr el programa.

**1) Implementar RSA KEY GENERATOR**

* **El mejor "s" para correr el algoritmo es 3, fue encontrado haciendo 3 tipos de pruebas que se pueden encontran en el "perm2a.ipynb"**, pero en la prueba donde nos dimos cuenta que sufre más fue en la de generar el siguiente primo con un valor de "s" bajo y mientas menor sea el "s" generaba primos incorrectos de 1000 primos que debia generar:

```python
def RSA_KEY_GENERATOR(k):
  if k<8:    # generar primos con bits menores al dividirlos entre 2 no abria muchos, ocurria un loop, o un error.
    k = 8
  p = RANDOMGEN_PRIMOS(k//2, 50)
  q = RANDOMGEN_PRIMOS(k//2, 50)
  while p==q:
    q = RANDOMGEN_PRIMOS(k//2, 50)

  n, phi = p*q, (p-1)*(q-1)

  while True:
    e = random.randint(2,n-1)
    while Euclides(e, phi)!=1:
      e = random.randint(2, phi)
    
    d = Inversa(e, phi)
    #d = (1 + 2*phi) // e
    if d != e:
      break

  return e, d, n

print(RSA_KEY_GENERATOR(8))
```

**output:**
```
(143, 47, 187)
```


**2) Crear un sistema RSA-64 (de k = 64 bits), y mostrar los valores para e, d y n. Generar una tabla con tres columnas m, c = P(m) y m' = S(c) - en teoría se cumple m' = m. Para las filas usar 10 valores números aleatorios distintos para m.**

```python


e, d, n = RSA_KEY_GENERATOR(64)

def Cifrado(m):
  return EXPMOD(m, e, n)

def Descifrado(c):
  return EXPMOD(c, d, n)
  
print(" m\t\t\t|\t c= P(m)\t\t|\t m' = S(c)")
print("-"*85)

for _ in range(10):
  m = random.randint(2, n-1)
  c = Cifrado(m)
  semi_m = Descifrado(c)
  print(" {:}\t|\t {:}\t|\t {:}".format(m, c, semi_m))

```
**output:**
```
 m			|	 c= P(m)		|	 m' = S(c)
-------------------------------------------------------------------------------------
 7207667428647347601	|	 1456710127478942461	|	 1921776242055237321
 8889895259635010805	|	 996644895781868218	|	 3656710678640631361
 2090665272231853218	|	 8505805083046487518	|	 4393305635060783204
 5245426658132327654	|	 1052373919662924865	|	 4261715574112487861
 5734268054426570574	|	 7712274410788929638	|	 2627475626434888780
 2678122799341674164	|	 5473824777332633586	|	 6931399669838376076
 1793397079633690066	|	 370944889834546759	|	 8933223213610329993
 4945245920683046869	|	 1029912308029539944	|	 4071995179299853371
 6857914339318320304	|	 20244746271920071	|	 4794475978790964109
 805605239765009570	|	 5949357983074450188	|	 7024303945467704463
```
