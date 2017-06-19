# Capítulo 4

## Variáveis

Representa uma ou mais *células de memória* e armazenam um estado (valor), que pode ser atualizado sempre que necessário.

- **Nome:** definido pelo programador

- **Endereço:** posição na memória da primeira célula ocupada pela variável. Se duas ou mais variáveis de um programa apontam para o mesmo endereço, são chamadas de sinônimas (`aliases`).

  ```c
  char l;
  char *m;
  m = &l + 10; // acesso a uma área da memória que não deveria ser acessada
  printf(“&l = %p\nm = %p\n*m = %c\n“, &l, m, *m);
  m = &l // sinônimas
  ```

  ​

- **Tipo:** determina o conjunto de valores que a variável pode assumir.

- **Valor:** conteúdo da variável.

- **Tempo de Vida:** período em que existem células de memória alocadas para a variável. *Variável global:* toda a execução do programa. *Variável local:* existência do bloco onde ela foi declarada. *Variável dinâmica:* não tem regra, já que podem ser criadas e destruídas a qualquer momento. *Transientes:* limitadas pelo tempo de ativação do programa. *Persistentes:* continua existindo depois que o programa acaba.

- **Escopo de Visibilidade:** trecho do programa onde a variável é acessível.

  ```c
  int x = 15;
  main() {
    int x; // aqui, a variável global existe, mas não pode ser acessada
    x = 10;
    printf(“x = %d\n“, x);
  }
  ```

### Atualização de Variáveis Compostas

São compostas de outras variáveis.

```c
struct data { int d, m, a; };
struct data f = {7, 9, 1965}; // atualização completa
struct data g;
g = f; // atualização completa
g.m = 17; // atualização seletiva
```

## Constantes

Armazenam valores que não devem ser atualizados durante a execução do programa.

```c
char x = ‘g’;
int y = 3;
char* z = “bola”;
/* int *w = &3; */  // erro porque está pegando o endereço de uma constante pré-definida (proibido)
*z = ‘c’; // C permite pegar o endereço da constante com um ponteiro. Muito perigoso!
```

```c++
int* x;
const int y = 3;
const int* z;
// y = 4;  // proibido alterar o valor de uma constante
// y++;  // proibido alterar o valor da constante
// x = &y; // proibido referenciar uma constante com ponteiro comum
z = &y; // ok, porque o é ponteiro para constante
```

## Armazenamento de Variáveis e Constantes

### Armazenamento na Memória Principal

Para variáveis/constantes *transientes*. Sequência de bits dividida em blocos do tamanho da palavra do computador (8 bits, 16 bits...).

#### Alocação Estática

Identificar as variáveis e seus tamanhos, reservar um espaço igual à soma dos tamanhos: desperdiça memória, os tamanhos variam de acordo com a execução (usa o maior tamanho), aloca memória para elementos não utilizados, não sabe alocar para programas recursivos (porque não sabe a quantidade de iterações). ~ `FORTRAN`

#### Alocação dinâmica

Alocação quando for necessário (ocorre durante a execução do programa). 

* **Alocação de vetor contíguo:** não reutiliza áreas desalocadas ou, se utiliza, é custoso demais porque tem que mover o conteúdo das outras células.
* **Pilha e heap:** a cada entrada em um bloco/subprograma, um espaço no topo da pilha (acessado por um ponteiro) é alocado para ele. Quando o bloco/subprograma finaliza, a área é desalocada. Para as variáveis, há espaço na heap. Não há uma regra definida para organizar o monte.
  * Em caso de mudança de tamanho, pode-se alocar um novo espaço com tamanho suficiente e copiar os dados para lá. > mais  custoso
  * Ou alocar um outro espaço, guardar o complemento da informação lá e fazer um ponteiro da área original para esse espaço. > acesso  mais lento
  * Não se usa tudo no monte porque o controle no monte é feito por estruturas que guardam as áreas alocadas e não alocadas. Isso é  muito custoso.

#### Pilha de Registros de Ativação

Os Registros de Ativação (RA) de um subprograma possuem:

* **Constantes locais:** valores das constantes locais;
* **Variáveis locais:** valores das variáveis locais;
* **Parâmetros:** parâmetros do subprograma;
* **Link dinâmico:** referência para a base do RA anterior; sabe onde acessar as informações do RA após o fim do subprograma;
* **Link estático:** referência para a base do RA externo; para acesso às variáveis declaradas externamente;
* **Endereço de retorno:** aponta para a próxima instrução a ser executada

