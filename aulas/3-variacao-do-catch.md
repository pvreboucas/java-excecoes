*[<< AULA ANTERIOR](https://github.com/pvreboucas/java-excecoes/blob/aula-2/aulas/2-try-catch.md)*

Sabemos que a pilha de execução é fundamental para executar as exceções, e que elas existem porque algo anormal pode acontecer em nosso código, podendo ser intencional ou não.

Anteriormente, estudamos sobre o tratamento de exceções por meio de tryCatch, solucionando a "bomba" que está em cima da pilha.

Agora, mostraremos como ficou tryCatch feito na main(), assim como foi dito anteriormente.

```java
public static void main(String[] args) {
    System.out.println("Ini do main");
    try {
        metodo1();
    } catch(ArithmeticException ex) {
        System.out.println("ArithmeticException");
    }
    System.out.println("Fim do main");
}
```

Temos o seguinte resultado:

```java
Ini do main
Ini do metodo1
Ini do metodo2
1
ArithmeticException
Fim do main
```

Note que "Fim do metodo1" e "Fim do metodo2" não aparecem, porque main() é a única que possui código capaz de tratar a exceção. Então é exibido "Fim do main" e é finalizado.

Sabemos que temos uma exceção do tipo ArithmeticException. A variável ex é uma referência e, com isso, podemos dizer que exceções também são objetos. Então, podemos usar a referência para chamar algum método público da classe. Pegaremos o método getMessage(), com o qual conseguiremos pegar a informação apresentada no console, por exemplo, a mensagem / by zero. Depois de pegá-la, iremos guardá-la em uma String e mostrá-la após "ArithmeticException".

```java
public static void main(String[] args) {
    System.out.println("Ini do main");
    try {
        metodo1();
    } catch(ArithmeticException ex) {
        String msg = ex.getMessage();
        System.out.println("ArithmeticException " + msg);
    }
    System.out.println("Fim do main");
}
```

Executaremos o projeto para ver o resultado dessas alterações. Após a execução, no console teremos o seguinte:

```java
Ini do main
Ini do metodo1
Ini do metodo2
1
ArithmeticException / by zero
Fim do main
```

Há mais coisas que podemos fazer. Da mesma forma que a exceção se lembra da mensagem, ela também se lembra por onde passou e deixou seu rastro. Para mostrá-lo, usaremos o método printStackTrace():

```java
public static void main(String[] args) {
    System.out.println("Ini do main");
    try {
        metodo1();
    } catch(ArithmeticException ex) {
        //String msg = ex.getMessage();
        //System.out.println("ArithmeticException " + msg);
        ex.printStackTrace();
    }
    System.out.println("Fim do main");
}
```

Salvaremos e executaremos, obtendo o seguinte retorno, no console:

```java
Ini do main
Ini do metodo1
Ini do metodo2
1
java.lang.ArithmeticException: / by zero
        at Fluxo.metodo2(Fluxo.java:25)
        at Fluxo.metodo1(Fluxo.java:17)
        at Fluxo.main(Fluxo.java:6)
Fim do main
```

Dessa forma, entendemos que ex é uma referência e o tipo da referência é o nome da classe da exceção. Parte do tratamento dela é saber trabalhar com a referência. Normalmente, não precisamos saber muito sobre ex. Basta sabermos que getMessage() é um método importante para descobrir a mensagem original e que o printStackTrace() pode ser útil também. Ainda nesse curso, criaremos as nossas próprias exceções!

Agora, criaremos uma outra situação para fins didáticos!

Antes de começar, será necessário criar a classe Conta, com o método deposita() vazio. Na classe Fluxo, colocaremos um comentário na linha que gera o erro de divisão por zero.

Logo abaixo da linha comentada, criaremos uma referência da classe Conta nula, ou seja, ela apontará para nenhum objeto. E chamaremos o método deposita() a partir dela.

```java
private static void metodo2() {
    System.out.println("Ini do metodo2");
    for(int i = 1; i <= 5; i++) {
        System.out.println(i);
        //int a = i / 0;
        Conta c = null;
        c.deposita();
    }
    System.out.println("Fim do metodo2");
}
```

Vamos executar o código. Já sabemos que vai dar erro, pois a referência c está nula. A saída dessa execução é a seguinte:

```java
Ini do main
Ini do metodo1
Ini do metodo2
1
Exception in thread "main" java.lang.NullPointerException
            at Fluxo.metodo2(Fluxo.java:27)
            at Fluxo.metodo1(Fluxo.java:17)
            at Fluxo.main(Fluxo.java:6)
```

Sabemos que existe um código em main capaz de resolver a exceção ArithmeticException. Entretanto, repare que não apareceu "Fim do main" na saída, como no exemplo anterior. Ou seja, não conseguimos tirar a bomba da pilha por meio de catch(). Isso aconteceu porque fizemos um catch específico, que funciona somente para ArithmeticException, e o nosso problema se chama NullPointerException.

Para resolver, é preciso mudar o nome da exceção dentro de catch. Mas, sabendo que o código pode criar outros tipos de exceções, além de ArithmeticException, podemos manter a exceção desse catch e criar um segundo para a exceção NullPointerException.

```java
public static void main(String[] args) {
    System.out.println("Ini do main");
    try {
        metodo1();
    } catch(ArithmeticException ex) {
        //String msg = ex.getMessage();
        //System.out.println("ArithmeticException " + msg);
        ex.printStackTrace();
    } catch(NullPointerException ex) {
        String msg = ex.getMessage();
        System.out.println("NullPointerException " + msg);
    }
    System.out.println("Fim do main");
}
```
```
Qualquer exceção tem o método getMessage().
```

Executaremos novamente para ver a diferença.

```java
Ini do main
Ini do metodo1
Ini do metodo2
1
NullPointerException null
Fim do main
```

A mensagem retornada pelo getMessage() foi null. Ou seja, o segundo bloco catch foi chamado. Então, podemos ter quantos blocos catch quisermos, desde que eles sejam específicos o suficiente.

A partir do Java 1.7, chegou mais uma variação do catch. Em vez de repetir vários blocos de catch, podemos colocar um pipe (|), que significa "OU":

```java
catch(ArithmeticException | NullPointerException ex)
```

É uma facilidade que veio para não precisarmos repetir código. Utilizando-a, o bloco de código main() ficará da seguinte forma:

```java
public static void main(String[] args) {
    System.out.println("Ini do main");
    try {
        metodo1();
    } catch(ArithmeticException | NullPointerException ex) {
        String msg = ex.getMessage();
        System.out.println("Exception " + msg);
        ex.printStackTrace();
    }
}
```

O código acima é válido para as duas situações que trabalhamos. Adiante, falaremos mais sobre a classe Exception.


*[PRÓXIMA AULA >>](https://github.com/pvreboucas/java-excecoes/blob/aula-3/aulas/1-revisao.md)*
