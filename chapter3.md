# Capítulo 3

**Tipo de dado** é um conjunto de valores que exibem comportamento uniforme nas operações daquele tipo.

> **Cardinalidade** é a quantidade de valores distintos que fazem parte de um tipo.
>
> Ex.: `boolean` tem cardinalidade igual a **2**. (JAVA)

![3-arv-tipos](/home/marcela/Dropbox/LP/3-arv-tipos.png)

## Tipos Primitivos

Os valores não podem ser decompostos em valores mais simples. São a base dos demais tipos. São implementados de acordo com o hardware (ex.: tipo `int` - em C - em máquinas de 32 e 64 bits).

### Tipo Inteiro

Geralmente, ocupa o tamanho da palavra do computador e reflete as operações aritméticas do hardware. Há vários tipos inteiros em uma LP (ex.: `long int`, `unsigned int`...).

> *c* deixa a cargo do compilador a definição dos intervalos do tipo para aproveitar ao máximo a capacidade da máquina, já que será possível fazer as operações a nível de hardware. Ganha na eficiência e perde na portabilidade.
>
> *JAVA* tem o intervalo dos tipos pré-definidos para permitir que um  programa rode em qualquer máquina. Porém, as operações são feitas a nível de software. Perde na eficiência, mas ganha na portabilidade.

**Cardinalidade do Tipo Inteiro:** 2^n^ (`n` é a quantidade de bits do número)

### Tipo Caracter

São armazenados como códigos numéricos que correspondem aos símbolos de uma tabela padrão (ex.: ASCII).

Em **C**, os caracteres podem ter as mesmas operações que inteiros, já que são representados por números.

```c
char d;
// Na tabela ASCII, 'a' está na posição 97.
d = 3 + 'a'; // d = 3 + 97 = 100
```

**Cardinalidade do Tipo Caracter:** igual ao número de entradas na tabela de caracteres adotada (ex.: se for ASCII, 128)

### Tipo Booleano

Dois valores: verdadeiro e falso.

> Em *C*, zero é <u>falso</u> e os demais valores são <u>verdadeiro</u>. Ele não possui o tipo `boolean` específico.

**Cardinalidade do Tipo Booleano:** 2

### Tipo Decimal

Essencial para aplicações comerciais, garante que as operações são sempre precisas. Ocupa muita memória porque os números são representados em BCD em vez de binário. O ponto decimal é definido pela LP, pelo hardware ou pelo programador.

**Cardinalidade do Tipo Decimal:** 2 x 10^n^

(ex.: 1234567.89 = 2 x 10^11^)

### Tipo Ponto Flutuante

Apresenta número reais infinitos de forma aproximada. As operações são feitas por hardware. Geralmente, há dois tipos: `float` (32 bits) e `double`(64 bits).

**Cardinalidade do Tipo Ponto Flutuante:** `float` é inferior a 2^32^

### Tipo Enumerado

É possível definir novos tipos através da enumeração de identifcadores. Aumento a legibilidade (usar nomes em vez de números) e confiabilidade (os únicos valores permitidos são os que fazem parte da enumeração) do código. 

> **C** e **C++** tratam enumerações como inteiros.
>
> **JAVA** não tem enumerações de C e C++.

**Cardinalidade do Tipo Enumerado:** número de identificadores da enumeração

### Tipo Intervalo de Inteiros

Semelhante ao Tipo Enumerado: herdam as operações do inteiros, aumentam legibilidade (intervalo claro de valores) e confiabilidade.

```pascal
type meses = 1 .. 12;
```



**Cardinalidade do Tipo Enumerado:** quantidade de valores possíveis do intervalo

## Tipos Compostos

### Tipo Cartesiano

Combinação de valores de tipos **diferentes** em tuplas.

Ex.: registros (PASCAL, ADA, MODULA 2, COBOL), estruturas (C)

```c
struct nome {
  char primeiro [20];
  char meio [10];
  char sobrenome [20];
};
struct empregado {
  struct nome nfunc; // nfunc e salario são seletores dos
  float salario;	// componentes da tupla (nome, salario)
} emp;

// para acessar:
emp.nfunc.meio
```

**Cardinalidade do Tipo Cartesiano:** #(S x T) = #S x #T

### Uniões

União de valores de tipos distintos para formar um novo tipo de dados.

#### Uniões Livres



Ex.: `EQUIVALENCE` (FORTRAN), `union` (C)

#### Uniões Disjuntas

### Mapeamentos

#### Mapeamentos Finitos

#### Mapeamentos através de funções

### Conjuntos Potência

### Tipos Recursivos

São definidos em termos de si mesmo. (ex.: listas)

```
R ::= <parte inicial> R <parte final>
Tipo Lista ::= Tipo Lista Vazia | (Tipo Elemento x Tipo Lista)
```

