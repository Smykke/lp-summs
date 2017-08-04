# Capítulo 6

## Abstrações

Representação de fenômenos da realidade.

### Tipos de Abstrações

* **Abstração de Processos:** sobre o fluxo de controle; subprogramas.
* **Abstração de Dados:** sobre as estruturas de dados

## Técnicas de Modularização

* **Sistemas de pequeno porte:** compilar e recompilar o programa não tem muito custo;
* **Sistemas de grande porte:** muitas entidades, muitas linhas, vários arquivos fontes, recompilar usa muito recurso computacional; compilação apenas das partes modificadas, preferencialmente.
* **Módulo:** unidade de programa que pode ser compilada separadamente; reutilizáveis e modificáveis (sem esforço).

### Subprogramas

- Segmentação de um programa em vários blocos relacionados pela lógica;
- Evita escrever várias vezes o mesmo processo;
- Realiza uma tarefa sempre preservando a interface com outros subprogramas.

#### Passagem de Parâmetros

- **Cópia:** cria uma cópia do parâmetro real no escopo do subprograma. `C, C++, JAVA` (no caso de `JAVA`, apenas para tipos primitivos).
- **Referência:** cria uma referência para o parâmetro real no escopo do programa.
  - A passagem de ponteiros em `C` não é passagem por referência propriamente dita. Ainda é uma passagem por cópia.

```java
void f (T t1, T t2) {
t1 = t2;
  // t1 (que está no escopo da função e não o objeto passado na chamada) passa a referenciar t2
  // No fim da execução, esse valor se perde
}
```

```c
void f(T& t1, T& t2) {
t1 = t2;
  // O parâmetro real passa a ter o valor de t2 (outro parâmetro real)
}

// Exemplo:
a = 15; 	// 15 e 10 são objetos
b = 10;
igual(a, b);
> t1 = 15;	// Apontam para os objetos 15 e 10 referenciados por 'a' e 'b'
> t2 = 10;
......
a = 10;	// Apontam para o mesmo objeto
b = 10;
```



- **Modo normal:** avaliação dos parâmetros na chamada do subprograma;
- **Por nome:** avaliação em todos os momentos em que o parâmetro formal é usado;
- **Modo preguiçoso:** avaliação no primeiro momento em que o parâmetro formal é usado;

```C
int caso (int x, int w, int y, int z) {
  if (x < 0) return w;
  if (x > 0) return y;
  return z;
}
// Modo normal: avalia 'z' e 'y' sem precisar talvez.
// Por nome: execução repetida x (se for passada uma função); 
// Modo preguiçoso: avalia os valores apenas quando são usados e os valores são sempre iguais na mesma chamada da função 'caso' (mesmo que sejam passadas funções como parâmetro)
```

- **Verificação de tipos:** característica das linguagens ALGOL-like.
  - `C`: um caso em que não é feita verificação estática: `printf`.

```c
f(); // Pode ser chamada com qualquer tipo de parâmetro
f(void); // Deve ser chamada sem parâmatros
```

```java
/*
		A
	  /	  \
   	 B	   C
*/

A a = new B();
C c = (c) a; // Erro! ClassCastException. É um caso de verificação dinâmica em JAVA.

int[] v = new int[100];
int j = 200;
v[j] = 10; // Erro! IndexOutOfBounds. Mais um caso de verificação dinâmica.
```



#### Tipos de Dados

- **Tipos Anônimos:** definidos na criação de variáveis e definição de parâmetros;  *Ex.: uma struct (sem typedef, ou seja, só pode ser usada uma vez com aquele nome)*
- **Tipos Simples:** combinar um conjunto de dados em uma única entidade. *Ex.: typedef struct*
  - Código reutilizável, redigível e legível;
  - A informação não  é protegida, usuário tem acesso livre aos dados internos do tipo;
- **Tipos Abstratos:** conjunto de valores com comportamento uniforme (variáveis + métodos).
  - Usuário só pode usar as operações definidas pelo implementador.
  - Operações construtoras, consultoras, atualizadoras e destrutoras.

```c
// Simulação de um TAD em C
#define max 100
typedef struct pilha {
  int elem[max];
  int topo;
} tPilha;

tPilha cria() { // Operação construtora
  tPilha p;
  p.topo = -1;
  return p;
}

int vazia(tPilha p) {
  return p.topo == -1;
}

tPilha empilha (tPilha p, int el) { // Operação atualizadora
  if (p.topo < max - 1 && el >= 0)
    p.elem[++p.topo] = el;
  return p;
}

tPilha obtemTopo(tPilha p) { // Operação consultora
  if (!vazia(p)) return p.elem[p.topo];
  return -1;
}

// Não há necessidade de uma operação destrutura porque a alocação deste TAD não é dinâmica
```

