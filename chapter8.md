# Capítulo 8

## Ausência de Exceções

- A linguagem não requer tratamento de possíveis erros;
- O programador pode misturar código de funcionlidades com o de tratamento de erro;
- `C`: alternativas:
  1. Abortar o programa
  2. Testar a condição antes de ocorrer;
  3. Criar uma flag
  4. `errno`: variável global para tratamento de Exceções.



## LPs com Exceções

### Tipos de Exceções

- `C++`: o programador pode acrescentar suas próprias exceções.

  ```c++
  try {
  // codigo que dispara excecoes
  } catch (ErroDiagnostico &e){
  	// trata apenas erro de diagnostico
  } catch (ErroCirurgia &e){
  	// trata apenas erro de cirurgia
  } catch (ErroMedico &e){
  	// trata qualquer erro medico
  } catch ( ... ) {
  	// trata qualquer outra excecao
  }
  ```

  ​

- `JAVA`: 

  - **Error:** problema grave; o programa não é executado até o fim; tratados pelo próprio JAVA (aborta).
  - **Exception:** podem ser tratadas pelo programador.

  ```java
  try {
  int num = Integer.valueOf(n).intValue();
  int den = Integer.valueOf(d).intValue();
  int resultado = num / den;
  } catch (NumberFormatException e){
  	System.out.println ("Erro na Formatacao ");
  } catch (ArithmeticException e){
  	System.out.println("Divisao por zero");
  } catch (Exception e){
  	System.out.println ("Qualquer outra Excecao");
  }
  ```



### Propagação de Exceções

```java
public static void main(String[] args) {
  System.out.println("Bloco 1");
  try {
    System.out.println("Bloco 2");
    try {
      System.out.println("Bloco 3");
      try {
        switch(Math.abs(new Random().nextInt())%4+1){
        	default:
            case 1: throw new NumberFormatException(); // Não casa com nenhum tratador, então será propagada para fora e o programa será encerrado.
            case 2: throw new EOFException();
            case 3: throw new NullPointerException();
            case 4: throw new IOException();
        }
      } catch (EOFException e) {
        System.out.println("Trata no bloco 3");
      }
    } catch (IOException e) {
      System.out.println("Trata no bloco 2");
    }
  } catch (NullPointerException e){
    System.out.println("Trata no bloco 1");
  }
}
```



### Especificação de Exceções

```c++
void f() throw (A,B,C); // Pode lançar exceções do tipo A, B ou C
void g() throw(); // Não pode lançar exceções
void h(); // Pode lançar qualquer tipo de exceção
```



### Modos de Continuação após Tratamento de Exceções

**Terminação:** erro crítico, então não retorna para onde gerou a interrupção. Volta para um ponto mais externo do programa. **->** Alternativa mais usada

**Retomada:** retorna para onde ocorreu a exceção. Não tem efetividade baixa.



- **Finally:** trecho de código que sempre é executado depois de um bloco *try-catch* independentemente de ocorrência de erro.
  - Um problema da cláusula `finally` é que, se ela jogar uma exceção, a exceção que ocorreu antes será ignorada.