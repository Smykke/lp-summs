# Capítulo 5

## Expressões

Expressão é uma frase do programa que precisa ser avaliada e produz um resultado (valor).

> *a = 2* :: Operando: a e 2; Operador: =; Resultado: atribui 2 à variável.

* Simples (1 operador) ou compostas (2 ou mais operadores);
* **Aridade:** número de operandos necessários;
* Pré-existentes ou criados pelos programadores por meio de funções;
* Notação **prefixada** (+ a 2), **infixada** (a + 2) ou **posfixada** (a 2 +);

### Tipos de Expressões

* **Literais:** mais simples. Produz valor fixo, de um tipo e sem operador explícito;

  ```c
  2.72	'c'		99
  ```

* **Agregação:** valor composto, marcadores de início e fim, separadores;

  ```c
  int c[] = {1, 2, 3}
  char *x = "abc"
    
  void f(int i) {
    int a[] = {3 + 5, 2, 16/4}; // avaliação estática
    int b[] = {3*i, 4*i, 5*i}; // avaliação dinâmica
    int c[] = {i + 2, 3 + 4, 2*i}; // mista
  }
  ```

  > **C** não permite expressão de agregação para atribuir valor a  uma estrutura ou vetor.

* **Aritméticas:** semelhantes às da matemática. 

  ```c
  float f;
  int num = 9;
  f = num/6;  // f = 1.0
  f = num/6.0; // f = 1.5
  ```

* **Relacionais:** comparação de valores.Normalmente, retorna um booleano.

  ```c
  ==	!=	>
  ```

* **Booleanas:** operandos podem ser booleanos ou equivalentes (se suportado pela LP); o resultado é um valor booleano.

  ```c
  !	&&	||
  ```

* **Binárias:** operações sobre dígitos binários; criação de programas mais eficientes.

  ```c
  main() {
    int j = 10;
    ...char c = 2;
    printf(“%d\n”, ~0);		// 1
    printf(“%d\n”, j & c);	// 2
    printf(“%d\n”, j | c);	// 10
    printf(“%d\n”, j ^ c);	// 8
    printf(“%d\n”, j << c);	// 40
    printf(“%d\n”, j >> c);	// 2
  }
  ```

* **Condicionais:** são compostas de várias subexpressões e retornam o valor resultante delas. 

  ```ml
  // if tem 3 subexpressões
  val c = if a > b then a - 3 else b + 5

  // case tem 5 expressões (i, if, 2*j, 3*je j)
  val k = case i of
  1 => if j > 5 then j - 8 else j + 5
  | 2 => 2*j
  | 3 => 3*j
  | _ => j
  ```

  ```java
  // ? é um operador ternário; logo, há uma expressão na condicional:
  max = x > y ? x : y;
  par = z % 2 == 0 ? true : false;
  ```

* **Chamadas de Funções:** podem ter qualquer aridade;  podem ser chamadas pelo indentificador da função ou por outras expressões (caso sejam valores de primeira classe na LP), inclusive ponteiros; algumas LPs admitem sobrecarga de operadores (que são entendidos como funções);

  >  **f(a)** Operador: f; Operando: a; Resultado: valor retornado

* **Efeitos Colaterais:** 

  ```c
  x = 2;
  y = 4;
  z = (y = 2 * x + 1) + y; // o compilador define a ordem que as atribuições serão feitas, então não há como determinar o valor de y

  fgetc(f) // faz leitura, mas também avança o ponteiro sobre f
  ```

* **Referenciamento:** acessam o conteúdo de variáveis/constantes ou retornam a referência a elas.

  ```c
  p[i] = p[i + 1]; // referência = valor
  *q = *q + 3;	// referência = valor
  r.ano = r.ano + 1;	// referência = valor
  s->dia = s->dia +1;	// referência = valor
  t = &m;	//	referência = referência
  int raio = 3	// referência
  ```

  ```java
  Ponto p = new Ponto() // referência
  ```

* **Categóricas:** realizam operações sobre tipos de dados, seja para extrair informações sobre aquele tipo (como o tamanho) ou para converter para outro tipo.

  ```c
  sizeof(float);
  int c; sizeof(c);
  float d = (float)c
  ```

  ```java
  if (c instanceof carro) // C++: typeid
  ```



### Avaliação de Expressões Compostas

* **Precedência de Operadores:** alguns operadores tem grau maior que outros e são processados primeiro; o programador tem que saber a precedência em cada LP; pode-se utilizar parênteses para mudar a ordem de avaliação (com uso controlado para não baixar a redigibilidade - escrever mais e fechar parênteses - e a legibilidade);

  ```pascal
  /* if a > 5 and b < 10 then */ // erro porque '5 and b' é avaliado primeiro
  if (a > 5) and (b < 10) then a := a + 1
  ```