```ada
procedure Principal is
	x: integer;	
	procedure Sub1 is
		a, b, c: integer;
		procedure Sub2 is
			a, d: integer;
			begin --Sub2
              a := b + c;
              d := 8;					-- 1
              if a < d then
                  b := 6;
                  Sub2;					-- 2
              end if;
			end Sub2;
		procedure Sub3 (x: integer) is
			b, e: integer;
			procedure Sub4 is
				c, e: integer;
				begin --Sub4
                  c := 6; e:= 7;
                  Sub2;					-- 3
                  e := b + a;
				end Sub4;
			begin --Sub3
				b := 4; e := 5;
				Sub4;					-- 4
				b := a + b + e;
			end Sub3;
		begin --Sub1
			a := 1; b:= 2; c:= 3;
			Sub3(19);					-- 5
		end Sub1;
	begin --Principal
		x := 0;
		Sub1;							-- 6
	end Principal;
```

*Os números comentados são os endereços de retorno fictícios do programa.*

Ao passar pelo número 1, a pilha deve estar assim:

![4-pilha-ada](/home/marcela/Dropbox/LP/4-pilha-ada.png)

#### Gerenciamento do Monte

1. **LED e LEO:** Lista de Espaços Disponíveis e Lista de Espaços Ocupados. Contem local de início e tamanho das células de memória.  Pode alocar o primeiro encontrado, o de tamanho mais próximo ou de maior tamanho.  Isso influencia na fragmentação da memória.

2. **Desalocação por conta do programador:** menos confiável e mais trabalhosa; pode causar vazamento de memória. (`C` e `C++` )

3. **Desalocação por conta da LP:** uso de coletores de lixo. LP complexa e menos eficiente (tem que percorrer todas as estruturas para verificar se estão em uso). 

   > Java aloca de maneira contígua e usa coletor de lixo para limpar aquilo que não é do programa.

### Armazenamento na Memória Secundária

#### Arquivos

* **Seriais vs. Diretos:** o primeiro é acessado sequencialmente.O segundo, não tem regra.

  ```c
  #include <stdio.h>
  struct data {
  	int d, m, a;
  };
  struct data d = {7, 9, 1999};
  struct data e;
  main() {
    FILE *p;
    char str[30];
    printf("\n\n Entre com o nome do arquivo:\n");
    105}
    gets(str);
    if (!(p = fopen(str,"w"))) {
    printf ("Erro! Impossivel abrir o arquivo!\n"); exit(1);
    }
    fwrite (&d, sizeof(struct data), 1, p); // transforma 'd' em uma cadeia de bytes
    fclose(p);
    p = fopen(str,"r");
    fread (&e, sizeof(struct data), 1, p); // converte essa cadeia de bytes em uma variável (e)
    fclose (p);
    printf ("%d/%d/%d\n", e.a, e.m, e.d); // acessa de modo direto as informações de e
  // se o acesso das componentes de data fossem realizadas no arquivo, seria necessário posicionar o cursor
  }
  ```

* Por banco de dados, as variáveis são salvas como registros de tabelas. (`SQL`)

  ```java
  int idadeMaxima = 50;
  Statement comando = conexao.createStatement();
  ResultSet resultados = comando.executeQuery (
    "SELECT nome, idade, nascimento FROM pessoa " +
    "WHERE idade < " + idadeMaxima);
  // salva em 'resultados' as informações de pessoas. Filtra a idade por idadeMaxima; comandos em SQL
  );
  while (resultados.next()) {
    String nome = resultados.getString("nome");
    int idade = resultados.getInt("idade");
  106}
  ```

#### Persistência Ortogonal

LPs que permitem o uso de todos os tipos tanto para variáveis transientes quanto para persistentes. Isso demanda muito recurso computacional.

#### Serialização

Consiste em converter uma variável transiente em uma sequência de bytes na memória secundária. Os endereços são relativizados. Isso permite que a variável seja resgatada posteriormente (valor e relações existentes com outras variáveis). É um processo muito custoso e complexo.

> Em Java, utiliza-se a interface `Serializable`, que permite salvar e recuperar objetos. C++ deixa por conta do programador.

```java
public class Impares implements Serializable // cria uma classe que pode usar os recursos de serialização

  // Escrita
ObjectOutputStream out = new ObjectOutputStream(new FileOutputStream("impares.dat")); // define o arquivo binário
out.writeObject("Armazena Impares") // imprime uma string serializada
out.writeObject(w) // imprime um objeto serializado

  // Leitura
ObjectInputStream in = new ObjectInputStream(new FileOutputStream("impares.dat")); // define o arquivo binário a ser lido
String s = (String)in.readObject(); // lê uma string serializada
Impares z = (Impares)in.readObject(); // lê um objeto serializado
```



