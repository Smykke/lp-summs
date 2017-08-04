# Capítulo 7 - Polimorfismo

## Sistemas de Tipos

**Verificação de Tipos:** garantir que operandos (parâmetros) de um operador (subprogramas/funções) são de tipos compatíveis.

- Pode ser convertido para o tipo "correto" implicitamente.
- **LPs estaticamente tipadas:** verificação em *tempo de compilação*. Logo, o tipo das expressões  e operações pode ser identificado.
  - Mais confiabilidade; Restrigem a redigibilidade e reutilição do código.
- **LPs dinamicamente tipadas:** verificação em *tempo de execução*. O tipo de uma determinada variável, inclusive, pode variar durante a execução. 
  - Perdem eficiência por causa da verificação antes de cada operação. 
  - Gasta mais memória porque tem que guardar o tipo dos valores em cada execução do programa e o código necessário para verificação dos tipos.   ???
- **LPs fortemente tipadas:** devem possibilitar a detecção de qualquer erro. Utilizar verificação dinâmica quando for necessário. O restante, estática.
  - Melhor combinação de eficiência e flexibilidade, confiabilidade.
- **`C++` e `Java`:** em tempo de compilação (maior parte) e de execução.
- **`C`:** em tempo de compilação.
- `Python`: em tempo de execução.

**Inferência de Tipos:** 

- **LPs estaticamente tipadas:** não precisam necessariamente exigir declaração de tipos.
  - Aumenta a redigibilidade; reduz a legibilidade (difícil saber o tipo de uma função/variável), mais difícil de consertar erros

**Equivalência de Tipos:** algumas LPs não aceitam se um operando não for do tipo esperado, outras fazem conversão implícita e outras ainda funcionam em meio termo.

- **Estrutural:** conjunto de valores é o mesmo do tipo esperado pela operação.
- **Nominal:** os dois tipos possuem o mesmo nome.

```c
typedef float quilometros;
typedef float milhas;
quilometros converte (milhas m) {
	return 1.6093 * m;
}
main() {
milhas s = 200;
quilometros q = converte(s); // ambas
s = converte(q); // somente estrutural
// porque 's [milhas] = converte(q) [quilometros]', então são nomes diferentes
}
```

- **LPs monomórficas:** cada tipo deve ter suas próprias operações - e cada operação e variável deve ter apenas um tipo.
  - O programador tem que escrever muito (baixa redigibilidade);
- **LPs polimórficas:** as estruturas e algoritmos atuam sobre diversos tipos.



## Tipos de Polimorfismo

### Ad-hoc

Aplica-se às chamadas de um método. Aparenta reutilizar o código, mas apenas permite que o programador use o mesmo nome para o método. Na verdade, o programa identifica o tipo que está chamando o método e invoca o método adequado para aquele tipo específico.

**Coerção:** conversão implícita de tipos; Upcast e downcast.

- `Go` não tem coerção
- `Lua` tem por parâmetros

**Sobrecarga:** quando um operador/identificador designa mais de uma operação. *Ex.: soma de inteiros vs. soma de floats*

- `C++`: permite a sobrecarga de operadores e construtores (permite vários construtores para uma mesma classe);
- `Go` não tem sobrecarga também
- `Python` permite sobrecarregar os operadores (não tem de função)
- `Haskell` permite, além de sobrecarregar, criar novos operadores.

```c
// Não é permitido:
void f(int) {};
int f(int) {};
f(3); // o programa não sabe qual função deve chamar
```





### Universal

Quando uma estrutura de dados incorpora elementos de vários tipos e um mesmo código pode atuar sobre elementos de vários tipos.

**Paramétrico:** os métodos podem ser usados com vários tipos com um comportamento uniforme.

- `C++`: usa o mecanismo `template`. Serve tanto para criação de métodos quanto de tipos (*Ex.: tPilha*). No entanto, na hora de compilar, o compilador precisa do arquivo de interface para saber quais tipos associar à classe genérica e, assim, alocar corretamente o espaço necessário na memória (em caso de não usar ponteiros).
  - Neste caso, o compilador cria uma função para cada tipo usado no programa.
- `JAVA`: não tem o mesmo problema de `C++` porque usa pointeiros. (*Ex.: `Arraylist <Double>`*).

**Inclusão:** de linguagens OO. Os subtipos podem usar elementos do supertipo.

- **Herança:** a classe herdeira possui elementos e métodos iniciais e podem conter atributos e métodos adicionais. Aumenta a reusabilidade do código.
- **Ampliação:** upcasting.

```c++
Pessoa p, *q;
Empregado e, *r;
q = r;	// Todo empregado é uma pessoa
// r = q;	// Nem toda pessoa é um empregado
// p = e;	// Não são ponteiros, o tamanho na memória não é o mesmo
// e = p;	// Não é upcasting e não é ponteiro
```