- *Se uma classe em C++ possuir um ponteiro...* deve-se implementear um construtor de cópia e sobrecarregar o operador de atribuição. Ao passar uma variável que é ponteiro, o compilador usa o construtor de cópia padrão (caso não seja definido outro), que só copia os valores da variável para a nova variável no interior da função. 	


- **Classes:** permitem que o código seja encapsulado e protegido (`C++` e `JAVA`);

```c++
// C++ possui construtor de cópia, operador de atribuição que pode ser sobrecarregado e construtor.
main() {
  tPilha p1, p2; 
  tPilha p3 = p1; // construtor de copia
  p3 = p2; // operador de atribuicao
}
```

* **Destrutor:** `C++` possui um destrutor responsável pela desalocação de um objeto da memória ou o programador pode fazer um para eliminar os elementos contidos no objeto que foram alocados anteriormente. Em `JAVA`, a desalocação é feita pelo coletor de lixo, então não é necessário fazer uma função específica para isso (mas é possível).
* **Classes em Python:** elas simulam os TADs - é possível acessar os parâmetros que estão dentro do objeto;
  - Não há garantia de inicialização porque alguma instância pode criar um atributo não declarado antes;

#### Pacotes

- **Bibliotecas:** variáveis + constantes + subprogramas;
- **Frameworks:** implementações parciais de um sistema;
- **Pacotes:** agrupam diversas  unidades computacionais e algumas delas são exportáveis (visíveis) para o usuário; servem para organizar o código e solucionar conflito de nome; são subdivisões superficiais.
  - `C++`: namespace; *Ex.: using namespace umaBiblioteca; umaBiblioteca::y = 20;*
  - `JAVA`: package; *Ex.: package umPacote; import umPacote*



### Modularização, Arquivos e Compilação Separada

- **Arquivo de interface:** declaradas ou definidas as entidades a serem exportadas;
- **Arquivos de implementação:** são definidas entidades declaradas e as usadas internamentet (não exportadas);
- A divisão desses dois arquivos permite uma compilação mais rápida porque é possível verificar erros pelo menor arquivo (o de interface) e compilar o mais pesado (implementação) quando necessário;
- `C++`: se for alterada a estrutura interna do TAD, é necessário compilar os dois arquivos novamente.
- `JAVA`: não é necessário recompilar os arquivos quando há alteração na estrutura do TAD ou na implementação das operações porque os objetos são alocados na *heap* por referência; logo, os objetos tem o mesmo tamanho - o de um ponteiro.

```c++
// Arquivo tPilha.h
~ definições das variáveis e métodos ~
// Arquivo tPilha.c
Pilha* global;
Pilha* a;
Pilha* b;
empilhar(global, a) // Adiciona o primeiro elemento
global.topo = b  // Erro! Não é possível acessar 'elemento'
```

**Vantagem de usar um ponteiro no Tipo Oculto:** após fazer alterações em tPilha.h, não seria necessário recompilar o tPilha.c, porque a variável criada continua com o mesmo tamanho, já que é um ponteiro. Se fosse uma estrutura de dados, o tamanho do registro de ativação na pilha mudaria (precisaria de mais espaço para atender às alterações).

- **Funções definidas no arquivo *.h* em `C++`:** quando são definidas dentro da classe (em vez de só colocar a a declaração), elas são interpretadas como funções *inline*, ou seja, funcionam de forma semelhante à `macro`. Em todo ponto em que a função é chamada, o compilador substitui a chamada da função pelo código/comando que ela representa.

**Diferença do tipo opaco no `C` e `C++`:** a definição do tipo, em `C` está no arquivo *.c*. Já em `C++`, a definição fica no *.h*.

- Inserir um atributo no arquivo *.h* de `C++` exige a recompilação dos dados do usuário (o arquivo principal, onde está a `main`) porque ele cria uma estrutura para guardar as informações; logo, precisará alocar mais espaço para esse novo atributo. Já em `C`, o tipo oculto é referenciado por um ponteiro. Em `JAVA`, não é preciso recompilar porque os objetos são referências, então só é necessário guardar um espaço suficiente para um ponteiro. Se for alterado algum comando dentro de um método, também não há necessidade de recompilar, pois o carregamento das classes é dinâmico, ou seja, a classe (inclusive a modificada) só é carregada quando utilizada. Então, a mudança não será notada até o momento do uso. Em `C` e `JAVA`, a recompilação é necessária ao alterar a interface (chamada da função/método).