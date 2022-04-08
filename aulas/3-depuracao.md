Anteriormente, estudamos sobre a pilha de execução, que é criada automaticamente. Em Java, a pilha padrão sempre começará com o método main() e pode crescer ou diminuir, dependendo de quais métodos serão chamados. A seguir, exploraremos como ela funciona, dentro do Eclipse.

Clicaremos duas vezes antes do número da linha que imprime "Ini do main":

![01](https://github.com/pvreboucas/java-excecoes/blob/aula-1/aulas/imagens/01.03_001_ponto-depuracao.png)

Perceba que, à esquerda da coluna de identificação do número de linhas, apareceu o chamado Ponto de Depuração ou também conhecido como Breakpoint. Ele pausa a execução em um ponto específico, caso nós executemos o projeto em modo Debug. Ao debugarmos o programa, será possível ver as mudanças em tempo real, em uma velocidade que podemos acompanhar. Para isso, clicaremos com o botão direito em "main() > Debug As > Java Application".

Nesse momento, o eclipse irá sugerir a mudança de perspectiva, alterando a organização das janelas, pois existe uma perspectiva específica para a depuração. Iremos usá-la, clicando em "Sim". Encontraremos a pilha dentro da classe Fluxo:

![02](https://github.com/pvreboucas/java-excecoes/blob/aula-1/aulas/imagens/01.03_002_debug-mudanca-perspectiva.png)

O main(), que está na linha quatro, é o primeiro método que está sendo executado. Em função do breakpoint, que colocamos nessa mesma linha, o programa está parado nela. O Java está aguardando as nossas instruções. Para orientá-lo a avançar uma linha, utilizaremos os itens de navegação encontrados na perspectiva de debug:

![03](https://github.com/pvreboucas/java-excecoes/blob/aula-1/aulas/imagens/01.03_003_itens-navegacao-debug.png)

Clicaremos no botão correspondente ao atalho "F6", o Step Over. Observe o que acontecerá:

![04](https://github.com/pvreboucas/java-excecoes/blob/aula-1/aulas/imagens/01.03_004_antes-depois-step-over.png)

O Java executou a linha que imprimia "Ini do main" e, depois, parou na seguinte, na chamada do metodo1(). E então, foi exibido no console a mensagem "Ini no main".

Entretanto, não queremos que o Java passe por cima desse método e pare na linha que imprime "Fim do main". No momento, queremos que ele acesse o método através dessa chamada. Caso você faça o teste de apertar o "F6" ou Step Over novamente, verá que o console ficará assim:

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
```

Todo o processo foi executado, até que chegasse a linha que imprimisse "Fim do main". Em função disso, não conseguimos ver o passo a passo. Essa foi a nossa primeira experiência com o debug. Vamos refazer esse processo, sem pular fases.

Após a finalização do processo, clicaremos no ícone de Bug, que contém o desenho de Besouro, e em "Debug Fluxo", para que o processo seja refeito. A execução irá parar novamente no método main(), na linha quatro.

Selecionaremos a opção "F6" ou Step Over. Como você pode reparar, a execução está parada na chamada do metodo1() e, em vez de passar pelo método novamente, entraremos nele. Para isso, utilizaremos o atalho "F5" ou Step Into.

O comando F5 entra no método

![05](https://github.com/pvreboucas/java-excecoes/blob/aula-1/aulas/imagens/01.03_005_step-into-metodo1.png)

A partir desse ponto, vamos clicar mais uma vez em "F6". Com isso, a nossa aplicação para na chamada do metodo2(). Para entrar nesse método, utilizaremos novamente o atalho "F5".

![06](https://github.com/pvreboucas/java-excecoes/blob/aula-1/aulas/imagens/01.03_006_step-into-metodo2.png)

A partir daqui, utilizaremos somente o Step Over. À direita, podemos ver o valor da variável i:

![07](https://github.com/pvreboucas/java-excecoes/blob/aula-1/aulas/imagens/01.03_007_variaveis-debug.png)

Feito o primeiro laço, a variável i presente ainda vale 1. Clicaremos em Step Over, novamente, para chegar no final do laço. Veja que o i foi incrementado para 2 e, no console, será impresso o valor 2. Vamos dar uma olhada em como está a pilha:

![08](https://github.com/pvreboucas/java-excecoes/blob/aula-1/aulas/imagens/01.03_008_pilha-ao-vivo.png)

Em cima de main(), o metodo1() está parado, e acima dele, está o metodo2(), no topo da fila. Clicaremos no Step Over ou teclaremos "F6" para continuar a execução.

![09](https://github.com/pvreboucas/java-excecoes/blob/aula-1/aulas/imagens/01.03_009_pilha-ao-vivo-sem-metodo2.png)

Note que metodo2() desapareceu da pilha de execução, pois foi finalizado, movendo metodo1() para o topo da lista. Executaremos o "F6", e a mensagem "Fim do metodo1" aparecerá no console. Ao clicar novamente em "F6", o metodo1() sairá da pilha e voltaremos ao método main().

Executaremos, pela última vez, o Step Over para que main() saia da pilha. Finalizaremos o processo, clicando no botão "Resume" ou utilizando o atalho "F8".

Por meio da depuração, é possível ver o Java criando essa pilha passo a passo, trabalhando em uma velocidade mais humana, linha por linha. Assim, acompanhamos cada valor. Esse processo é muito útil quando nos deparamos com um problema e não sabemos o porquê.

Bom, o Eclipse nos mostrou uma nova perspectiva para o Debugging. Para voltar à anterior, repare que, no canto superior direito do Eclipse, temos opções de perspectivas disponíveis.

![10](https://github.com/pvreboucas/java-excecoes/blob/aula-1/aulas/imagens/01.03_010_perspectivas-eclipse.png)

Perspectivas do Eclipse

A atual perspectiva é a de Debug, que contém um besouro no ícone. Estamos em busca da que contém um "J". Ao clicar nela, todas as janelas mudam de lugar e temos uma visão melhor do código Java.

O principal objetivo foi mostrar a pilha ao vivo. Mas, aproveitamos para aprender a depurar uma aplicação.

Adiante, analisaremos as exceções em cima desse simples código que estamos trabalhando.

