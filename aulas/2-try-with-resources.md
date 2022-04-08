*[<< AULA ANTERIOR](https://github.com/pvreboucas/java-excecoes/edit/aula-6/aulas/1-finally.md)*


Estudaremos mais alguns detalhes sobre try, catch e finally. Um try sozinho nunca é válido. Ele exige pelo menos um catch ou um finally!

O código a seguir é válido, mesmo sem o catch:


```java
Conexao con = null;
try {
    con = new Conexao();
    con.leDados();
} finally {
    con.fecha();
}
```

Nesse caso, nós não queremos capturar a exceção em caso de erro. Queremos somente fechar a conexão. Se executarmos esse código, teremos a seguinte saída:


```java
Abrindo conexao
Recebendo dados
Fechando conexao
Exception in thread "main" java.lang.IllegalStateException
        at Conexao.leDados(Conexao.java:10)
        at TesteConexao.main(TesteConexao.java:9)
```

A exceção "explodiu" porque não temos um código para tratá-la. Então, a regra é que try deve ter zero, um ou mais catch. Podemos repetir catch, desde que as exceções sejam diferentes. Entretanto, o try pode ter um único finally.

Retomando o assunto pendente, concluímos que o código está, no mínimo, complexo para leitura. Além disso, encontramos um problema pior. Para entendê-lo, copiaremos a linha throw new IllegalStateException() e colaremos dentro do construtor Conexao(), após a linha que imprime "Abrindo conexão", a fim de criar um problema na construção do objeto. 


```java
public class Conexao {
    public Conexao() {
        System.out.println("Abrindo conexao");
        throw new IllegalStateException();
    }

    public void leDados() {
        System.out.println("Recebendo dados");
        throw new IllegalStateException();
    }

    // método fecha()
}
```

Isso significa que, se ele chamar o construtor, será jogada uma exceção. Consequentemente, o objeto nunca será criado e também não terá uma referência inicializada.

Ao executarmos novamente a classe TesteConexao, tentaremos abrir a conexão, sabendo que dará erro.

```java
Abrindo conexao
Deu erro na conexao
Exception in thread "main" java.lang.NullPointerException
        at TesteConexao.main(TesteConexao.java:13)
```

Falamos que finally sempre será executado, com ou sem erro. Realmente tentamos fechar a conexão, só que a conexão está nula, ela nunca foi inicializada. Consequentemente, recebemos NullPointerException.

O que deveríamos fazer para simplificar esse código? Devemos chamar a con.fecha() se a conexão for diferente de null.



```java
public static void main(String[] args) {
    Conexao con = null;
    try {
        con = new Conexao();
        con.leDados();
    } catch(IllegalStateException ex) {
        System.out.println("Deu erro na conexao");
    } finally {
        System.out.println("finally");
        if(con != null) {
            con.fecha();
        }
    }
}
```

Então, teremos a seguinte saída:


```java
Abrindo conexao
Deu erro na conexao
finally
```

O finally foi executado, porém ele não entrou no if e, com isso, a conexão não foi fechada porque ela realmente é nula.

Nós precisamos simplificar esse código, mas como faremos isso?

A simplificação que faremos a seguir só entrou no Java 1.7. A ideia é, na hora de usar o try, inicializarmos a variável dentro de (), juntando duas linhas em uma só:

```java
public static void main(String[] args) {

    try (Conexao conexao = new Conexao()) {

    }
}
```

Como se fosse um método, o try recebe a inicialização da variável conexao. Entretanto, esse código acima não compila, pois o Java exige que a Conexao siga um contrato, porque se colocarmos o cursor em cima da palavra que tem o erro, veremos que a classe Conexao precisa implementar uma interface chamada AutoCloseable.

```java
public class Conexao implements AutoCloseable {}
```

Essa é uma interface, e a ideia das interfaces é definir um contrato e, caso você "assine" um contrato, será obrigado a implementar o método. Nesse caso, o método que somos obrigados a implementar da interface, é o método close().

Então, em vez de chamar o método fecha(), chamaremos close(). Vamos apagar o fecha() e mover a linha que imprime para o novo método:

```java
public class Conexao implements AutoCloseable{
    public Conexao() {
        System.out.println("Abrindo conexao");
        throw new IllegalStateException();
    }

    public void leDados() {
        System.out.println("Recebendo dados");
        throw new IllegalStateException();
    }

    @Override
    public void close() {
        System.out.println("Fechando conexao");
    }
}
```

O AutoCloseable exige que tenhamos o método close(), mas podemos deixar o método menos perigoso, retirando o throws Exception. Assim, simplificaremos um pouco o código e não será necessário mais um tratamento de erro para quem faz a chamada.

Além disso, vamos focar no problema da conexão, na qual não há exceção na construção do objeto.

```java
    public Conexao() {
        System.out.println("Abrindo conexao");
        //throw new IllegalStateException();
    }
```

Voltamos ao TesteConexao e vemos que o problema foi resolvido. O novo try exige que a classe do objeto que está sendo construído, implemente a interface AutoCloseable. Mas antes de continuar, para evitar confusão, vamos comentar o nosso código anterior. Construiremos um novo try-catch.

Chamaremos o método leDados(), dentro do try:

```java
public static void main(String[] args) {

    try (Conexao conexao = new Conexao()) {
        conexao.leDados();
    }
}
```

Essas três novas linhas de código, se referem ao antigo bloco:

```java
//    Conexao con = null;
//    try {
//        con = new Conexao();
//        con.leDados();
//    }
```

Ao executar esse novo código, obteremos a seguinte saída:

```java
Abrindo conexao
Recebendo dados
Fechando conexao
Exception in thread "main" java.lang.IllegalStateException
        at Conexao.leDados(Conexao.java:11)
        at TesteConexao.main(TesteConexao.java:8)
```

Repare que a conexão foi fechada automaticamente. Então concluímos que essas três linhas do console se referem aos antigos blocos try e finally. Não precisamos mais escrever explicitamente o bloco finally, pois o novo try já nos garante que o recurso que está sendo aberto dessa forma será fechado automaticamente, desde que ele implemente a interface AutoCloseable.

Isso já foi feito, sem ter que nos preocupar com o fechamento da conexão. O que ainda não fizemos é o catch. Veja que a exceção aconteceu no método leDados() e caiu no console e ninguém resolveu esse problema. Vamos criá-lo agora! Lembrando que ele pode ou não ser adicionado, diferente de finally, que tem o fechamento do recurso.



```java
O try com finally é válido sem o catch.
```

```java
public static void main(String[] args) {

    try (Conexao conexao = new Conexao()) {
        conexao.leDados();
    } catch(IllegalStateException ex) {
        System.out.println("Deu erro na conexao");
    }
}
```

Vamos executar novamente para ver a diferença na execução:

```java
Abrindo conexao
Recebendo dados
Fechando conexao
Deu erro na conexao
```

Legal! Podemos concluir que essas cinco linhas novas substituem o código anterior e, por isso, o código ficou muito melhor. Vale a pena aprender esse novo try. Além disso, veremos quando acontece uma exceção durante a construção do objeto conexao. 

```java
Abrindo conexao
Deu erro na conexao
```

O close() não foi chamado porque o objeto nem conseguiu ser construído. Esse é um código mais recente que, talvez, precisará de manutenção, porém aprendemos a usar o try-catch de uma forma mais enxuta.

Agora é a hora de colocar os aprendizados em prática, fazendo os exercícios a seguir!


*[PRÓXIMA AULA >>](https://github.com/pvreboucas/java-excecoes/blob/aula-6/aulas/3-conclusao.md)*
