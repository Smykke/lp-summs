# Capítulo 2

## Tempos de Amarração

* **Tempo de Projeto da LP:** símbolos e identificadores (ex.: * = multiplicação)
* **Tempo de Implementação do Tradutor:** ocorre na hora de traduzir o código (ex.: tamanho do *int* = cada compilador define o seu)
* **Tempo de Compilação:** associação de uma variável a um tipo, identificação das operações...
* **Tempo de Ligação:** ligação entre as chamadas de funções e seus respectivos códigos. Como o programa está na memória secundária, os endereços são relativos.
* **Tempo de Carga:** carregamento do executável na memória. (ex.: alocação de memória para as variáveis globais e constantes)
* **Tempo de Execução:** dar valor a uma variável, alocar memória para uma variável local...

## Ambientes de Amarração

**Escopo de Visibilidade:** área do programa onde a entidade é visível

```c++
// C++
int a = 13;
void f() {
	int b = a;  // variável global (b = 13)
	int a = 2;	// variável local
	b = b + a;	// variável local (13 + 2 = 15)
}
```

* **Escopo Estático:** delimitado por blocos (subprograma, trecho de código...).

  * **Blocos Não Aninhados:** visível dentro do bloco onde foi criado (local). Fora do bloco, estão as variáveis globais. Desvantagem: ou a variável é visível apenas em um bloco, ou é global.

  * **Blocos Aninhados:** blocos podem ser aninhados dentro de outros blocos. O identificador mais interno tem mais preferência.

    ```c
    main() {
      int i = 0, x = 10;
      while (i++ < 100) {
        float x = 3.231;  // o x = 10 não é mais visível a partir daqui
        printf(“x = %f\n“, x*i);
      }
    }
    ```

    > JAVA não permite o uso de identificadores iguais.

    ```ada
    -- ADA
    procedure A is
      x : integer;
      procedure B is
      	y : integer;
      	procedure C is
      		x : integer;
      	begin
      	  x := A.x; -- o primeiro x é de C. O segundo é de A
     	end C;
      begin
     	null;
      end B;
     	null;
    end A;
    ```

    ```c
    int x = 10;
    int y = 15;
    void f() {
      if (y – x) {
      	int z = x + y;
      }
    }
    void g() {
      int w;
      w = x;
    }
    main() {
      f(); // z = 25 (oculto dentro de f)
      x = x + 3; // 13
      g(); // w = 13
    }
    // As variáveis e funções são visíveis para os blocos que nãs as utilizam também (ex.: g pode chamar x, y e f)
    ```

* **Escopo Dinâmico:** amarrações de acordo com o fluxo do programa. 

  ```
  // Linguagem fictícia
  procedimento sub() {
    inteiro x = 1;
    procedimento sub1() { // declaração apenas
    	escreva(x);
    }
    procedimento sub2() { // declaração apenas
    	inteiro x = 3;
    	sub1();
    }
    sub2(); // escreve x = 3
    sub1(); // escreve x = 1
  }
  ```

  * A *legibilidade* de LPs de escopo dinâmico é reduzida porque é necessário seguir a sequência de chamadas para saber o valor que será atribuído às variáveis.

## Definições e Declarações

**Definições:** amarrações de identificadores novos

**Declarações:** amarrações de identificadores já criados

> Em C, todas as amarrações devem ser feitas no <u>início</u> do bloco.

### Constantes

O valor não pode ser alterado ao longo do programa.

```c
const float pi = 3.14
	// pode ser alterado ao longo do programa (o compilador avisa); é alocada na memória toda vez que é chamada; sujeito ao escopo do bloco; aceita valores dinâmicos (ex.: resultado de uma função)
  
#define pi 3.14
  // macro: não pode ser alterado; visível para todo o programa; gasta menos memória; apenas valores estáticos
  //~> na compilação, todas as referências à constante são substituídas pelo valor dela
```

```java
final int const1 = 9; // estático
static final int const2 = 39; // estático
final int const3 = (int)(Math.random()*20); // dinâmico
static final const4 = (int)(Math.random()*20); // dinâmico

// Inicialização dentro de um método
final int j;
  Construtor () {
  j = 1;
}
```

### Definições e Declarações de Variáveis

Alocação de memória.

```c
int i = 0;
char virgula = ', ';
float f, g = 3.59; // f = 0.0
int j, k, l = 0, m=23; // j = 0, k = 0
```

```c++
int r = 10;
int &j = r;
j++; // aumenta r para 11
```

### Definições e Declarações de Subprogramas

```c
int incr (int); // serve para verificar, durante a compilação, se o programa é chamado corretamente
void f(void) {
  int k = incr(10);
}
int incr (int x) {
  x++;
  return x;
}
```

## Definições Sequenciais

Pode usar a definição de outras variáveis, tipos ou subprogramas.

```c
struct funcionario {
  char nome [30];
  int matricula;
  float salario;
};
struct empresa {
  struct funcionario listafunc [1000]; // sequencial
  int numfunc;
  float faturamento;
};
int m = 3;
int n = m; // sequencial
```

## Definições Recursivas

```c
struct lista {
  int elemento;
  struct lista * proxima; // recursiva
};

float potencia (float x, int n) {
  if (n == 0) then {
    return 1.0;
  } else if (n < 0) {
    return 1.0/ potencia (x, -n); // recursiva
  } else {
    return x * potencia (x, n - 1); // recursiva
  }
}

void segunda (int);
void primeira (int n) {
  if (n < 0) return;
  segunda (n – 1); // recursiva
}
void segunda (int n) {
  if (n < 0) return;
  primeira (n – 1); // recursiva
}
```

```java
// Mutuamente Recursivo = loop infinito
class A {
  private b = new B;
}
class B {
  private a = new A;
}
```

