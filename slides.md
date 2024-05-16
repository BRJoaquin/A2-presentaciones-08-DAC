---
theme: seriph
background: https://cover.sli.dev
title: Dividir y Conquistar
info: |
  ## Presentación sobre Dividir y Conquistar
  Explicación detallada de la táctica con ejemplos y casos de uso.
class: text-center
highlighter: shiki
drawings:
  persist: false
transition: slide-left

mdc: true
---

# Dividir y Conquistar

Técnica aplicada ya previamente, quizás sin saber catalogarlo con ese nombre.

<div class="pt-12">
  <span @click="$slidev.nav.next" class="px-2 py-1 rounded cursor-pointer" hover="bg-white bg-opacity-10">
    Presiona Espacio para la siguiente página <carbon:arrow-right class="inline"/>
  </span>
</div>

<div class="abs-br m-6 flex gap-2">
  <button @click="$slidev.nav.openInEditor()" title="Abrir en el editor" class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-white">
    <carbon:edit />
  </button>
  <a href="https://github.com/slidevjs/slidev" target="_blank" alt="GitHub" title="Abrir en GitHub"
    class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-logo-github />
  </a>
</div>

---

# ¿Qué es Dividir y Conquistar?

Dividir y Conquistar (DAC) es una técnica que se basa en dividir un problema en subproblemas más pequeños, resolver esos subproblemas, y luego combinar esas soluciones para resolver el problema original.

---

# Etapas de Dividir y Conquistar

1. **Dividir**: Separar el problema en subproblemas más pequeños.
2. **Conquistar**: Resolver los subproblemas, a menudo de manera recursiva.
3. **Merge**: Combinar las soluciones de los subproblemas para formar la solución final.

---

# Etapa Dividir

En la primera etapa, se trata de crear subconjuntos de datos que tienden a una solución elemental/atómica.

- Por lo general, se dividen en dos, pero no necesariamente tiene que ser así.
- La dificultad puede residir en esta etapa.

---

# Etapa Conquistar

Para resolver los subproblemas, generalmente se utiliza la recursión, aunque también se puede resolver de manera iterativa.

- Existe un tipo especial llamado "decrease and conquer" donde solo se conquista una parte del problema.

---

# Etapa Merge

El merge requiere utilizar las sub-soluciones resultado de la etapa anterior.

- Esta etapa puede ser tanto trivial como compleja, dependiendo del problema.

---

# Pseudocódigo

```txt
Solucion DAC(Problema problema)
if(esSolucionTrivial(problema))
  return resolverTrivial(problema);

Problema pa1 = dividirIzq(problema);
Problema pa2 = dividirDer(problema);

Solucion s1 = DAC(pa1);
Solucion s2 = DAC(pa2);

return merge(s1,s2);
```

---

# Ejemplo: Encontrar el Mínimo

```cpp
int minimo(int valores[], int inicio, int fin)
if(inicio == fin)
  return valores[inicio];

int mitad = (inicio + fin) / 2;

int min1 = minimo(valores, inicio, mitad);
int min2 = minimo(valores, mitad + 1, fin);

return min(min1, min2);
```

---

# Ejemplo: MergeSort

```cpp
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
algorithm quicksort(A, lo, hi) is
  if lo < hi then
    p := partition(A, lo, hi)
    quicksort(A, lo, p - 1)
    quicksort(A, p + 1, hi)

algorithm partition(A, lo, hi) is
  pivot := A[hi]
  i := lo
  for j := lo to hi do
    if A[j] < pivot then
      swap A[i] with A[j]
      i := i + 1
  swap A[i] with A[hi]
  return i
```

---

# Clasificación de DAC

Dos tipos principales:

1. **easy-split-hard-join**: Ejemplo: MergeSort.
2. **hard-split-easy-join**: Ejemplo: QuickSort.

---

# Ejemplo: Torres de Hanoi

Mover los discos de un pilar al otro siguiendo las reglas:

1. Mover un disco a la vez.
2. Nunca colocar un disco sobre uno de menor diámetro.

```txt
hanoi(int disco, char origen, char destino, char intermediario)
if(disco == 1)
  mover(disco, origen, destino)
else
  hanoi(disco-1, origen, intermediario, destino)
  mover(disco, origen, destino)
  hanoi(disco-1, intermediario, destino, origen)
```

---

# Problemas y Soluciones

1. **Problema**: ¿Cómo dividir el problema adecuadamente?
   **Solución**: Analizar la estructura del problema y encontrar un punto de división natural.

2. **Problema**: Dificultad en combinar las sub-soluciones.
   **Solución**: Diseñar una función de merge eficiente y correcta.

---

# Conclusión

Dividir y Conquistar es una poderosa técnica que simplifica problemas complejos dividiéndolos en partes manejables. Con una buena comprensión de las etapas de dividir, conquistar y merge, puedes aplicar esta técnica a una variedad de problemas.

---

# Preguntas y Discusión

¡Ahora es tu turno! Plantea tus preguntas y discutamos posibles soluciones.

---

layout: center
class: text-center
---

# ¡Gracias!

[Documentación](https://sli.dev) · [GitHub](https://github.com/slidevjs/slidev) · [Showcases](https://sli.dev/showcases.html)