# Java Exceções: aprenda a criar, lançar e controlar exceções

[Curso de controle de exceções da formação Java/Alura.](https://cursos.alura.com.br/course/java-excecoes)

[Principal](https://github.com/pvreboucas/java-excecoes/tree/main)


1) Comece criando o projeto no eclipse chamado java-pilha.

2) Em seguida, crie a classe Fluxo e copie o seguinte código para a classe:

```java
public class Fluxo {

    public static void main(String[] args) {
        System.out.println("Ini do main");
        metodo1();
        System.out.println("Fim do main");
    }

    private static void metodo1() {
        System.out.println("Ini do metodo1");
        metodo2();
        System.out.println("Fim do metodo1");
    }

    private static void metodo2() {
        System.out.println("Ini do metodo2");
        for(int i = 1; i <= 5; i++) {
            System.out.println(i);
        }
        System.out.println("Fim do metodo2");        
    }
}
```

3) Agora, analisaremos a pilha de execução. Como apresentado no vídeo, crie um ponto de parada (Breakpoint), por exemplo na linha que chama o metodo1().
Execute o programa no modo de depuração e use os comandos Step into e Step Over. Atenção à pilha de execução.

# O que aprendemos?

* O que é, para que serve e como funciona a pilha de execução.
* O que é depuração (debug) e para que serve.
* Como utilizar o Eclipse e sua perspectiva de debug.
* Como alternar entre perspectivas do Eclipse.


[Próxima Aula](https://github.com/pvreboucas/java-excecoes/tree/aula-2)
