# Java Exceções: aprenda a criar, lançar e controlar exceções

[Curso de controle de exceções da formação Java/Alura.](https://cursos.alura.com.br/course/java-excecoes)

[Principal](https://github.com/pvreboucas/java-excecoes/tree/main)

[Aula Anterior](https://github.com/pvreboucas/java-excecoes/tree/aula-5)


# Finally e Try with Resources

Vamos fazer alguns testes agora.

1) Comece criando a classe Conexao:

```java
public class Conexao{

    public Conexao() {
        System.out.println("Abrindo conexao");
    }

    public void leDados() {
        System.out.println("Recebendo dados");
//        throw new IllegalStateException();
    }

    public void fecha() {
        System.out.println("Fechando conexao");
    }
}
```

2) Agora, para testarmos nossa conexão, precisaremos criar outra classe, a TesteConexao. Não se esqueça do método main:

```java
public class TesteConexao {

    public static void main(String[] args) {

    }
}
```

3) Dentro do método main, de nossa classe recém-criada, colocaremos um bloco try e catch para fazermos uso da nossa conexão:

```java
try{
    Conexao con = new Conexao();
    con.leDados();
    con.fecha();
} catch(IllegalStateException ex){
    System.out.println("Deu erro na conexão");
}
```

Não esqueça de remover o comentário da linha respectiva dentro da classe Conexao.

4) Queremos fechar nossa conexão sempre, mesmo em caso de erros, então precisamos garantir que o método con.fecha() sempre seja chamado. Para isso, temos o bloco finally. Faça o seguinte:

```java
Conexao con = null;
try{
    con = new Conexao();
    con.leDados();
    con.fecha();
} catch(IllegalStateException ex){
    System.out.println("Deu erro na conexão");
} finally {
    con.fecha();
}
```

5) Ainda podemos melhorar o nosso código. Para isso, faremos a declaração da conexão dentro dos parênteses do try, aplicando o seguinte:

```java
try(Conexao conexao = new Conexao() ){

}
```

6) Por conta da declaração acima, precisamos fazer com que nossa classe Conexao implemente a interface AutoCloseable e o método close(). Então, deixaremos do seguinte modo:

```java
public class Conexao implements AutoCloseable{

    public Conexao() {
        System.out.println("Abrindo conexao");
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

7) Agora, faremos a chamada do método leDados(), dentro do nosso novo try, comentando o código antigo.

```java
try(Conexao conexao = new Conexao() ){
    conexao.leDados();
}
```

8) Ainda precisamos fazer o nosso novo catch também, que ficará do seguinte modo:

```java
try(Conexao conexao = new Conexao() ){
    conexao.leDados();
} catch(IllegalStateException ex){
    System.out.println("Deu erro na conexão");
}
```

## Exceções Padrão

No vídeo, usamos a exceção IllegalStateException, que faz parte da biblioteca do Java e indica que um objeto possui um estado inválido. Você já deve ter visto outras exceções famosas, como a NullPointerException. Ambos fazem parte das exceções padrões do mundo Java que o desenvolvedor precisa conhecer.

Existe outra exceção padrão importante que poderíamos ter usado no nosso projeto. Para contextualizar, faz sentido criar uma conta com uma agência que possui valor negativo? Por exemplo:

```java
Conta c = new ContaCorrente(-111, 222); //faz sentido? 
```

Não faz sentido nenhum. Por isso, poderíamos verificar os valores no construtor da classe. Caso o valor esteja errado, lançamos uma exceção. Qual? A IllegalArgumentException, que faz parte das exceções do biblioteca do Java:

```java
public abstract class Conta {

    //atributos omitidos

    public Conta(int agencia, int numero){

        if(agencia < 1) {
            throw new IllegalArgumentException("Agencia inválida");
        }

        if(numero < 1) {
            throw new IllegalArgumentException("Numero da conta inválido");
        }

        //resto do construtor foi omitido
    }
```

A IllegalArgumentException e IllegalStateException são duas exceções importantes, que o desenvolvedor Java deve usar. Em geral, quando faz sentido, use uma exceção padrão em vez de criar uma própria.

## Nomenclatura

No vídeo, usamos uma exceção com o nome SaldoInsuficienteException. Discutir nomes pode ser algo subjetivo e exige conhecimentos sobre o assunto. Ou seja, é pauta de longas discussões, mas acreditamos que um nome um pouco mais genérico para nossa exceção também seria uma solução adequada.

Por exemplo, a exceção poderia se chamar SacaException ou ContaException. Repare que usamos o nome do método ou da classe. Para detalhar mais o problema (valor do saldo, etc) podemos utilizar a mensagem da exceção, como já fizemos no curso:

```java
throw new SacaException("Valor invalido: Saldo: " + this.saldo + ", Valor: " + valor);
```

Dessa forma, caso tenha outro problema, basta alterar a mensagem.

De qualquer forma, saiba que encontrar o nome perfeito para as suas classes e métodos não é uma tarefa fácil e pode tomar o seu tempo. Em alguns casos, já encontramos nomes nas classes que deixaram claro que isso é apenas algo provisório e que deve ser alterado quando houver um consenso no nome.


# O que aprendemos?

* que existe um bloco finally, útil para o fechamento de recursos (como conexão); 

* quando há um bloco finally o bloco catch é opcional;

* que o bloco finally é sempre executado, sem ou com exceção;

* como usar o try-with-resources.

[Próximo Curso](https://github.com/pvreboucas/java-lang-object-string)
