*[<< AULA ANTERIOR](https://github.com/pvreboucas/java-excecoes/edit/aula-5/aulas/3-sacando-com-checked-exception.md)*


Fecharemos o tópico de exceções, trabalhando um novo exemplo para entender mais um detalhe do tratamento de erro.

Quando escrevemos uma aplicação um pouco mais complexa, muito provável que não funcione sozinha. Ela depende de outras aplicações. O melhor e mais claro exemplo que temos, é o nosso próprio celular!

Todos os aplicativos do Android usam Java, entretanto eles não possuem todos os dados no próprio aplicativo. Assim, é necessário que o aplicativo acesse o servidor para pedir informações.

Nessa comunicação com o servidor, podem acontecer diversos problemas. Estamos falando de uma comunicação na internet e, durante o processo, pode ser que ocorram erros que a aplicação não pode prever. O certo é que ela saiba como lidar com isso.

A ideia é introduzir o tratamento de exceções como se funcionassem em uma comunicação real, de grande porte. Para isso, preparamos uma classe chamada Conexao, a fim de simular uma conexão na rede e mostrar as dificuldades de se trabalhar com esses recursos relacionados com o tratamento de erros.

```java
public class Conexao {

    public Conexao() {
        System.out.println("Abrindo conexao");
    }

    public void leDados() {
        System.out.println("Recebendo dados");
            //throw new IllegalStateException();
    }

    public void fecha() {
        System.out.println("Fechando conexao");
    }
}
```

Vamos criar a classe no projeto, selecionando "New > Class", e colar o código acima.

Ótimo! Temos um construtor para simular uma abertura de conexão, um método para ler os dados do servidor e um método, não menos importante, para fechar a conexão.

Hora de testar! Criaremos uma nova classe com o método main(), que será chamada de TesteConexao:

```java
public class TesteConexao {
    public static void main(String[] args) {

    }
}
```

Qual é o nosso desafio? Ao trabalhar com conexões, precisamos realizar um tratamento de erro, como vamos tratar antecipadamente as exceções. Criaremos logo de cara, um try e, dentro dele, ficará a instância de Conexao.

```java
public static void main(String[] args) {
    try {
        Conexao con = new Conexao();
    }
}
```

A partir da conexão criada, podemos chamar o método leDados() e, depois, fecharemos a conexão com o método fecha():

```java
public static void main(String[] args) {
    try {
        Conexao con = new Conexao();
        con.leDados();
        con.fecha();
    }
}
```

Pode acontecer um erro, então é necessário criar um catch para pegar esse possível erro. Na classe Conexao, existe um comentário de uma classe que ainda não conhecemos, a IllegalStateException. Essa é uma exceção padrão do mundo Java que já existe e indica que o objeto utilizado tem um estado inconsistente.

O IllegalStateException estende de RuntimeException, ou seja, é uma exceção do tipo unchecked. Vamos tirar o comentário dessa linha e salvar.

Voltando na TesteConexao, faremos um catch de IllegalStateException:

```java
public static void main(String[] args) {
    try {
        Conexao con = new Conexao();
        con.leDados();
        con.fecha();
    } catch(IllegalStateException ex) {
        System.out.println("Deu erro na conexao");
    }
}
```

Quando trabalhamos com recursos de abrir conexões com servidores, é importante se preocupar com o fechamento desse recurso, ou seja, o recurso que você abre, você deve fechar! Se não tratarmos bem esse recurso do sistema operacional, todo o sistema pode ser afetado.

```
Esse é um conceito muito importante: Se abrimos uma conexão com o banco de dados, temos que fechá-la. 
```

Por esse motivo, chamamos o con.fecha() e, na teoria, tem que aparecer no console a mensagem "Fechando conexão", certo? Ao testarmos, essa é a mensagem que temos no console:

```
Abrindo conexao
Recebendo dados
Deu erro na conexao
```

Como podemos ver, deu erro na linha con.leDados(), então a execução parou, saiu abruptamente desse bloco, foi para o catch e não chegamos na linha con.fecha(). Por essa razão, chamaremos o método que fecha a conexão dentro do catch.

Entretanto, não podemos simplesmente fechar a conexão chamando o método a partir da variável con, pois ela só é visível dentro de try.

Faremos o seguinte: declaremos uma variável nula do tipo Conexao e, dentro do bloco try, vamos inicializá-la.

```java
public static void main(String[] args) {
    Conexao con = null;
    try {
        con = new Conexao();
        con.leDados();
        con.fecha();
    } catch(IllegalStateException ex) {
        System.out.println("Deu erro na conexao");
        con.fecha();
    }
}
```

Ao testarmos, teremos as seguintes mensagens no console:

```java
Abrindo conexao
Recebendo dados
Deu erro na conexao
Fechando conexao
```

Funcionou! Conseguimos fechar a conexão, mesmo dando erro. Repare que chamamos o método fecha() duas vezes, e isso não é muito legal.

Nossa intenção é que a conexão feche a qualquer custo, dando erro ou não. Mas, o try-catch tem um lugar próprio para isso. Estamos falando do bloco finally, um bloco opcional que podemos colocar no final e que sempre será executado, com ou sem erro. Dessa forma, não será mais necessário repetir o código para fechar a conexão.

```java
public static void main(String[] args) {
    Conexao con = null;
    try {
        con = new Conexao();
        con.leDados();
    } catch(IllegalStateException ex) {
        System.out.println("Deu erro na conexao");
    } finally {
        con.fecha();
    }
}
```

A questão é que não tem como fugir do finally, se ele estiver no código, sempre será executado. Ao executar o código, novamente, com as mudanças, veremos que não houve alteração. Foi exibida a mensagem "Fechando conexao", mesmo com o erro.

Muito bem! Agora, repare que a abertura e o fechamento de conexão se tornou um código difícil de ler. Não temos muita lógica aqui. Por isso, é interessante simplificar esse código, considerando que existe mais um problema, que veremos a seguir.

*[PRÓXIMA AULA >>](https://github.com/pvreboucas/java-excecoes/blob/aula-6/aulas/2-try-with-resources.md)*
