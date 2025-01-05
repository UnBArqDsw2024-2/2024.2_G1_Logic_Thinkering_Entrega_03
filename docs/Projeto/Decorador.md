# Decorador

## Introdução

<div align="justify">
Este documento apresenta o padrão de projeto Decorador das portas lógicas, um padrão de projeto que permite redefinir o comportamento de um objeto em tempo de execução através de classes "embalagem"¹.
</div>

## Objetivo

<div align="justify">
O objetivo principal do uso do padrão Decorador é adicionar novos comportamentos para objetos de forma flexível em tempo de execução, evitando problemas como herança múltipla e alteração de comportamento em tempo de execução
</div>

## Metodologia
<div align="justify"> 
Foi criada uma classe decoradora que implementa funcionalidade de registro para o seu objeto "embalado", a fim de facilitar a depuração das portas lógicas
</div>

## Resultados

### Classe: LogicLoggerDecorator

```kotlin
class LogicLoggerDecorator(gate: AbstractLogicGate) : AbstractLogicGate(gate.settings, gate.logicStrategy) {
    override fun getCodec(): MapCodec<AbstractRedstoneGateBlock>? = null


    override fun onUse(
        state: BlockState?,
        world: World?,
        pos: BlockPos?,
        player: PlayerEntity?,
        hit: BlockHitResult?
    ): ActionResult {
        val powered = state!![POWERED]
        val input = getInputPower(world!!, pos!!, state)
        logger.info("Logic gate state at {}. Powered {}, East {}, West {}, South {}",
            pos, powered, input.east, input.west, input.south)
        return super.onUse(state, world, pos, player, hit)
    }

    override fun onPlaced(
        world: World?,
        pos: BlockPos?,
        state: BlockState?,
        placer: LivingEntity?,
        itemStack: ItemStack?
    ) {
        logger.info("Placing logic gate at {}", pos!!)
        super.onPlaced(world, pos, state, placer, itemStack)
    }
}

```

### Adição de decorador em inicialização

```kotlin

fun initialize() {
    logger.info("Initializing Logic Thinkering mod, Kotlin side!")

    buildRegistryHelper {
        group(LogicThinkeringItemGroup.LOGICTHINKERING_GROUP)
        settings(Settings.copy(Blocks.REPEATER))
        registerItems(true)
        decorator {
            if (it is AbstractLogicGate)
                LogicLoggerDecorator(it)
            else
                it
        }
        ::ORGate with "or_gate"
        ::ANDGate with "and_gate"
        ::XORGate with "xor_gate"
        ::NOTGate with "not_gate"
        ::NORGate with "nor_gate"
        ::NANDGate with "nand_gate"
        ::XNORGate with "xnor_gate"
    }.register()

    registerItems {
        group(ItemGroups.COMBAT)
        ReinforcedCopperShield() with "reinforced_copper_shield"
        ReinforcedCopperSword() with "reinforced_sword"
    }.register()

    ModComponents.initialize()
}
```

### Modelagem

<center>
Figura 1 - Decorador

![Modelagem - Decorador](https://raw.githubusercontent.com/UnBArqDsw2024-2/2024.2_G1_Logic_Thinkering_Entrega_03/refs/heads/main/assets/decorador.png)

<b>Fonte:</b> Silva, André, 2025.
A implementação do padrão Decorador permitiu a alteração facilitada das portas lógicas em tempo de execução, permitindo a adição de funcionalidade de registro que pode ser facilamente trocada ao registrar as classes.

</center>

## Conclusão

<!--
-   **Resuma os pontos principais do trabalho.**
-   **Avalie se os objetivos foram alcançados e o impacto do trabalho.**
-   **Apresente perspectivas para melhorias ou trabalhos futuros.**
-->

<div align="justify">&emsp;&emsp;
Em suma foi utilizada uma classe "embalagem" que altera a funcionalidade das portas lógicas, adicionando funcionalidade de regsitro de estado atual de cada porta ao interagir com a tal. Alcançando os objetivos de alteração sem herança
</div>


## Bibliografia

<!-- - **Altere!**-->

> [1] REFACTORING GURU. Builder. Disponível em: <https://refactoring.guru/design-patterns/builder>.

‌
<center>

## Participantes

</center>

<!-- de preferência: em ordem alfabética, seguindo o exemplo: -->

<div style="margin: 0 auto; width: fit-content;">

| Matrícula | Aluno                        | Git                                       |
|-----------|------------------------------|-------------------------------------------|
| 221007813 | André Emanuel Bispo da Silva | [Hunter104](https://github.com/Hunter104) |

</div>

---

<center>

## Histórico de Versão

</center>

<!-- Lembre de alterar a data -->
<!-- É PRA POR O NOME, NÃO O USER DO GITHUB -->

<div style="margin: 0 auto; width: fit-content;">

|  Versão   |      Data da alteração       |      Alteração       |                         Responsável                          | Revisor | Data de revisão |
|:---------:|:----------------------------:|:--------------------:|:------------------------------------------------------------:|:-------:|:---------------:|
|    1.0    |            05/01             | Criação do documento | [André Emanuel Bispo da Silva](https://github.com/Hunter104) |         |

</div>

## Controle de Revisão

| Revisor(es) | O que foi realizado |
|:-----------:|:-------------------:|
|             |                     |
