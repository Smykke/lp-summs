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

Segmentação de um programa em vários blocos relacionados pela lógica; evita escrever várias vezes o mesmo processo; realiza uma tarefa sempre preservando a interface com outros subprogramas.