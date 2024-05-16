---
theme: seriph
background: https://cdn.jsdelivr.net/gh/slidevjs/slidev-covers@main/static/d34DtRp1bqo.webp
title: Dividir y Conquistar
info: |
  ## Presentación sobre Dividir y Conquistar
  Explicación detallada de la táctica con ejemplos y casos de uso.
class: text-center
highlighter: shiki
drawings:
  persist: false
transition: slide-left
lineNumbers: true
mdc: true
---

# Dividir y Conquistar

Técnica de diseño de algoritmos que simplifica problemas complejos dividiéndolos en partes manejables.

---

# ¿Qué es Dividir y Conquistar?

Dividir y Conquistar (DAC) es una técnica que se basa en dividir un problema en subproblemas más pequeños, resolver esos subproblemas, y luego combinar esas soluciones para resolver el problema original.

<img src="/dac.png" alt="DAC" style="">

---

# Etapas

1. **Dividir**: Separar el problema en subproblemas más pequeños.
2. **Conquistar**: Resolver los subproblemas, a menudo de manera recursiva.
3. **Merge**: Combinar las soluciones de los subproblemas para formar la solución final.

---

# Etapa Dividir

En la primera etapa, se trata de crear subconjuntos de datos que tienden a una solución elemental/atómica.

- Por lo general, se dividen en dos, pero no necesariamente tiene que ser así.
- La dificultad puede residir en esta etapa.

<br>

# Etapa Conquistar

Para resolver los subproblemas, generalmente se utiliza la recursión, aunque también se puede resolver de manera imperativo.

> Existe un tipo especial llamado "decrease and conquer" donde solo se conquista una parte del problema.

<br>

# Etapa Merge

El merge requiere utilizar las sub-soluciones resultado de la etapa anterior.

- Esta etapa puede ser tanto trivial como compleja, dependiendo del problema.

---

# Pseudocódigo

```txt 
Solucion DAC(Problema problema)
  if(esProblemaTrivial(problema))
    return resolverTrivial(problema);

  Problema pa1 = dividirIzq(problema);
  Problema pa2 = dividirDer(problema);

  Solucion s1 = DAC(pa1);
  Solucion s2 = DAC(pa2);

  return merge(s1,s2);
```

---

# Ejemplo: Encontrar el Mínimo


````md magic-move

```cpp
int* copiar(int valores[], int inicio, int fin) {
  int* copia = new int[fin - inicio + 1];
  for(int i = inicio; i <= fin; i++)
    copia[i - inicio] = valores[i];
  return copia;
}

int minimo(int valores[]) {
  if(inicio == fin)
    return valores[inicio];

  int mitad = (inicio + fin) / 2;

  int* copia1 = copiar(valores, inicio, mitad);
  int* copia2 = copiar(valores, mitad + 1, fin);

  int min1 = minimo(copia1);
  int min2 = minimo(copia2);

  return min(min1, min2);
}
```

```cpp
int minimo(int valores[], int inicio, int fin) {
  if(inicio == fin)
    return valores[inicio];

  int mitad = (inicio + fin) / 2;

  int min1 = minimo(valores, inicio, mitad);
  int min2 = minimo(valores, mitad + 1, fin);

  return min(min1, min2);
}
```
````

--- 

# Dividir arrays de forma lógica
Nos ahorraremos tiempo y espacio si dividimos los arrays de forma lógica.

<img src="/dac-array.webp" alt="DAC Array" class="h-60">

Podríamos hayar la mitad de dos formas:
- $mitad = inicio + (fin - inicio) / 2$ 
- $mitad = (inicio + fin) / 2$

---

# Problema: Prefix Común en un Array de Palabras

Encontrar el prefijo común más largo entre un array de palabras.


## Ejemplo

<br>

```
Input: ["flower", "flow", "flight"]
Output: "fl"
```

<br>

## Solución

<br>

1. **Dividir**: Dividimos el array de palabras en dos subarrays.
2. **Conquistar**: Encontramos el prefijo común en cada subarray.
3. **Merge**: Combinamos los resultados para obtener el prefijo común final.


---

# Solución: Prefix Común

Utilizamos la técnica de dividir y conquistar

```cpp {14-24|1-12|all}{maxHeight:'400px'}
string mergePrefix(string left, string right) {
  string ret = "";
  int minLength = min(left.length(), right.length());
  for (int i = 0; i < minLength; ++i) {
      if (left[i] == right[i]) {
          ret += left[i];
      } else {
          break;
      }
  }
  return ret;
}

string commonPrefix(string arr[], int inicio, int final) {
  if (inicio == final) 
    return arr[inicio];

  int mid = (inicio + final) / 2;

  string izqPrefix = commonPrefix(arr, inicio, mid);
  string derPrefix = commonPrefix(arr, mid + 1, final);

  return mergePrefix(izqPrefix, derPrefix);
}
```

---

