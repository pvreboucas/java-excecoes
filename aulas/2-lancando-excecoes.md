*[<< AULA ANTERIOR](https://github.com/pvreboucas/java-excecoes/edit/aula-3/aulas/1-revisao.md)*

A partir de agora, criaremos as nossas próprias exceções. Anteriormente, vimos que elas podem ocorrer quando algo inesperado acontece no código, por exemplo, uma divisão por zero ou uma referência nula.

Mas, sabemos que essas exceções não deveriam acontecer. Deveríamos programar de forma correta, evitando-as. Todavia, existem casos em que as exceções nos ajudarão muito.

Na classe Conta do projeto criado anteriormente, temos o método saca():

```java
public boolean saca(double valor) {
    if(this.saldo >= valor) {
        this.saldo -= valor;
        return true;
    } else {
        return false;
    }
}
```

O retorno do tipo boolean mostra se o saque deu certo ou não. Entretanto, há muitos outros motivos para que um saque não funcione. Saldo insuficiente, saques não permitidos aos domingos, saques não permitidos fora do horário comercial, entre outros.

O return false dessa função simplesmente diz que o saque não funcionou, sem especificar o motivo. Podemos pensar em uma forma mais fácil para descrever os motivos, trocando o tipo de retorno de boolean para int. Assim, podemos atribuir:

* número 1 se saca() funcionar;
* um valor negativo para especificar o motivo, se o saca() não funcionar. Por exemplo:
  * -1 pode representar domingo;
  * -2 pode representar sábado;
  * -3 pode representar o limite diário e assim por diante. 

Mas essa programação voltada ao primitivo não cheira muito bem. No mundo Java, exceções possuem nomes, são objetos e classes!

Para resolver o problema do saca(), as exceções podem ser uma boa solução. Voltando ao projeto java-pilha, criaremos uma cópia da classe Fluxo e vamos chamá-la de FluxoComTratamento.

Voltando à classe Fluxo, deixaremos try-catch em main, mas alteraremos o metodo2(). Apagaremos o laço e deixaremos somente os System.out.println():

```java
private static void metodo2() {
    System.out.println("Ini do metodo2");

    System.out.println("Fim do metodo2");
}
```

De volta ao try-catch, usaremos a referência ex para chamar algum método que se comunique com o objeto. Se ex é uma referência, então NullPointerException é um tipo baseado em uma classe.

Criaremos um objeto da classe ArithmeticException e o guardaremos na referência exception:

```java
private static void metodo2() {
    System.out.println("Ini do metodo2");

    ArithmeticException exception = new ArithmeticException();

    System.out.println("Fim do metodo2");
}
```

Se criamos um objeto da classe ArithmeticException, também criamos uma exceção? Não! Nós apenas criamos o objeto, e ainda falta mais um passo para isso.

A referência exception aponta para a ArithmeticException, que está no HEAP (memória de objetos). Precisamos falar para o Java pegar esse objeto, transformar em uma exceção e "jogar" na pilha. O verbo "jogar", em inglês, é "throw"! Então, vamos "jogar" o objeto a partir da referência exception:

```java
private static void metodo2() {
    System.out.println("Ini do metodo2");
    ArithmeticException exception = new ArithmeticException();
    throw exception ;
    System.out.println("Fim do metodo2");
}
```

O Java reconhece que, quando jogamos uma exceção, saímos abruptamente do código. Se isso acontece, jamais será possível executar a linha que mostra "Fim do metodo2". Por isso, comentaremos essa linha do bloco, deixando-o da seguinte forma:

```java
private static void metodo2() {
    System.out.println("Ini do metodo2");
    ArithmeticException exception = new ArithmeticException();
    throw exception ;
    //System.out.println("Fim do metodo2");
}
```

E vamos executar, obtendo a seguinte saída no console:

```java
Ini do main
Ini do metodo1
Ini do metodo2
Exception null
java.lang.ArithmeticException
        at Fluxo.metodo2(Fluxo.java:24)
        at Fluxo.metodo1(Fluxo.java:17)
        at Fluxo.main(Fluxo.java:6)
Fim do main
```

Perceba que não foi impresso "Fim do metodo1" e, por esse motivo, sabemos que saímos abruptamente do método. Quando main recebe ArithmeticException, pega essa exceção e mostra a mensagem. Passaremos para o construtor a mensagem "deu errado".

```java
private static void metodo2() {
    System.out.println("Ini do metodo2");
    ArithmeticException exception = new ArithmeticException("deu errado");
    throw exception ;
    //System.out.println("Fim do metodo2");
}
```

Legal! Será que conseguimos transformar qualquer objeto na pilha? Vamos criar um objeto do tipo Conta:

```java
private static void metodo2() {
    System.out.println("Ini do metodo2");
    Conta c = new Conta();
    ArithmeticException exception = new ArithmeticException("deu errado");
    throw c;
    //System.out.println("Fim do metodo2");
}
```

Note que não foi possível fazer o throw c na pilha, pois só é possível com exceções. Vamos apagar a instância de Conta.

Normalmente, quando queremos jogar uma exceção, fazemos isso de maneira mais enxuta, sem guardar a referência.

```java
private static void metodo2() {
    System.out.println("Ini do metodo2");
    throw new ArithmeticException("deu errado");
    //System.out.println("Fim do metodo2");
}
```

Essa é a forma mais comum que encontraremos no dia a dia. A seguir, voltaremos ao método saca() para resolver o seu problema, no qual não faz sentido usar ArithmeticException ou NullPointerException.


*[PRÓXIMA AULA >>](https://github.com/pvreboucas/java-excecoes/blob/aula-4/aulas/1-hierarquia-de-excecoes.md)*
