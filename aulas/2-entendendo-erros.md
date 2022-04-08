*[<< AULA ANTERIOR](https://github.com/pvreboucas/java-excecoes/edit/aula-4/aulas/1-hierarquia-de-excecoes.md)*


Anteriormente, estudamos hierarquia e vimos que é a classe do topo — Throwable — quem realmente faz as coisas. Exception e RuntimeException não possuem utilidade e cada uma só possui alguns construtores. Mas mesmo assim, nós estendemos RuntimeException, assim como ArithmeticException e NullPointerException.

Mas, não seria mais fácil cortar caminho e estender diretamente o Throwable? Considerando que RuntimeException e Exception não possuem utilidade? Errado! Com certeza existe um porquê disso.

Existe uma outra hierarquia de classes que estende Throwable, como a classe Error. Entretanto, nós não a conhecemos muito bem, porque é uma hierarquia pensada para desenvolvedores de máquina virtual. Nós, desenvolvedores Java, não trabalhamos com essas classes diretamente. Quem cria e joga esses objetos na pilha é a máquina virtual, quando algo muito grave acontece.

A máquina virtual pode não conseguir mais executar o código porque não tem mais memória o suficiente, e então ela jogará um erro internamente. Vamos simular esse erro?

Voltando ao código, faremos uma cópia da classe Fluxo e vamos chamá-la de FluxoComError. Dentro dessa classe, apagaremos todo o corpo do metodo2() e o chamaremos, dentro dele mesmo! Assim:

```java
private static void metodo2() {
    System.out.println("ini do metodo 2");
    metodo2();
    System.out.println("fim do metodo 2");
}
```

Quando o Java entrar no metodo2(), imprimirá a mensagem "ini do metodo 2" e vai chamar de novo. A pilha irá crescer, pois o metodo2() ficará se chamando.

Ao executarmos esse código, vemos várias linhas de "ini do metodo 2", porque o método fica chamando ele mesmo. Essa ação se repete até não ter mais espaço na pilha. Nessa execução, temos um dos erros mais famosos, o StackOverflowError.

Aprendemos que existe uma hierarquia de classes utilizada internamente pela máquina virtual. Mas ainda temos as seguintes questões: 

* Qual a diferença entre Exception e RuntimeException??
* Por que não podemos eliminá-las?

Veremos as respostas, a seguir!

*[PRÓXIMA AULA >>]()*