# Problema: Máxima Secuencia de un Subarray

Encontrar la suma máxima de una subarray continua en un array de números.

## Ejemplo

<br>

```
Input: [-2,1,-3,4,-1,2,1,-5,4]
Output: 6
Explanation: [4,-1,2,1] has the largest sum = 6.
```

<br>

## Solución

<br>

1. **Dividir**: Dividimos el array en dos mitades.
2. **Conquistar**: Encontramos la máxima secuencia en cada mitad.
3. **Merge**: Combinamos los resultados considerando también las subarrays que cruzan el punto medio.

---

# Solución: Máxima Secuencia de un Subarray

```cpp {19-28|1-17}{maxHeight:'400px'}
int maxCrossingSum(int arr[], int left, int mid, int right) {
    int sum = 0;
    int leftSum = INT_MIN;
    for (int i = mid; i >= left; --i) {
        sum += arr[i];
        if (sum > leftSum) leftSum = sum;
    }

    sum = 0;
    int rightSum = INT_MIN;
    for (int i = mid + 1; i <= right; ++i) {
        sum += arr[i];
        if (sum > rightSum) rightSum = sum;
    }

    return leftSum + rightSum;
}

int maxSubArraySum(int arr[], int left, int right) {
    if (left == right) return arr[left];

    int mid = (left + right) / 2;

    int maxIzq = maxSubArraySum(arr, left, mid);
    int maxDer = maxSubArraySum(arr, mid + 1, right);

    return max(maxIzq, max(maxDer, maxCrossingSum(arr, left, mid, right);));
}
```

---

# Ejemplo: MergeSort

```cpp {26-37|1-24}{maxHeight:'450px'}
int* intercalar(int arr1[], int arr2[]) {
  int n1 = sizeof(arr1) / sizeof(arr1[0]);
  int n2 = sizeof(arr2) / sizeof(arr2[0]);
  int* ret = new int[n1 + n2];

  int i = 0, j = 0, k = 0;
  while (i < n1 && j < n2) {
    if (arr1[i] < arr2[j]) {
      ret[k++] = arr1[i++];
    } else {
      ret[k++] = arr2[j++];
    }
  }

  while (i < n1) {
    ret[k++] = arr1[i++];
  }

  while (j < n2) {
    ret[k++] = arr2[j++];
  }

  return ret;
}

int* mergeSort(int valores[], int inicio, int fin)
  if(inicio == fin)
    int* ret = new int[1];
    ret[0] = valores[inicio];
    return ret;

  int mitad = (inicio + fin) / 2;

  int* ord1 = mergeSort(valores, inicio, mitad);
  int* ord2 = mergeSort(valores, mitad + 1, fin);

  return intercalar(ord1, ord2);
```

---

# Ejemplo: QuickSort

```cpp
void quickSort(int valores[], int inicio, int fin) {
  if(inicio < fin) {
    int pivote = particion(valores, inicio, fin);
    quickSort(valores, inicio, pivote - 1);
    quickSort(valores, pivote + 1, fin);
  }
}

int particion(int valores[], int inicio, int fin) {
  int pivote = valores[fin];
  int i = inicio - 1;

  for(int j = inicio; j < fin; j++) {
    if(valores[j] < pivote) {
      i++;
      swap(valores[i], valores[j]);
    }
  }

  swap(valores[i + 1], valores[fin]);
  return i + 1;
}

```

---

# Clasificación de DAC

Dos tipos principales

1. **easy-split-hard-join**: Ejemplo: MergeSort.
2. **hard-split-easy-join**: Ejemplo: QuickSort.

---

# El problema de la mochila
Clásico problema de optimización que se puede resolver con DAC.

El problema consiste en llenar una mochila con objetos de valor máximo sin exceder el peso máximo.

Tiene variaciones:
- Por cantidad de objetos.
  - 0/1: No se puede tomar una fracción de un objeto.
  - 0/INF: Se pueden tomar cuantas veces se quiera un objeto.
  - 0/N: Se puede tomar un objeto un número limitado de veces (depende de cada objeto).
- Por dimensiones.
  - 1D: Solo se tiene en cuenta el peso.
  - 2D: Se tienen en cuenta dos dimensiones (peso y volumen por ejemplo).
  - ND: Se tienen en cuenta n dimensiones.

---

#  El problema de la mochila
Solucionamos el problema de la mochila con DAC.

```cpp
int mochila(int pesos[], int valores[], int capacidad, int n) {
  if(n < 0 || capacidad == 0)
    return 0;

  if(pesos[n - 1] > capacidad)
    return mochila(pesos, valores, capacidad, n - 1);

  int valorUsarObjeto = valores[n - 1] + mochila(pesos, valores, capacidad - pesos[n - 1], n - 1);
  int valorNoUsarObjeto = mochila(pesos, valores, capacidad, n - 1);

  return max(valorUsarObjeto, valorNoUsarObjeto);
}
```