- **Amarração Tardia de Tipos:** funciona em tempo de execução para identificar a classe correta. Confere maior flexibilidade para reutilização de código.

  ![amarracao-tardia](/home/marcela/Dropbox/LP/P2/amarracao-tardia.png)

  - `JAVA`: Usa mais recursos computacionais porque tem que seguir a cadeia de ponteiros que ligam as classes quando um método é chamado. É possível 'desligar' a amarração  tardia definindo o método como `final`.

  ```java
  class Pessoa {
    String nome;
    int idade;
    public Pessoa (String n, int i) {}
    public void aumentaIdade () {}
    public void imprime(){}
  }
  class Empregado extends Pessoa {
    float salario;
    Empregado (String n, int i, float s) {}
    public void imprime(){}
  }
  public class Cliente {
    public static void main(String[] args) {
    Pessoa p = new Empregado (“Rogerio”, 28, 1000.00);
    p.aumentaIdade(); // Da classe pessoa
    p.imprime();	// Da classe Empregado
    }
  }
  ```

  ​

  - `C++`: o programador escolhe se quer amarração tardia ou não. Logo,  dá opção ao programador de eficiência ou versatilidade. O método padrão é amarração estática. Para criar amarração tardia, tem que usar `virtual` no método.

  ```c++
  class Pessoa {
    public:
    void ler(){}
    virtual void imprimir() {} // Amarração tardia
  };
  ```

- **Classes Abstratas:** possuem membros, mas não instâncias. É "geral" e serve para especificar características comuns das subclasses, mas não oferece implementação (isso fica a cargo das subclasses). Elas podem ter construtores (não abstratos).

  - `JAVA`: usa `abstract`. Permite que nenhum método dentro dela seja abstrato. Uma `interface` é uma classe abstrata pura.
  - `C++`: a classe deve possuir pelo menos um método abstrato.

  ```c++
  class Militar { // Não é indicada como abstrata
    public:
    virtual void operacao()=0; // Por possuir um método abstrato (=0 e, necessariamente, declarado como virtual), se torna abstrata.
    void imprime { cout << “Militar”; }
  }
  ```

  As classes herdeiras devem implementar todos os métodos abstratos da classe abstrata ou ser abstratas também.

- **Estreitamento:** downcasting. Conversão da superclasse para subclasses. Não é  muito segura porque nem todo membro da superclasse é uma instância da subclasse. Logo, as operações específicas da subclasse podem ser imcompatíveis com o objeto.

  - `JAVA`: exige conversão implícita. Caso contrário, dá erro de compilação. Se estão na mesma linha de descendência, ocorre exceção (*ClassCastException*). Pode-se usar `instanceof` para testar se a conversão é válida.
  - `C++`: usa `dynamic_cast` (casting em tempo de execução), `static_cast` (casting em tempo de compilação) e `typeid` (igual ao *instanceof* de JAVA). Também permite fazer o casting direto (*Ex.: (UmaClasse)objeto*), mas é  muito perigoso.

  ```Java
  class UmaClasse {}
  class UmaSubclasse extends UmaClasse {}
  class OutraSubclasse extends UmaClasse{}

  os = (OutraSubclasse) us;  // Erro de compilação
  os = (OutraSubclasse) uc; // Exceção (estão na mesma linha)
  ```

  ```C
  struct A {
    ...;
  } x;
  struct B {
    ...;
  } y;

  x = y;   // Erro!
  x = (A)y; // Aceita
  x = static_cast <A*> y // Funciona em como o downcast de Java
  ```

- **Herança Múltipla:** quando uma classe herda mais de uma classe. Evita desperdício de objetos dentro de uma classe. No entanto, há desperdício de memória no caso das duas classes herdadas partirem da mesma superclasse, pois há cópia de atributos da superclasse.

  ![heranca-repetida](/home/marcela/Dropbox/LP/P2/heranca-repetida.png)

  - `JAVA`: a herança múltipla é feita de forma intermediária. Aceita `extends` de uma classe e `interface` de várias. O problema é o de reescrita, já que os métodos das interfaces devem ser implementados em cada classe que chama a interface.
  - `C++`: quando as duas classes herdadas tem um método com o mesmo nome, é necessário especificar de qual das duas é o método que se deseja usar.

  ```c++
  class ProfessorAluno: public Professor, public Aluno {
  public:
  	void imprime();
  };
  void ProfessorAluno::imprime() {
  	Aluno::imprime(); // Especifica que é o 'imprime' de Aluno e não de Professor
  }
  ```

  ```c++
  class Academico {
    int i;
    int m;
  public:
    int idade ( ) { return i; }
    int matricula ( ) { return m; }
  };
  class Professor: virtual public Academico { // Virtual não deixa que os astributos de Acadêmico sejam herdados duas vezes em ProfessorAluno
    float s;
  public:
    float salario ( ) { return s; }
  };
  class Aluno: virtual public Academico { // Virtual não deixa que os astributos de Acadêmico sejam herdados duas vezes em ProfessorAluno
    float coef;
  public:
    int coeficiente ( ) { return coef; }
  };
  class ProfessorAluno: public Professor, public Aluno {};
  ```



**Composição vs. Herança:** o primeiro segue a lógica *tem-um*. O segundo, *é-um*. Ex.: Honda Fit *é um* carro. O carro *tem um* motor.