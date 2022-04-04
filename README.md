# Java Exceções: aprenda a criar, lançar e controlar exceções

[Curso de controle de exceções da formação Java/Alura.](https://cursos.alura.com.br/course/java-excecoes)

[Principal](https://github.com/pvreboucas/java-excecoes/tree/main)

[Aula Anterior](https://github.com/pvreboucas/java-excecoes/tree/aula-1)


# Tratamento de Exceções

Vamos forçar agora uma exceção em nossa classe Fluxo do projeto java-pilha.

1) Dentro do for do metodo2(), faça o seguinte:

```java
for(int i = 1; i <= 5; i++) {
    System.out.println(i);
    int a = i / 0;
}
```

Isso deverá retornar uma ArithmeticException!

2) Para evitar que a exceção caia na nossa pilha, vamos utilizar o bloco try e catch:

```java
try{
    int a = i / 0;
} catch(ArithmeticException ex) {
    System.out.println("ArithmeticException");
}
```

3) Porém, podemos fazer a utilização do try e catch durante a chamada do metodo2(), dentro do metodo1(), ficando do seguinte modo:

```java
try{
    metodo2();
} catch(ArithmeticException ex) {
    System.out.println("ArithmeticException");
}
```

4) Podemos subir mais uma vez o nosso try e catch para o nosso método main, no momento de chamada do metodo1().
 Além disso, podemos definir algumas coisas de nossa exceção, como pegar a mensagem:

```java
try{
    metodo1();
} catch(ArithmeticException ex) {
    String msg = ex.getMessage();
    System.out.println("ArithmeticException " + msg);
}
```

5) Outra coisa que podemos fazer é pegar o rastro da nossa exceção:

```java
try{
    metodo1();
} catch(ArithmeticException ex) {
    String msg = ex.getMessage();
    System.out.println("ArithmeticException " + msg);
    ex.printStackTrace();
}
```

6) Agora, vamos fazer outro teste. Para isso, crie a classe Conta do seguinte modo:

```java
public class Conta {

    void deposita() {}

}
```

7) E, dentro do for do metodo2(), faremos o seguinte teste:

```java
for(int i = 1; i <= 5; i++) {
    System.out.println(i);
    Conta c = null;
    c.deposita();
}
```

8) Mas a exceção causada é uma NullPointerException, para pegarmos essa exceção, precisamos colocá-la dentro do catch no método main:

```java
try {
    metodo1();
} catch(ArithmeticException | NullPointerException ex) {
    String msg = ex.getMessage();
    System.out.println("Exception " + msg);
    ex.printStackTrace();
}
```


# O que aprendemos?

* O que são exceções, para que servem e porquê utilizá-las.
* Como analisar o rastro de exceções, ou stacktrace.
* Tratar exceções com os blocos try-catch.
* Manipular uma exceção lançada dentro do bloco catch.
* Tratar múltiplas exceções com mais de um bloco catch ou usando Multi-Catch utilizando o pipe (|).

[Próxima Aula](https://github.com/pvreboucas/java-excecoes/tree/aula-3)
