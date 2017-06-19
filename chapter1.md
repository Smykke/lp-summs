# Capítulo 1
### Início das linguagens de programação

- As instruções eram para um computador específico (LPs de baixo nível)
- Não acompanhavam a complexidade das aplicações e eram muito simples, o que reduzia a produtividade dos programadores.

### Propriedades desejáveis em um software

- **Confiabilidade:** segurança contra erros e integridade dos dados manipulados;
- **Manutenabilidade:** facilidade de alteração do software;
- **Eficiência:** uso otimizado dos recursos computacionais

### Propriedades desejáveis em uma LP

- **Legibilidade:** Facilidade de leitura e compreensão. É prejudicada por comandos ambíguos, má indentação, desvios incondicionais e efeitos colaterais (linhas de código implícitas), palavras reservadas que podem ser utilizadas como nomes de variáveis;

```c
*p = (*p)*q;

// 1º asterisco: endereço da célula apontada por p
// 2º asterisco: valor contido na memória apontada por p
// 3º asterisco: multiplicação
```

```c
int x = 1;
int retornaCinco() {
  x = x + 3;
  return 5;
}
main() {
  int y;
  y = retornaCinco ();
  y = y + x;
}
// Se a pessoa não prestar atenção na função retornaCinco,
// não saberá que o valor de x é alterado no processo e o
// resultado não é 6, mas 9.
```

```c
if (x>1)
  if (x==2)
    x=3;
else
  x=4;

// O else é o if mais indentado, e não do primeiro.
```

- **Redigibilidade:** Concisão e praticidade na implementação do código, prejudicada por tipos de dados e comandos limitados, assim como a ausência de conversão implícita de tipos, falta de mecanismos de tratamento de erros;
```c
void f(char *q, char *p) {
  for (;*q=*p; q++,p++); // copia uma palavra p em loop infinito
}
```
- **Confiabilidade:**Capacidade de tratar possíveis erros e exceções, verificar erros de tipos, exigir declaração de variáveis;
```java
boolean u = true;
int v = 0;
while (u && v < 9) {
  v = u + 2; // trocou v por u
  if (v == 6) u = false;
}
```
```java
try {
  System.out.println(a[i]); // Java não deixa acessar índice fora do limite
  } catch (IndexOutofBoundsException) {
  System.out.println(“Erro de Indexação”);
}
```
- **Eficiência:** Agilidade durante a execução do código, relacionada ao fato de evitar-se verificações e processos desnecessários;
> Java, por exemplo, verifica o índice em todas as iterações de um vetor

- **Facilidade de aprendizado:** Pode ser comprometida quando há múltiplas maneiras de se realizar uma mesma funcionalidade;
- **Ortogonalidade:** Capacidade de usar-se os conceitos de uma linguagem sem que se produzam efeitos anômalos (deve haver poucas ou nenhuma exceção às regras gerais da mesma);

```java
int x, y = 2, z = 3;
byte a, b = 2, c = 3;
x = y + z;
a = b + c;    // erro

// No caso de Java, foi escolhido que a soma de int e byte
// não fossem ortogonais para não dar overflow no byte.
// No exemplo, para fazer a soma, b e c são convertidos para
// int e o resultado do cálculo é retornado em int. Logo,
// há um erro dizer que a (byte) = // b + c (int).
```
- **Reusabilidade:** Possibilidade de reutilização de código, relacionado aos conceitos de parametrização e modularização;
- **Modificabilidade:** Facilidade de alterar-se um programa em função de novos requisitos sem precisar alterar outras partes dele, relacionada ao uso de constantes;
- **Portabilidade:** Capacidade de comportamento idêntico independentemente do interpretador, compilador ou arquitetura de máquina utilizada (pode bater de frente com a eficiência).
> O int em C não tem o tamanho da memória definido. Logo, a alocação de memória
> em computadores de 32 e 64 bits é diferente.
> C prioriza a eficiência porque usa o tamanho da palavra definido pela arquitetura
> do computador, assim consegue fazer as operações aritméticas em hardware.
> Java tem tamanho definido porque faz as operações a nível de software e
> assim prioriza a portabilidade.


## Especificação de LPs
Toda linguagem é especificada a partir de seu **léxico**, de sua **sintaxe** e de sua **semântica**.
* **Léxico:** vocabulário utilizado para formar as sentenças;
* **Sintaxe:** regras para a combinação dos itens léxicos
* **Semântica:** interpretação das construções sintaticamente corretas

Uma notação comum para descrever gramáticas de LPs é a Backus-Naur Form – BNF, enquanto a semântica pode ser descrita segundo um enfoque informal, operacional (tradução para uma linguagem mais elementar, como assembly) ou denotacional (formal e rigoroso).

## Métodos de implementação de LPs
- **Compilação:** Tradução integral do programa fonte para o código de máquina. Otimiza a execução (as verificações de erros são realizadas previamente), porém apresenta problemas de portabilidade e de depuração de erros de execução (independência entre executável e código-fonte: não há registro dos números de linha onde o erro ocorreu. IDEs adicionam várias coisas ao código para possibilitar uma ajuda durante o debug);
- **Interpretação pura:** A tradução e a execução são simultâneas e acontecem à medida que se necessita de cada instrução. É flexível, portável e garante facilidade de prototipação ou depuração, porém consome muita memória;
- **Híbrido:** A compilação gera um código intermediário (bytecode, linguagem de máquina), o qual é posteriormente interpretado. Provê, simultaneamente, eficiência e portabilidade.

É importante lembrar que nenhuma LP é unicamente interpretada ou compilada, embora cada uma delas possua uma forma de implementação mais comum.

## Paradigmas de LPs
* **Paradigma Imperativo:** sequência de **alterações no estado da memória** do computador.
  * **Estruturado:** contorna a ideia de programas "macarrônicos", isto é, foca na abstração do controle da execução do programa. As *variáveis são células de memória*. Incentiva o uso de subprogramas e blocos aninhados de comandos. (C e PASCAL)
  * **Orientado a Objetos:** foca na abstração dos dados do programa. (C++, JAVA)
* **Paradigma Concorrente:** vários processos executam simultaneamente e concorrem por recursos. (ADA)
* **Paradigma Declarativo:** foco na tarefa que deve ser realizada (ex.: funções matemáticas). As variáveis não são células de memória. O computador gerencia os recursos.
* **Paradigma Funcional:** basicamente, funções recebem listas e retornam um valor. As funções são valores de primeira classe (podem ser passadas para outras funções). (LISP, HASKELL)
* **Paradigma Lógico:**  relação entre fatos ou variáveis. Apresenta uma questão e os fatos são deduzidos e verificados. Isso requer *muito* recurso computacional porque é um tipo de linguagem muito poderoso, já que não é necessário dizer à máquina como resolver o problema, apenas informar o problema. (PROLOG)

