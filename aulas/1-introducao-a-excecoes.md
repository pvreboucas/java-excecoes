Começaremos, de fato, o estudo sobre as exceções, um dos tópicos mais difíceis. Anteriormente, vimos a pilha de execução, e aprendemos que o método no topo dela, é o que está sendo executado.

## Exceções: O que são e para que servem?

As exceções são problemas que acontecem na hora de compilar o código. Considerando que existe uma variedade imensa, elas possuem nomes explicativos e, às vezes, mostram claramente o motivo de seu surgimento, facilitando a identificação delas.

No exemplo a seguir, usaremos classes e métodos vistos anteriormente. Suponhamos que você queira instanciar uma conta do tipo ContaCorrente, porém nula. Ou seja, sem agência e número de conta. Em seguida, depositaremos 200 reais nela:

```java

public class TesteContas {

    public static void main(String[] args) {

        //instancia para testar exceção!!!!
        ContaCorrente conta = null;
        outra.deposita(200.0);

        //instancia da conta corrente
        ContaCorrente cc = new ContaCorrente(111, 111);
        cc.deposita(100.0);

        //instancia da conta poupança
        ContaPoupanca cp = new ContaPoupanca(222, 222);
        cp.deposita(200.0);
    }
}
```

Se rodarmos essa classe, no console, aparecerá a seguinte mensagem:

```java
Exception in thread "main" java.lang.NullPointerException
        at TesteContas.main(TesteContas.java:7)
```

Podemos ver que essa referência não foi inicializada corretamente, e esse é um exemplo bem trivial. Em alguns casos, não conseguimos enxergar se essa referência foi inicializada de forma correta ou não, o que pode ocasionar a exibição desse problema no console.

Mas, quando isso acontece, o desenvolvedor precisa entender o que houve e o que ele fez de errado. Considerando que a exceção se chama NullPointerException e nós inicializamos a variável conta como nula, deduzimos que ela faz referência à variável.

Vamos analisar mais um exemplo. Dentro da main, colocaremos a seguinte operação:

```java
public class TesteContas {
    public static void main(String[] args) {

        int a = 3;
        int b = a / 0;
    }
}
```

O que vai acontecer? Lembrando que isso não dará erro de sintaxe.

Novamente algo será exibido no console:

```java
Exception in thread "main" java.lang.ArithmeticException: / by zero
        at TesteContas.main(TesteContas.java:7)
```

/ by zero, ou seja, nem o Java consegue dividir algo por zero. Esse tipo de problema também não deveria acontecer, mas em um ambiente mais complexo, é realmente mais difícil prever.

A primeira motivação é que erros e exceções acontecem. Portanto, precisamos saber lidar com eles. Vamos nos colocar em uma nova situação, ainda sobre exemplo anterior:

Na classe Conta, existe um método saca(), que recebe um valor como parâmetro.

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

Se funcionar, devolveremos true. Caso contrário, devolveremos false. Nessa implementação, partiremos do princípio de que existe apenas um motivo para que o saque não funcione: saldo insuficiente. Considerando um cenário real, existem mais motivos que impedem o saque. Pode ser o limite diário, o horário comercial, o banco pode estar fechado nesse momento e achar que nós poderíamos fraudar ou qualquer outro motivo.

Como podemos sinalizar para quem chamar o método saca(), que o saque não está funcionando por um motivo específico?

O método retorna somente "Funcionou" ou "Não funcionou", mas queremos que ele nos retorne motivos específicos quando não funcionar. Para isso, usamos as exceções.

Vamos voltar à classe Fluxo e ver como isso funciona junto à pilha.

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

Introduziremos um erro simples no metodo2() para nos ajudar a entender melhor. Declararemos uma nova variável e a dividiremos por zero:

```java
private static void metodo2() {
    System.out.println("Ini do metodo2");
    for(int i = 1; i <= 5; i++) {
        System.out.println(i);
        int a = i / 0;
    }
    System.out.println("Fim do metodo2");
}
```

