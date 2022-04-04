# Java Exceções: aprenda a criar, lançar e controlar exceções

[Curso de controle de exceções da formação Java/Alura.](https://cursos.alura.com.br/course/java-excecoes)

[Principal](https://github.com/pvreboucas/java-excecoes/tree/main)

[Aula Anterior](https://github.com/pvreboucas/java-excecoes/tree/aula-2)


# Lançando uma Exceção

Vamos lançar nossa primeira exceção! Para isso, siga os passos abaixo:

1) No projeto java-pilha, faça uma cópia da classe Fluxo, renomeando-a para FluxoComTratamento.

2) Na classe FluxoComTratamento, no metodo2, apague por completo o laço for.

3) Instancie uma ArithmeticException:

```java
ArithmeticException exception = new ArithmeticException();
```

4) Agora, lance a exceção:

```java
private static void metodo2() {
    System.out.println("Ini do metodo2");
    ArithmeticException exception = new ArithmeticException();
    throw exception;
}
```

5) Não é necessário guardar a exceção em uma referência, você pode lançá-la diretamente em uma linha só:

```java
private static void metodo2() {
    System.out.println("Ini do metodo2");
    throw new ArithmeticException();
}
```

6) E você também pode enviar uma mensagem por parâmetro para o construtor da exceção:

```java
private static void metodo2() {
    System.out.println("Ini do metodo2");
    throw new ArithmeticException("Deu erro");
}
```

# O que aprendemos?

* Como lançar exceções.
* Como atribuir uma mensagem à exceção.

[Próxima Aula](https://github.com/pvreboucas/java-excecoes/tree/aula-4)
