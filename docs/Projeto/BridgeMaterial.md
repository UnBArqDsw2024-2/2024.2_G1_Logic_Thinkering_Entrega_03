# Bridge - Material

<div align="justify">&emsp;&emsp; Este documento apresenta o padrão de projeto bridge utilizado para resolver o problema da separação entre abstração e implementação entre os itens e seus materiais no mod "Logic Thinkering".
</div>

## Objetivo

<div align="justify">&emsp;&emsp;
O objetivo principal do uso do padrão Bridge é desacoplar as hierarquias de classes de abstração e implementação, permitindo que ambas possam ser desenvolvidas e estendidas de forma independente. No contexto deste projeto, esse padrão foi aplicado para gerenciar diferentes os diferentes tipos de materiais e itens do mod, onde as abstrações representam os conceitos gerais de materiais e itens (como armaduras e ferramentas), e suas implementações concretas tratam de suas variações singulares (como materiais reforçados de cobre, esmeralda e ametista).
</div>

## Metodologia
<div align="justify">&emsp;&emsp;
Durante a fase de implementação, uma das dificuldades encontradas foi gerenciar as inúmeras combinações possíveis entre os itens, que são compostos por um tipo e um material. Normalmente, seria necessário gerar o produto cartesiano dessas combinações, ou seja, instanciar todos os tipos de itens para cada possibilidade de material. Esse processo, além de ineficiente, resultaria em um grande número de classes ou instâncias redundantes uma vez que, por exemplo, se um novo item (que requer um material) fosse adicionado futuramente, o número de instâncias ou subclasses necessários para cada variação do material aumentaria demasiada e desnecessáriamente a comlexidade do sistema.
</div>
<div align="justify">&emsp;&emsp; 
Para resolver esse problema, foi utilizamos o padrão de projeto bridge, que permite a separação da abstração (os tipos de itens) da implementação (os materiais). Essa abordagem possibilitou a criação de uma estrutura modular, onde os itens são definidos de forma genérica e podem ser associados dinamicamente a diferentes materiais, evitando a necessidade de instanciar todas as combinações possíveis. 
</div> 

## Resultados

### Classe Abstrata: PrototypeItem

```java
//Inserir
```

### Classe: ConcreteArmor

```java
//Inserir
```

### Classe: ConcreteItem

```java
//Inserir
```

### Classe: ConcreteTool

```java
//Inserir
```

### Interface: Material

```java
//Inserir
```

### Classe: ModArmorMaterial

```java
//Inserir
```

### Classe: ModToolMaterial

```java
//Inserir
```

<center>
Figura 1 - Bridge

![v2](https://raw.githubusercontent.com/UnBArqDsw2024-2/2024.2_G1_Logic_Thinkering_Entrega_03/refs/heads/main/assets/strategyregisterv2.png)

 <b>Fonte:</b> Carvalho, Sandes, 2025.
</center>

<div align="justify">&emsp;&emsp;
TEXTO
TEXTO
TEXTO
</div>

## Conclusão

<!--
-   **Resuma os pontos principais do trabalho.**
-   **Avalie se os objetivos foram alcançados e o impacto do trabalho.**
-   **Apresente perspectivas para melhorias ou trabalhos futuros.**
-->

<div align="justify">&emsp;&emsp;
Com a implementação do padrão Bridge, foi possível gerenciar a relação entre tipos de itens e materiais de forma flexível e organizada, desacoplando a abstração da implementação. Essa abordagem permite que novos tipos de itens ou materiais podem ser adicionados sem a necessidade de modificar o código existente, promovendo a separação de responsabilidades e facilitando a manutenção. Além disso, o uso do Bridge eliminou a necessidade de gerar todas as combinações possíveis entre itens e materiais, reduzindo a complexidade do código e otimizando o desempenho. Essa solução também oferece maior facilidade para expandir o mod, inserindo novos itens e materiais de forma que estes possam ser integrados de forma simples e organizada.
</div>

### Versões Anteriores

<details>
<summary>Visualizar versão 1.0</summary>

### Versão 1.0

<!-- Aqui documente as mudanças de uma versão para a outra -->

A Figura 1 apresenta a versão 1.0 do UML de registro de itens.

<center>
Figura 1 - Bridge

![v1](https://raw.githubusercontent.com/UnBArqDsw2024-2/2024.2_G1_Logic_Thinkering_Entrega_03/refs/heads/main/assets/strategyregister.jpeg)

<b>Fonte:</b> Carvalho, Sandes, 2025.
</center>

</details>


## Bibliografia

<!-- - **Altere!**-->

> [1] SERRANO, Milene. Videoaula: [10b - Vídeo-Aula - DSW - GoFs - Comportamentais - Strategy](https://unbbr-my.sharepoint.com/:v:/g/personal/mileneserrano_unb_br/EUZop92j5WlIvdppY_A5wGcBt5xkL_HFpF8USoR6BJh4lw?e=JRjZ7B). [Online]. Disponível em: [https://unbbr-my.sharepoint.com/:v:/g/personal/mileneserrano_unb_br/EUZop92j5WlIvdppY_A5wGcBt5xkL_HFpF8USoR6BJh4lw?e=JRjZ7B](https://unbbr-my.sharepoint.com/:v:/g/personal/mileneserrano_unb_br/EUZop92j5WlIvdppY_A5wGcBt5xkL_HFpF8USoR6BJh4lw?e=JRjZ7B) . Acesso em: 02 jan. 2025.

<center>

## Participantes

</center>

<!-- de preferência: em ordem alfabética, seguindo o exemplo: -->

<div style="margin: 0 auto; width: fit-content;">

| Matrícula | Aluno                             | Git                                               |
| --------- | --------------------------------- | ------------------------------------------------- |
| 221008024 | Eduardo Matheus dos Santos Sandes | [DiceRunner714](https://github.com/DiceRunner714) |
| 170010872 | Gabriela de Oliveira Lemos        | [heylisten64](https://github.com/heylisten64)     |
| 221008150 | João Antonio Ginuino Carvalho     | [i-JSS](https://github.com/i-JSS)       |

</div>

---

<center>

## Histórico de Versão

</center>

<!-- Lembre de alterar a data -->
<!-- É PRA POR O NOME, NÃO O USER DO GITHUB -->

<div style="margin: 0 auto; width: fit-content;">

| Versão | Data da alteração |      Alteração       |                                                                           Responsável                                                                           | Revisor | Data de revisão |
|:------:|:-----------------:| :------------------: | :-------------------------------------------------------------------------------------------------------------------------------------------------------------: | :-----: | :-------------: |
|  1.0   |       04/01       | Criação do documento | [Eduardo Sandes](https://github.com/DiceRunner714), [Gabriela Lemos](https://github.com/heylisten64), [João Antonio G. Carvalho](https://github.com/i-JSS) |         |                 |

</div>

## Controle de Revisão

|                        Revisor(es)                        | O que foi realizado |
| :-------------------------------------------------------: |:-------------------:|
| |                     |