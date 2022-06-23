# Perm2b-Algebra Abstracta

**Integrantes:**

*   Gabriel Ivan Rodriguez Postigo
*   Jaime Mateo Gutiérrez Muñoz
*   Alexander Carpio Mamani

**Como correr el codigo:**
* Para poder ver y probar el trabajo selecione el archivo "Permanente_2b.ipynb" y luego clickear en "Open in colab" para poder correr el programa.

**1) Implementar RSA KEY GENERATOR**

* El código funciona con normalidad hasta un "k" de 59, despues del 60 no llega a generar las llaves. El constructor de "RSA_KEY_GENERATOR" busca una forma en que si hay un error con la encriptación y descencriptación aumente el "s" para el funcionamiento de "MILLER_RABIN" en razón de 10^n (10^1 = 10, 10^2 = 100, etc.).

```python
class RSA:
  def __init__(self, k):

    n = 1
    while True:
      self.e, self.d, self.n = self.RSA_KEY_GENERATOR(k, 10**n)
      m = random.randint(2,self.n-1)
      c = self.Cifrado(m)
      if m == self.Descifrado(c):
        break
      else:
        n += 1

    print("RSA (s={:}, e={:}, d={:}, n={:}, phi={:})".format(10**n, self.e, self.d, self.n, self.phi))

  def RSA_KEY_GENERATOR(self, k, s):
    if k<8:    # generar primos con bits menores al dividirlos entre 2 no abria muchos, ocurria un loop, o un error.
      k = 8
    p = RANDOMGEN_PRIMOS(k//2, s)
    q = RANDOMGEN_PRIMOS(k//2, s)
    while p==q:
      q = RANDOMGEN_PRIMOS(k//2, s)

    n, self.phi = p*q, (p-1)*(q-1)

    e = random.randint(2,n-1)
    while Euclides(e, self.phi)!=1:
      e = random.randint(2, n-1)
      
    d = Inversa(e, self.phi)
    return e, d, n

  def Cifrado(self, m):
    return EXPMOD(m, self.e, self.n)

  def Descifrado(self, c):
    return EXPMOD(c, self.d, self.n)
  
  def get_edn(self):
    return self.e, self.d, self.n


rsa = RSA(59)

mensaje = 13
print("\nMensaje original: ", mensaje)
c = rsa.Cifrado(mensaje)
print("Cifrado: ", c)
print("Descifrado: ", rsa.Descifrado(c))


e,d,n = rsa.get_edn()

print("\ne*d mod phi =", e*d % rsa.phi)    # e*d mod n = 1 mod n
print("phi | (e*d - 1) = ", (e*d-1) % rsa.phi)   # phi | (e*d - 1)
print("mgd(e, phi)={:}\t mgd(d,phi)={:}".format(Euclides(e, rsa.phi), Euclides(d, rsa.phi)))

```

**Output:**
```
RSA (s=1000, e=35838547217257697, d=111491921678772113, n=207181330872770987, phi=207181329952084080)

Mensaje original:  13
Cifrado:  94807060877105423
Descifrado:  13

e*d mod phi = 1
phi | (e*d - 1) =  0
mgd(e, phi)=1	 mgd(d,phi)=1

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
**Output:**
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
