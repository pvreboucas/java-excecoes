*[<< AULA ANTERIOR](https://github.com/pvreboucas/java-excecoes/edit/aula-4/aulas/2-entendendo-erros.md)*

Anteriormente, falamos sobre a hierarquia dos erros.

![01]()

```
    Para acessibilidade: A hierarquia abordada contém uma classe mãe chamada Throwable. A partir dela, formam-se duas categorias: uma para exceções onde o desenvolvedor pode gerenciar e uma outra categoria voltada para erros da máquina virtual.

    As exceções ArithmeticException, NullPointerException e MinhaException herdam de RuntimeException, que por sua vez, herda de Exception e Exception herda de Throwable, formando a primeira categoria de exceções. Já a segunda categoria de erros possui StackOverflowError, que herda de VirtualMachineError, que herda de Error, que herda de Throwable, formando a segunda categoria.
```

Lembrando que a segunda categoria é a hierarquia utilizada pela máquina virtual. Entretanto, o que nos interessa é a hierarquia da Exception, a primeira categoria. E por que a classe ArithmeticException não estende diretamente da classe Exception? Por que a MinhaExcecao não estende a classe Exception? Vamos tentar resolver esse enigma.

Acessaremos a classe MinhaExcecao, e estenderemos diretamente da classe Exception. Repare que o código já para de compilar. Apareceu algum problema. Vamos ver a classe Fluxo.

Existe um erro de compilação na linha do throw new MinhaExcecao("deu muito errado"). Se retirarmos a palavra throw dessa frase, o problema desaparecerá. O problema é que o Java faz uma separação. Duas categorias de exceções são criadas dentro das exceções para o desenvolvedor de aplicações.

A primeira categoria é a que estende de RuntimeException, e a outra é a que estende diretamente de Exception. O compilador faz uma verificação sintática para ver quem dá throw nessas exceções. Isso significa que a exceção exige que fique explícito na assinatura do método que estamos jogando a exceção:

```java

```