* **Associatividade de Operadores:** quando não há regra de precedência ou os operadores tem a mesma precedência; define o sentido da avaliação (esq -> dir e vice-versa);

  ```c
  x = f() + g() + h()
   // pode causar problemas já que o compilador vai definir a ordem de execução
    // ex.: se f e g forem positivos grandes e somados primeiro -> overflow
  ```

* **Precedência de Operandos:** pode causa indeterminismo se não houver especificação da precedência de operandos;

  >  A especificação pode impedir o compilador de gerar código mais eficiente. Logo, **C** não define. Em **JAVA**, é sempre da esquerda para a direita. Garante que a avaliação será a mesma em qualquer máquina (portabilidade).

  ```c
  a[i] = i++
    // não se sabe se a[i] retornará o endereço da posição 'i' ou se 'i' será incrementado primeiro
  ```

  ```java
  x =  2; y = 4;
  z = (y = 2 * x + 1) + y; // z = 10
  ```

* **Curto Circuito:** o resultado é determinado antes que toda a expressão tenha sido avaliada; muito custoso para implementar; muito usada para expressões booleanas.

  ```
  z = (x - y) * (a + b) * (c – d);
  -- se x = y, então não é necessário realizar as demais operações, porque z = 0
  ```

  ```java
  while (i < n && a[i] != v) i++;
  // o curto circuito impede que uma posição inválida de 'a' seja acessada
  ```

  ```c
  if (b < 2*c || a[i++] > c ) { a[i]++ };
  // a[i] retorna valores diferentes dependendo do resultado das expressões anteriores
  ```

  ​

## Comandos

Instruções que visam atualizar variáveis ou controlar o fluxo de controle do programa. 

### Tipos de Comandos

* **Atribuições:** 

  ```c
  if (a = 10) a+= 3; // é válido, mas inútil porque é sempre verdade. Deveria ser uma comparação (==)
    
  a = b = 0; // atribuição múltipla
  a *= 3; // atribuição composta (a = a*3)
  b = ++a; // atribuição unária (b = a + 1)
  b = a++; // atribuição unária (b = a, depois a = a+1)
  while ((ch = getchar()) != EOF) // atribuição expressão
  ```

  ```ml
  (if a < b then a else b) := 2; // atribuição condicional
  // dependendo da condição, 2 é atribuído a 'a' ou 'b'
  ```

* **Sequenciais:** o comando antecedente é executado antes do subsequente. Os comandos podem ser agrupados em blocos (C) ou dentro de outros comandos (ADA). 

* **Colaterais:** o compilador define a ordem de execução dos comandos; bom para  LPs que lidam com processamento paralelo.

* **Condicionais:** especifica caminhos alternativos para o fluxo de controle; é importante utilizar marcadores de início e fim de condicional para não haver erro na análise.

  ```c
  if (x<0) // caminho condicionado
  if (x<0) ... else ... // caminho duplo condicionado
  switch (a) { // caminhos múltiplos = goto
    case 1: ...;
    case 2: ...
  }
  ```

* **Iterativos:** comando de repetição; `while`, `do-while`, `for`.

  > **C++** permite a definição da variável de controle no próprio comando. Ela é visível pelo bloco mais externo. Em **Java**, é limitado ao `for`.

* **Chamadas de Procedimentos:**  altera o valor de variáveis; aridade indefinida.

  * Procedimento vs. Função: a chamada de procedimento **não retorna valor**.

* **Desvios Incondicionais:** permitem fluxo  de controle de entrada única e  saída múltipla; desvios irrestritos, escapes e exceções.

  * **Desvios Irrestritos:** transfere a execução do programa para qualquer ponto especificado por meio de um rótulo (`goto`); não é tão interessante por produzir uma programação "macarrônica".

    > Não é um comando em **JAVA**, mas é uma palavra reservada.

    ```c
    // diferencial do goto: sair de comandos aninhados. É possível reescrever sem goto, mas com mais variável e testes
    for (i = 0; i < n; i++)
      for (j = 0; j < n; j++)
      	if ( a[i] == b[j] ) goto saida;
      saida:
    if (i < n) printf (“achou!!!”);
    else printf (“não achou!!!”);
    ```

  * **Escapes:** `break`, `continue`, `return`, `exit`, escapes rotulados.

    ```java
    saida:
    for (i = 0; i < n; i++) {
      for (j = 0; j < n; j++) {
        if ( a[i] < b[j] ) continue saida; // escape rotulado
        if ( a[i] == b[j] ) break saida; // escape rotulado
    	}
    }
    if (i < n) printf (“achou!!!”);
    else printf (“não achou!!!”);
    ```

    ​