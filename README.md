# Java Exceções: aprenda a criar, lançar e controlar exceções

[Curso de controle de exceções da formação Java/Alura.](https://cursos.alura.com.br/course/java-excecoes)

[Principal](https://github.com/pvreboucas/java-excecoes/tree/main)

[Aula Anterior](https://github.com/pvreboucas/java-excecoes/tree/aula-3)


# Checked e Unchecked

1) Primeiro, vamos criar a classe MinhaExcecao que herda de RuntimeException:

```java
public class MinhaExcecao extends RuntimeException {
}
```

2) Na classe MinhaExcecao, vamos criar um construtor que recebe uma mensagem do tipo string e passaremos ela para o construtor da classe mãe RuntimeException:

```java
public class MinhaExcecao extends RuntimeException {
    public MinhaExcecao(String msg) {
        super(msg);
    }
}
```

3) Agora iremos lançar nossa exceção dentro do metodo2(), na classe Fluxo. Vamos substituir o throw atual pelo seguinte:

```java
throw new MinhaExcecao("deu muito errado");
```

4) Também precisamos adicionar o tipo MinhaExcecao dentro do catch na classe Fluxo:

```java
try {
    metodo1();
} catch(ArithmeticException | NullPointerException | MinhaExcecao ex) {
    String msg = ex.getMessage();
    System.out.println("Exception " + msg);
    ex.printStackTrace();
} 
```

5) Agora altera a classe MinhaExcecao para estender a classe Exception (deixando checked): 

```java
public class MinhaExcecao extends Exception { //checked

}
```

6) Na classe Fluxo, faça que o código volte a compilar e use throws MinhaExcecao no metodo1() e no metodo2():

```java
    private static void metodo1() throws MinhaExcecao {
        System.out.println("Ini do metodo1");
        metodo2();
        System.out.println("Fim do metodo1");
    }

    private static void metodo2() throws MinhaExcecao{
        System.out.println("Ini do metodo2");
        throw new MinhaExcecao("deu muito errado");
        //System.out.println("Fim do metodo2");        
    }
```

## Testando o Erro

No projeto java-pilha, se ainda não criou, crie uma nova classe FluxoComError com o seguinte conteúdo: 

```java
public class FluxoComError {

    public static void main(String[] args) {
        System.out.println("Ini do main");
        try{
            metodo1();
        } catch(ArithmeticException | NullPointerException ex) {
            String msg = ex.getMessage();
            System.out.println("Exception " + msg);
            ex.printStackTrace();
        } 
        System.out.println("Fim do main");
    }

    private static void metodo1() {
        System.out.println("Ini do metodo1");
        metodo2();
        System.out.println("Fim do metodo1");
    }

    private static void metodo2() {
        System.out.println("Ini do metodo 2");
        metodo2();    
        System.out.println("Fim do metodo 2");
    }
}
```

Repare que método2() chama a si mesmo. Isso também é chamado de recursão.

2) Ao executar a classe FluxoComError recebemos um Error. Você se lembra por quê?


# O que aprendemos?

* Existe uma hierarquia grande de classes que representam exceções. Por exemplo, ArithmeticException é filha de RuntimeException, que herda de Exception, que por sua vez é filha da classe mais ancestral das exceções, Throwable. Conhecer bem essa hierarquia significa saber utilizar exceções em sua aplicação.

* Throwable é a classe que precisa ser extendida para que seja possível jogar um objeto na pilha (através da palavra reservada throw)

* É na classe Throwable que temos praticamente todo o código relacionado às exceções, inclusive getMessage() e printStackTrace(). Todo o resto da hierarquia apenas possui algumas sobrecargas de construtores para comunicar mensagens específicas

* A hierarquia iniciada com a classe Throwable é dividida em exceções e erros. Exceções são usadas em códigos de aplicação. Erros são usados exclusivamente pela máquina virtual.

* Classes que herdam de Error são usadas para comunicar erros na máquina virtual. Desenvolvedores de aplicação não devem criar erros que herdam de Error.

* StackOverflowError é um erro da máquina virtual para informar que a pilha de execução não tem mais memória.

* Exceções são separadas em duas grandes categorias: aquelas que são obrigatoriamente verificadas pelo compilador e as que não são verificadas.

* As primeiras são denominadas checked e são criadas através do pertencimento a uma hierarquia que não passe por RuntimeException.

* As segundas são as unchecked, e são criadas como descendentes de RuntimeException.

[Próxima Aula](https://github.com/pvreboucas/java-excecoes/tree/aula-5)