Sabemos que não é uma boa ideia, mas é importante ver como realmente entendemos essa exceção.

Vamos executar clicando com o direito e selecionando "Run As > Java Application". No console, teremos a seguinte saída:

```java
Ini do main
Ini do metodo1
Ini do metodo2
1
Exception in thread "main" java.lang.ArithmeticException: / by zero
        at Fluxo.metodo2(Fluxo.java:19)
        at Fluxo.metodo1(Fluxo.java:11)
        at Fluxo.main(Fluxo.java:5)
```

Repare no que aparece abaixo de ArithmeticException. Existe algo familiar?

Exatamente! A pilha de execução. Em cima do main, temos o metodo1 e, acima dele, temos o metodo2.

![01](https://github.com/pvreboucas/java-excecoes/blob/aula-2/aulas/imagens/02.01_001_console-comparacao-execucao.png)

Para acessibilidade:

| Console (Execução Normal) | Console (Execução com Exceção) |
| :----:        |    :----:   |
| Header      | Title       |
| Paragraph   | Text        |
| Header      | Title       |
| Paragraph   | Text        |
| Header      | Title       |
| Paragraph   | Text        |
| Header      | Title       |
| Paragraph   | Text        |
| Header      | Title       |
| Paragraph   | Text        |
| Paragraph   | Text        |



Perceba que o Console de Execução com Exceção é semelhante ao Normal, até 1. Além da exibição do nome (ArithmeticException) e da mensagem (/ by zero), após 1, a execução muda — por causa da exceção — e é exibida a pilha de execução.

Assim, percebemos que as exceções mudam o fluxo, pois elas fazem parte do controle dele. Mas, por que o fluxo mudou? A partir do momento em que o metodo2() entrou no laço, apareceu a exceção "Não foi possível realizar a divisão por zero". Podemos imaginar que o Java jogou uma "bomba" em cima da pilha, ou seja, no método que está no topo dela.

![02](https://github.com/pvreboucas/java-excecoes/blob/aula-2/aulas/imagens/02.01_002_diagrama-bomba-excecao.png)

```


    Para acessibilidade: Podemos imaginar a pilha de execução, como um copo que recebe várias camadas, chamadas de métodos. A ordem de surgimento delas está de acordo com o método que é chamado. Como todo método é chamado a partir da main(), ele sempre estará no fundo do copo, pois é sempre o primeiro.

    A main(), por sua vez, chama o metodo1(), que chamará o metodo2(). Como o metodo2() foi o último a ser chamado, ele está no topo do copo. A bomba (exceção) cai no metodo2() e, como ele não tem nenhum bloco de código que possa tratar essa bomba, o metodo2() sai do copo, e cai na função anterior, ou seja, no metodo1().

    Por sua vez, o metodo1() também não possui o bloco de código para tratar essa exceção, e por isso, o método sai da pilha, transferindo a exceção para main(); que assim como os anteriores, não possui o tratamento para a exceção, sendo obrigada a sair da pilha de execução, que será jogada no console.

```

Então, fica a questão: será que, dentro desse método, existe algum código que consiga resolver a exceção ArithmeticException? Já escrevemos algum código que saiba resolver ArithmeticException? Não! Não aprendemos isso ainda.

Assim, o Java descarta o metodo2(), porque ele não consegue resolver. Agora, a "bomba" está em cima do metodo1(), mas assim como o anterior, esse método também não sabe resolver ArithmeticException. Mesmo sem finalizar, o Java também o descarta da pilha. Assim, a exceção chega ao método main(). No entanto, considerando que não existe um código para resolver o ArithmeticException ela é transferida para o console, no qual vimos o rastro dela.

Resumindo: sabemos que existem exceções e que elas mudam o fluxo. Se soluções não forem encontradas dentro da pilha de execução, elas serão impressas no console.

Adiante, veremos como lidar com essa "bomba", fazendo um tratamento das exceções.


