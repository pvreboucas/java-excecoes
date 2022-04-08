*[<< AULA ANTERIOR](https://github.com/pvreboucas/java-excecoes/blob/aula-2/aulas/1-introducao-a-excecoes.md)*

Continuaremos a nossa viagem por Java e suas exceções, nos aprofundando cada vez mais no assunto.

Anteriormente, concluímos que exceções possuem nomes, e elas acontecem porque algo deu errado ou inesperado, podendo até mudar o fluxo de execução da aplicação.

![01](https://github.com/pvreboucas/java-excecoes/blob/aula-2/aulas/imagens/02.01_002_diagrama-bomba-excecao.png)

De acordo com o diagrama acima, a exceção pode ser vista como uma bomba em cima da pilha. Se o método não souber lidar com ela, ele será descartado e, assim, a exceção muda o fluxo de execução.

Para tratarmos uma exceção, é preciso sinalizar para a máquina virtual que isso pode acontecer, por meio de um código específico (try). Assim, ela entenderá que deve tentar executar esse código, entre chaves ({}) e com mais cautela.

```java
private static void metodo2() {
    System.out.println("Ini do metodo2");
    for(int i = 1; i <= 5; i++) {
        System.out.println(i);
        try {
            int a = i / 0;
        }
    }
}
```

Ainda dessa forma, o código nem compila, porque precisamos avisar para a máquina virtual qual exceção pode acontecer. Para isso, utilizaremos um novo bloco de código, por meio de catch, sinalizando que queremos capturar um problema. No caso, ArithmeticException.

```java
try {
    int a = i / 0;
} catch (ArithmeticException ex) {

}
```
```
Depois de colocar o nome da exceção, colocamos uma variável.
```

Dessa forma, o bloco de try sinaliza que o código int a = i / 0 é perigoso e, em caso de exceção, a capturaremos e executaremos no bloco seguinte, por meio de catch. Dentro dele, imprimiremos o seguinte:

```java
try {
    int a = i / 0;
} catch (ArithmeticException ex) {
    System.out.println("ArithmeticException");
}
```

Salvamos e veremos no console qual será a saída com o tryCatch. Teremos o seguinte resultado:

```java
Ini do main
Ini do metodo1
Ini do metodo2
1
ArithmeticException
2
ArithmeticException
3
ArithmeticException
4
ArithmeticException
5
ArithmeticException
Fim do metodo2
Fim do metodo1
Fim do main
```

Note que agora temos o código que sabe como resolver a "bomba". A exceção foi capturada por catch e, no console, foi impresso System.out.println("ArithmeticException"). No entanto, logo depois apareceu o 2, ou seja, a máquina virtual continuou com o laço e voltou para a execução normal. Isso é o que acontece em cada iteração. Jogamos cinco "bombas" em cima da pilha e, por meio de catch, voltamos à execução normal.

Vimos a sintaxe básica do tryCatch. Adiante, estudaremos outras variações. O importante é que já sabemos como resolver alguma exceção, utilizando tryCatch. No entanto, não queremos mais que ele fique dentro do laço, considerando que ele não precisa estar lá. Vamos alterá-lo de lugar, transferindo-o na hora de chamar o metodo2():

```java
private static void metodo1() {
    System.out.println("Ini do metodo1");
    try {
        metodo2();
    } catch(ArithmeticException ex) {
        System.out.println("ArithmeticException");
    }
    System.out.println("Fim do metodo1");
}
```

Com essas mudanças, é interessante pensar na saída. Vamos executar!

```javaIni do main
Ini do metodo1
Ini do metodo2
1
ArithmeticException
Fim do metodo1
Fim do main
```

Repare que houve erro, na primeira iteração do laço dentro do metodo2(). Temos algum código no metodo2() que saiba como resolver a "bomba" que foi jogada? Não! Então, o Java saiu abruptamente da linha int a = i / 0 e voltou para a chamada do metodo2(), dentro do tryCatch. Repare que na saída não apareceu "Fim do metodo2", porque ele foi descartado. E então, voltamos para o metodo1(), no qual temos um código para resolver ArithmeticException.

Capturamos a exceção (a bomba) da pilha, e imprimimos "ArithmeticException" e, logo depois, voltou à execução normal, imprimindo "Fim do metodo1" e "Fim do main".

A mesma situação pode ser aplicada em main! Veremos adiante! ;)


*[PRÓXIMA AULA >>]()*
