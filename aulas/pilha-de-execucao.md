Continuemos a nossa viagem pela plataforma e linguagem Java. Temos a versão 9 instalada, mas caso você tenha instalado uma das anteriores (6, 7 ou 8) em sua máquina, não se preocupe, pois o que veremos não é específico em relação às versões.

Também temos o Eclipse na versão Oxygen.2 instalado. Mas, caso você tenha uma versão mais antiga, não há problema. Usaremos o mesmo workspace do curso anterior, o eclipse-workspace.

Após abrir o Eclipse, criaremos um novo projeto Java para começar. Clicaremos com o botão direito na área do "Package Explorer > New > Java Project". O novo projeto será chamado de java-pilha. É um nome peculiar, pois o foco do primeiro tópico é o tratamento de exceções em geral, sendo esse um dos tópicos iniciais mais complicados do mundo Java. Clicaremos em Finish, para criar o projeto.

Antes de entrar nesses detalhes sobre erros e exceções, entenderemos como funciona o Java, internamente.

Adicionaremos uma nova classe à pasta src, clicando com o botão direito em "src > New > Class". Vamos chamá-la de Fluxo. Para ganharmos tempo, disponibilizaremos o conteúdo dela. Assim, você poderá copiar e colar na sua classe:

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

Temos então, o nosso querido método main(). Dentro dele, como podemos ver, existe uma chamada para o metodo1(), que é static, assim como a main(). Para chamar um método que não tem uma referência em mãos ou um objeto criado, ele precisa ser static.

No console, para as seguintes mensagens que serão exibidas:

* "Ini do main", acessaremos o metodo1() através da chamada;
* "Ini do metodo1", acessaremos o metodo2();
* "Ini do metodo2", na sequência, será executado um laço no qual a variável i começa em 1 e vai até 5. Essa mensagem será exibida até o final do laço.

Então, para cada finalização de execução de método, será impressa uma mensagem de conclusão, na seguinte ordem:

* Fim do metodo2;
* Fim do metodo1;
* Fim do main, para finalizar.

Se executarmos esse código, obteremos o seguinte resultado:

```java
Ini do main
Ini do metodo1
Ini do metodo2
1
2
3
4
5
Fim do metodo2
Fim do metodo1
Fim do main
```

Esse é o resultado do fluxo. Repare que ainda não temos orientação a objetos, mas independente do paradigma de programação, usamos uma pilha de execução. Para entendermos melhor, trabalharemos a seguinte imagem:

![01](https://github.com/pvreboucas/java-excecoes/blob/aula-1/aulas/imagens/01.01_001_console-main-pilha.png)

Como sabemos, um programa em Java sempre começa no método main(). A pilha ou stack existe para organizar a execução do código, lembrando o que ainda precisa ser executado.

O Java sempre irá procurar primeiro pelo método main(). Depois, ele cria a pilha e coloca o bloco de código dentro de outro, na pilha. Em seguida, o código do metodo1() é inserido nela.

```
A pilha existe para executar algo e lembrar o que ainda precisa ser executado.
```

![02](https://github.com/pvreboucas/java-excecoes/blob/aula-1/aulas/imagens/01.01_002_console-metodo1-pilha.png)

Agora, é a vez do metodo1() entrar na pilha de execução. Repare que main() ainda não foi finalizado, porém ele não está sendo executado agora. Apenas o método que está no topo da pilha está sendo executado.

A partir do metodo1(), vem a chamada do metodo2, e todo o bloco de código é jogado dentro da pilha.

![03](https://github.com/pvreboucas/java-excecoes/blob/aula-1/aulas/imagens/01.01_003_console-metodo2-pilha.png)-

Agora, os métodos main() e metodo1() estão parados, pois ainda tem código a ser executado, no caso, as linhas do "Fim do metodo1" e "Fim do main". O metodo2() está sendo executado porque está no topo da pilha, que conseguirá removê-lo assim que terminar de executá-lo, quando não houver mais nada para executar.

![04](https://github.com/pvreboucas/java-excecoes/blob/aula-1/aulas/imagens/01.01_004_console-remover-metodo2-pilha.png)

Quem está sendo executado agora é o metodo1(). O que sobrou nele é a linha que exibe a mensagem "Fim do metodo1". Depois que ele acaba, a pilha o remove e continua com a main(), que ficou por último.

![05](https://github.com/pvreboucas/java-excecoes/blob/aula-1/aulas/imagens/01.01_005_console-remover-metodo1-pilha.png)

Voltando a main(), é exibida a linha que imprime "Fim do main". Com isso, ele é finalizado e retirado da pilha.

![06](https://github.com/pvreboucas/java-excecoes/blob/aula-1/aulas/imagens/01.01_006_console-remover-main-pilha.png)

Quando acabam os métodos da pilha, o Java entende que o processo foi encerrado.

Recapitulando: A pilha existe para saber qual método está sendo executado — o que está no topo — e lembrar quais ainda precisam ser executados. Entendido como ela funciona, podemos falar sobre os problemas de execução, as exceções.
