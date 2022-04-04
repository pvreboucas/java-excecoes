# Java Exceções: aprenda a criar, lançar e controlar exceções

[Curso de controle de exceções da formação Java/Alura.](https://cursos.alura.com.br/course/java-excecoes)

[Principal](https://github.com/pvreboucas/java-excecoes/tree/main)

[Aula Anterior](https://github.com/pvreboucas/java-excecoes/tree/aula-4)


# Aplicando Exceções

## Catch Genérico

No projeto java-pilha, abra a classe Conta (aquela classe de teste). No método deposita(), lance a exceção que criamos anteriormente.

1) O corpo ficará desse jeito:

```java
void deposita() throws MinhaExcecao {
    //codigo omitido
}
```

Lembrando que a classe MinhaExcecao é checked.

2) Agora, para podermos testar nossa exceção, vamos criar a classe TestaContaComExcecaoChecked. Dentro dela, faremos uma chamada ao método deposita. Ao chamar o método, somos obrigados a tratar a exceção:

```java
public class TestaContaComExcecaoChecked {

    public static void main(String[] args) {

        Conta c = new Conta();
        try {
            c.deposita();
        } catch(MinhaExcecao ex) {
            System.out.println("tratamento ....");
        }

    }
}
```

3) Agora, abra a classe Fluxo. Similarmente, dentro do catch dessa classe, experimente o "catch genérico" usando apenas Exception:

```java
try {
    metodo1();
} catch(Exception ex) { //catch genérico, capturando qq exceção
    String msg = ex.getMessage();
    System.out.println("Exception " + msg);
    ex.printStackTrace();
}
```

4) Se você encontrar um erro de compilação na classe FluxoComError, pode ser por conta da exceção MinhaExcecao que é checked. Verifique se ainda está tratando essa exceção, dentro do catch, e apague essa parte. Vamos deixar do jeito antigo (sem MinhaExcecao):

```java
//na classe FluxoComError
try {
    metodo1();
} catch(ArithmeticException | NullPointerException ex) {
    String msg = ex.getMessage();
    System.out.println("Exception " + msg);
    ex.printStackTrace();
}
```

## Criando a Exceção

No projeto bytebank-herdado-conta, vamos refatorar o código da classe Conta e criar uma exceção.

1) Primeiramente, crie a nossa exceção SaldoInsuficienteException:

```java
public class SaldoInsuficienteException extends Exception{ //checked

    public SaldoInsuficienteException(String msg) {
        super(msg);
    }
}
```

2) Agora, abra a classe Conta. Procure o método saca() e troque o tipo do retorno de boolean para void. Remova os returns e lance a exceção nova. O método saca() terá a seguinte cara:

```java
public void saca(double valor) throws SaldoInsuficienteException{

        if(this.saldo < valor) {
            throw new SaldoInsuficienteException("Saldo: " + this.saldo + ", Valor: " + valor);
        } 

        this.saldo -= valor;       
}
```

Repare que invertemos a lógica para podermos lançar a exceção antes.

3) Veja que agora nosso método transfere() também precisa ser alterado, já que agora ele não terá mais um retorno do tipo boolean e que o método saca() não retorna void, além da exceção na assinatura do método.

Altere o método para ficar como:

```java
public void transfere(double valor, Conta destino) throws SaldoInsuficienteException{
    this.saca(valor);
    destino.deposita(valor);
}
```

4) Por conta disso, precisaremos alterar nosso método saca() de nossa classe ContaCorrente:

```java
@Override
public void saca(double valor) throws SaldoInsuficienteException {
    double valorASacar = valor + 0.2;
    super.saca(valorASacar);
}
```

5) Agora, altere a classe TesteContas para funcionar com nossas exceções. Para tal, adicione um "throws" na assinatura do método main:

```java
public class TesteContas {

    public static void main(String[] args) throws SaldoInsuficienteException{

        ContaCorrente cc = new ContaCorrente(111, 111);
        cc.deposita(100.0);

        ContaPoupanca cp = new ContaPoupanca(222, 222);
        cp.deposita(200.0);

        cc.transfere(110.0, cp);

        System.out.println("CC: " + cc.getSaldo());
        System.out.println("CP: " + cp.getSaldo());
    }
}
```

Depois, tente transferir um valor inválido e execute o código:

6) Por fim, crie uma classe TesteSaca para testar o método saca. Use um try-catch para capturar a exceção:

```java
public class TesteSaca {

    public static void main(String[] args) {
        Conta conta = new ContaCorrente(123, 321);

        conta.deposita(200.0);

        try {
            conta.saca(210.0);
        } catch(SaldoInsuficienteException ex) {
            System.out.println("Exception: " + ex.getMessage());
            ex.printStackTrace();
        }

        System.out.println(conta.getSaldo());
    }
}
```

Execute a classe TesteSaca!


## Nomenclatura

No vídeo, usamos uma exceção com o nome SaldoInsuficienteException. Discutir nomes pode ser algo subjetivo e exige conhecimentos sobre o assunto. Ou seja, é pauta de longas discussões, mas acreditamos que um nome um pouco mais genérico para nossa exceção também seria uma solução adequada.

Por exemplo, a exceção poderia se chamar SacaException ou ContaException. Repare que usamos o nome do método ou da classe. Para detalhar mais o problema (valor do saldo, etc) podemos utilizar a mensagem da exceção, como já fizemos no curso:

```java
throw new SacaException("Valor invalido: Saldo: " + this.saldo + ", Valor: " + valor);
```

Dessa forma, caso tenha outro problema, basta alterar a mensagem.

De qualquer forma, saiba que encontrar o nome perfeito para as suas classes e métodos não é uma tarefa fácil e pode tomar o seu tempo. Em alguns casos, já encontramos nomes nas classes que deixaram claro que isso é apenas algo provisório e que deve ser alterado quando houver um consenso no nome.


# O que aprendemos?

* como criar um bloco catch genérico usando a classe Exception;

* como criar uma exceção nova SaldoInsuficienteException;

* como transformar a exceção em checked ou unchecked.

[Próxima Aula](https://github.com/pvreboucas/java-excecoes/tree/aula-6)
