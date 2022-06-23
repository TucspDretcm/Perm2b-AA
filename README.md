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
print(" m\t\t\t|\t c= P(m)\t\t|\t m' = S(c)")
print("-"*85)

for _ in range(10):
  m = random.randint(2, n-1)
  c = rsa.Cifrado(m)
  semi_m = rsa.Descifrado(c)
  print(" {:}\t|\t {:}\t|\t {:}".format(m, c, semi_m))
```
**Output:**
```
 m			|	 c= P(m)		|	 m' = S(c)
-------------------------------------------------------------------------------------
 176766408831992707	|	 61408374729407702	|	 176766408831992707
 183597512350914172	|	 195889659501958673	|	 183597512350914172
 136068147220167639	|	 38581632321644873	|	 136068147220167639
 44545726954338757	|	 212320741186060674	|	 44545726954338757
 159526595081864709	|	 64698867816577206	|	 159526595081864709
 102238157482647417	|	 198809144261950100	|	 102238157482647417
 42566669424109485	|	 173690110227850276	|	 42566669424109485
 187235573050613406	|	 191513237696801722	|	 187235573050613406
 70922504592093835	|	 160177844572460541	|	 70922504592093835
 129506786751827790	|	 66997309505938865	|	 129506786751827790
```

**EXTRA: Frases o palabras Cifradas y Descifradas.**

```python
def Cifrado(msg):
  cade = ""
  data = []
  for m in msg:
    me = rsa.Cifrado(ord(m))    # pasamos el valor en ascci de la palabra a el Cifrado RSA
    encrip = me % ord('A') + 65   # rango [65, 122] donde 65='A' y 122='z'
    cade += str("%c" % encrip)   # convertimos de decimal a word ascci
    data.append(me)
  return cade, data

def Descifrado(msg, data):
  cade = ""
  for i in range(len(msg)):
    med = rsa.Descifrado(data[i])
    cade += str("%c" % med)
  return cade

words = ["Hola", "Juego", "pichanga", "lemur", "DBP", "Algebra", "Hulk", "Mina", "Gaa", "UCSP"]
for word in words:
  word_2, data = Cifrado(word)
  word_3 = Descifrado(word_2, data)
  print("(Original={:}, Cifrado={:}, Descifrado={:})".format(word, word_2, word_3))
```
**Output:**
```
(Original=Hola, Cifrado=qnb~, Descifrado=Hola)
(Original=Juego, Cifrado=HFQCn, Descifrado=Juego)
(Original=pichanga, Cifrado=CMBU~YC~, Descifrado=pichanga)
(Original=lemur, Cifrado=bQ|FO, Descifrado=lemur)
(Original=DBP, Cifrado=NSe, Descifrado=DBP)
(Original=Algebra, Cifrado=LbCQbO~, Descifrado=Algebra)
(Original=Hulk, Cifrado=qFb|, Descifrado=Hulk)
(Original=Mina, Cifrado=QMY~, Descifrado=Mina)
(Original=Gaa, Cifrado=C~~, Descifrado=Gaa)
(Original=UCSP, Cifrado=NZTe, Descifrado=UCSP)
```