```c
class no {
  int elem;
  no* prox; // definição por meio de ponteiros
};
```

```java
class no {
  int elem;
  no prox; // definição direta (possível em linguagens OO)
};
```

**Cardinalidade do Tipo Recursivo:** infinita

#### Tipos Ponteiros

São mais frequentes em estruturas recursivas, mas não se limitam a elas. Os valores de um ponteiro são os endereços de memória e o valor `nil` (não está referenciando memória).

```c
#define nil 0
#include <stdlib.h>
#include <stdio.h>
typedef struct no* listaint;
struct no {
  int cabeca;
  listaint cauda;
};
listaint anexa (int cb, listaint cd) {
  listaint l;
  l = (listaint) malloc (sizeof (struct no));
  l->cabeca = cb;
  l->cauda = cd;
  return l;
}
void imprime (listaint l) {
  printf("\nlista: ");
  while (l) {
    printf("%d ",l->cabeca);
    l = l->cauda;
  }
}
main() {
  listaint palindromos, soma10, aux;
  palindromos = anexa(343, anexa(262, anexa(181, nil)));
  soma10 = anexa(1234, palindromos);
  imprime (palindromos); // 343 262 181
  imprime (soma10); // 1234 343 262 181
  aux = palindromos->cauda;
  palindromos->cauda = palindromos->cauda->cauda;
  free(aux);
  imprime (palindromos); // 343 181
  imprime (soma10); // 1234 343 181
}
// a atualização do ponteiro afeta palindromos E soma10
```

```c
// Atribuição de ponteiros
int *p, *q, r; // dois ponteiros para int e um int
q = &r; // atribui endereco onde variavel r esta alocada a q
p = q; // atribui endereco armazenado em q a p

// Alocação de ponteiros
int* p = (int*) malloc (sizeof(int)); // retorna o endereço da célula (a primeira) alocada

// Desalocação de memória
free(p);

// Derreferenciamento
int *p;
*p = 10;
*p = *p + 10
 
// Operações aritméticas (C)
p++; ++p; p = p + 1; p--; --p; p = p - 3;

// Indexação (C)
p[3]; // *(p+3)
```

##### Ponteiros Genéricos

Apontam para `void` e, portanto, podem apontar para qualquer tipo. Pode provocar erro por não ter controle de  tipo.

```c
int f, g;
void* p;
f = 10;
p = &f;
g = *p; // erro: ilegal derreferenciar ponteiro p/ void (detectado em tempo de compilação em C porque não deixa utilizar valores derreferenciados de ponteiros para void)
```

##### Problemas com Ponteiros

* **Baixa Legibilidade:** são como um `GOTO`. As atualizações de valores não  são explícitas e podem afetar outras estruturas que estejam apontando para um mesmo lugar.

* **Erro de violação do sistema de tipos:** podem ser avaliados com valores de tipos diferentes. 

  ```c
  int i, j = 10;
  p = &j;
  p++; // p não neessariamente aponta mais para um inteiro
  i = *p + 5
  ```

* **Objetos Pendentes:** células alocadas dinamicamente se tornam inacessíveis. Provoca vazamento de memória.

  ```c
  int* p = (int*) malloc (10*sizeof(int));
  int *q = (int*) malloc (5*sizeof(int));
  p = q; // celula que era apontada por p torna-se inacessivel
  ```

* **Referências Pendentes:** um ponteiro aponta para um célula desalocada

  ```c
  int* p = (int*) malloc(10*sizeof(int));
  int* q = p;
  free(p); // q aponta agora para area de memoria desalocada

  main() {
  int *p, x;
  x = 10;
  if (x) {
    int i;
    p = &i;
  }
  // i nao existe mais, mas p continua apontado para onde i estava alocado
  }

  int* p;
  *p = 0; // p aponta para um lugar desconhecido da memoria por causa da falta de inicialização
  ```

  #### Tipo Referência

  Disponível em C++ (valor imutável) e JAVA (todas as variáveis de tipos não  primitivos são referêcias, valor mutável).

  ```c++
  int& ref = foo
  ```

  **Cardinalidade do Tipo Referência:** conjunto de valores de endereços das células de memória

### Strings

Sequência de caracteres. Cada LP trata strings de uma forma diferente:

* Tipo primitivo: tem um conjunto específico e pré-definido de operações.
* Mapeamentos finitos: vetor de caracteres. As operações são as mesmas de vetor.
* Classe: possui sua própria biblioteca e operações.
* Tipo recursivo: tipo lista pré-definido

A string pode ser **estática** (tamanho imutável - COBOL), **dinâmica limitada** (apenas o tamanho *máximo* pré-definido - C) ou **dinâmica** (o tamanho muda livremente - PERL)