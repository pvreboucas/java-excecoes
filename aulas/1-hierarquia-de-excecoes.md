*[<< AULA ANTERIOR](https://github.com/pvreboucas/java-excecoes/blob/aula-3/aulas/2-lancando-excecoes.md)*

Anteriormente, comentamos sobre o problema do método saca() e que podemos criar a nossa própria exceção. Mas antes, é necessário aprendermos como as exceções se organizam internamente.

Com throw, jogamos o objeto da exceção na pilha, e isso só funciona com exceções. Mas, por quê? Vamos ver o que tem dentro da classe ArithmeticException., com "Ctrl" pressionado e clicando em cima do método, como se fosse um link.

```java
public class ArithmeticException extends RuntimeException {
    private static final long serialVersionUID = 2256477558314496007L;

    public ArithmeticException() {
        super();
    }

    public ArithmeticException(String s) {
        super(s);
    }
}
```

Aqui temos um ótimo exemplo de herança, não é mesmo? Temos também dois construtores: um vazio e outro que recebe uma String, que representa a mensagem. O primeiro super() significa que estamos subindo a hierarquia para chamar o construtor da classe mãe. Já o segundo, repassa a mensagem do tipo String do construtor para o construtor da classe mãe.

Também podemos encontrar nessa classe, um atributo estático serialVersionUID, que representa uma identificação da classe, um valor único. Mas isso não faz diferença para nós.

Chamaremos a classe mãe da ArithmeticException, a RuntimeException! Da mesma forma, com o "Ctrl" pressionado, clicamos em cima da classe. Encontraremos o seguinte:

```java
public class RuntimeException extends Exception {
    static final long serialVersionUID = -7034897190745766939L;

    public RuntimeException() {
        super();
    }

    public RuntimeException(String message) {
        super(message);
    }

    public RuntimeException(String message, Throwable cause) {
        super(message, cause);
    }

    public RuntimeException(Throwable cause) {
        super(cause);
    }

    protected RuntimeException(String message, Throwable cause, boolean enableSuppression, boolean writableStackTrace) {
        super(message, cause, enableSuppression, writableStackTrace);
    }
}
```

Repare que essa classe também possui uma classe mãe, a Exception, além de construtores. Voltando à classe Fluxo, vemos que não importa o tipo da referência ex, sendo ArithmeticException ou NullPointerException, nós conseguimos chamar o método getMessage(), mesmo que nenhum desses dois tipos tenha o método getMessage().

A classe RuntimeException também não tem esse método. A classe Exception tem vários construtores, mas nenhum sinal desse método. Essa classe estende ou herda de Throwable. Usamos a palavra throw para jogar o objeto da exceção na pilha e ela não funciona para classes e objetos que não sejam exceções. Isso acontece porque a nossa classe Conta não estende Throwable. Para jogar um objeto na pilha, a classe precisa estender Throwable. Vamos clicar nela para ver o que ela tem.

A classe Throwable implementa a interface Serializable que se relaciona com o atributo serialVersionUID, mas esse não é o nosso foco.

Ainda dentro de Throwable, encontramos o atributo detailMessage que especifica detalhes, as mensagens. Entretanto a classe Throwable é enorme, e nem tudo o que ela possui nos interessa. Por essa razão, para saber os membros dessa classe, utilizamos o atalho "Ctrl + O".

Em uma janela mais enxuta, conseguimos então visualizar os seus membros, os métodos e atributos. E então, encontramos os métodos getMessage() e printStackTrace(). Ou seja, todos os métodos do mundo de exceções foram implementados na classe mãe Throwable.

![01](https://github.com/pvreboucas/java-excecoes/blob/aula-4/aulas/imagens/04.01_001_pilha-java-throwable.png)

```


    Para acessibilidade: A hierarquia abordada possui uma classe mãe Throwable está no topo do diagrama e, dela, é formada uma hierarquia de exceções voltada para o desenvolvedor/desenvolvedora Java.

    As classes ArithmeticException e NullPointerException herdam de RuntimeException, considerando que nessa classe só existem construtores, e ela herda de Exception que também só possui construtores. Exception é a última classe que herda de Throwable.

```

Na hierarquia, objetos que fazem parte de qualquer classe que estenda Throwable, podem ser jogados na pilha. Mas, por que a classe ArithmeticException, por exemplo, não estende diretamente a Throwable? Veremos isso adiante.

A partir do diagrama, concluímos que para criarmos a nossa própria exceção, basta nos colocarmos dentro dessa hierarquia. Para começar, criaremos MinhaExcecao, que estende a classe RuntimeException.

Clicaremos com o botão direito no package e selecionaremos "New > Class > MinhaExcecao". Vamos colocá-la na hierarquia:

```java
public class MinhaExcecao extends RuntimeException {

}
```

Tudo o que é necessário para ser uma exceção, já está aqui! Vamos usá-la. Na classe Fluxo, dentro do metodo2(), faremos o seguinte:


```java
private static void metodo2() {
    System.out.println("Ini do metodo2");
    throw new MinhaExcecao("deu muito errado");

    //System.out.println("Fim do metodo2");
}
```

Repare que ainda não criamos nenhum construtor dentro de MinhaExcecao que recebe uma string. Entretanto, a exceção já funciona se tirarmos a string, em função da hierarquia. Então, vamos criar um construtor para receber uma mensagem.

```java
public class MinhaExcecao extends RuntimeException {
    public MinhaExcecao(String msg) {

    }
}
```
Agora, a classe Fluxo já compila! Mas perderemos a msg do construtor em MinhaExcecao. Daremos uma olhada na classe RuntimeException. Ela possui o construtor que recebe uma message. E note que ele chama o super e manda a mensagem para a classe mãe. Vamos fazer isso também! É uma boa prática para não perder a mensagem.


```java
public class MinhaExcecao extends RuntimeException {
    public MinhaExcecao(String msg) {
        super(msg);
    }
}
```

Voltando ao Fluxo, repare que a linha throw new MinhaExcecao("deu muito errado") está compilando novamente. Compilaremos essa classe para ver a exceção acontecendo.

```java
Ini do main
Ini do metodo1
Ini do metodo2
Exception in thread "main" MinhaExcecao: deu muito errado
        at Fluxo.metodo2(Fluxo.java:23)
        at Fluxo.metodo1(Fluxo.java:17)
        at Fluxo.main(Fluxo.java:6)
```

Legal! Temos o rastro da pilha e o nome da exceção iguais a ArithmeticException e NullPointerException. Mas uma coisa mudou. Repare que não apareceu "Fim do main". Essa linha não foi executada porque não capturamos a exceção!

Em catch, só estamos capturando as exceções ArithmeticException e NullPointerException, e não MinhaExcecao. Então, é preciso adicionar mais um pipe (|) para conseguirmos colocar mais um tipo de exceção:

```java
public static void main(String[] args) {
    System.out.println("Ini do main");
    try {
        metodo1();
    } catch(ArithmeticException | NullPointerException | MinhaExcecao ex) {
        String msg = ex.getMessage();
        System.out.println("Exception " + msg);
        ex.printStackTrace();
    }
    System.out.println("Fim do main");
}
```

Vamos testar? Antes, lembre-se de salvar e executar novamente!

```java
Ini do main
Ini do metodo1
Ini do metodo2
Exception deu muito errado;
MinhaExcecao: deu muito errado
        at Fluxo.metodo2(Fluxo.java:23)
        at Fluxo.metodo1(Fluxo.java:17)
        at Fluxo.main(Fluxo.java:6)
Fim do main
```

Agora, já sabemos como criar exceções. Em nosso projeto do Banco, daremos um nome melhor para deixar claro o que pode acontecer. Mas, antes de voltar realmente para resolver o problema da Conta, temos que aprender mais um pouco sobre a hierarquia e a diferença entre as classes RuntimeException e Exception.


*[PRÓXIMA AULA >>]()*
