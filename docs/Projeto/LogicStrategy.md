# Strategy - Portas Lógicas

## Introdução

<div align="justify">
Este documento apresenta o padrão de projeto Strategy das portas lógicas, um padrão de projeto que permitem definir uma família de algoritmos que podem ser trocados entre sí¹. permitindo maior flexibilidade em escolha de algoritmos que resolvem o mesmo problema.
</div>

## Objetivo

<div align="justify">
O objetivo do strategy é permitir que um conjunto de algoritmos seja definido e encapsulado separadamente de forma que a escolha de qual será usado possa ser feita em tempo de execução. Resumidamente, o strategy promove a separação entre o comportamento de uma classe e a lógica que a implementa.
</div>

## Metodologia

<div align="justify">
No processo de desenvolvimento das portas lógicas foi notada a necessidade de uma interface simples para a função lógica utilizada por uma porta lógica, devido a sua simplicidade e genericidade entre as diferentes portas lógicas. Assim, foi criada um classe LogicStrategy, utilizada pela classe abstrata de portas lógicas para evaluar a função lógica. Implementada por cada tipo de função lógica
</div>

## Resultados

### Interface: LogicStrategy

```kotlin
interface LogicStrategy {
    fun getOutput(inputPower: InputPower): Boolean
}
```

### Classe: ANDStrategy

```kotlin
class ANDStrategy : LogicStrategy {
    override fun getOutput(inputPower: InputPower) = inputPower.east && inputPower.west
}
```

### Classe: NANDStrategy

```kotlin
class NANDStrategy : LogicStrategy {
    override fun getOutput(inputPower: InputPower) = !(inputPower.east && inputPower.west)
}
```

### Classe: NORStrategy

```kotlin
class NORStrategy : LogicStrategy {
    override fun getOutput(inputPower: InputPower) = !(inputPower.east || inputPower.west)
}
```

### Classe: NOTStrategy

```kotlin
class NOTStrategy : LogicStrategy {
    override fun getOutput(inputPower: InputPower) = !inputPower.south
}
```

### Classe: ORStrategy

```kotlin
class ORStrategy : LogicStrategy {
    override fun getOutput(inputPower: InputPower) = inputPower.east || inputPower.west
}
```

### Classe: XNORStrategy

```kotlin
class XNORStrategy : LogicStrategy {
    override fun getOutput(inputPower: InputPower) = !(inputPower.west xor inputPower.east)
}
```

### Classe: XORStrategy

```kotlin
class XORStrategy : LogicStrategy {
    override fun getOutput(inputPower: InputPower) = inputPower.east xor inputPower.west
}
```

### Classe: AbstractLogicGate

```kotlin
/**
 * Abstract base class for logic digital gates that extends the behaviour of redstone gates in Minecraft.
 * this class takes a logic function that determines whether the gate is powered based on the input power from
 * neighboring blocks.
 *
 * @param settings The settings from the block, passed to the superclass 'AbstractRedstoneGateBlock'.
 * @param logicStrategy a strategy for the respective logic gate
 * indicating whether the gate should be powered
 */
abstract class AbstractLogicGate(settings: Settings, val logicStrategy: LogicStrategy) :
    AbstractRedstoneGateBlock(settings) {
    init {
        defaultState = stateManager.defaultState
            .with(FACING, Direction.NORTH)
            .with(POWERED, false)
    }

    override fun getOutlineShape(
        state: BlockState?,
        world: BlockView?,
        pos: BlockPos?,
        context: ShapeContext?
    ) = BLOCK_SHAPE

    /**
     * Delay before the gate's state is updated. hardcoded to two ticks.
     *
     * @param state the current block state
     * @return The delay time in ticks
     */
    override fun getUpdateDelayInternal(state: BlockState?) = 2

    /**
     * Checks if the gate is powered based on the input from its neighbors. The logic function
     * passed to the gate is used to determine if the gate should be powered or not.
     *
     * @param world The current world instance.
     * @param pos The position of the block in the world.
     * @param state The current block state.
     * @return Boolean indicating whether the gate is powered.
     */
    override fun hasPower(world: World, pos: BlockPos, state: BlockState) = logicStrategy.getOutput(getInputPower(world, pos, state))


    override fun appendProperties(builder: StateManager.Builder<Block, BlockState>) {
        builder.add(FACING, POWERED)
    }


    /**
     * Retrieves the input power from neighboring blocks. This function checks the redstone power from the blocks
     * to the east, west, and south relative to the gate.
     *
     * @param world The world instance in which the gate exists.
     * @param pos The position of the gate block.
     * @param state The current state of the block.
     * @return An `InputPower` object containing the power status from the east, west, and south neighbors.
     */
    fun getInputPower(world: World, pos: BlockPos, state: BlockState) : InputPower{
        val facing = state[FACING]
         return InputPower(
             hasPowerFromNeighbor(world, pos, facing.rotateYClockwise()),
             hasPowerFromNeighbor(world, pos, facing.rotateYCounterclockwise()),
             hasPowerFromNeighbor(world, pos, facing),
         )
    }

    /**
     * Checks if there is power coming from a neighboring block in a specific direction. This function checks the
     * emitted redstone power from a neighboring block.
     *
     * @param world The world instance.
     * @param pos The position of the current block.
     * @param direction The direction to check for power (east, west, south, etc.).
     * @return Boolean indicating if the neighbor block is emitting redstone power.
     */
    private fun hasPowerFromNeighbor(world: World, pos: BlockPos, direction: Direction): Boolean {
        val neighborPos = pos.offset(direction)
        val power = world.getEmittedRedstonePower(neighborPos, direction)
        return power > 0
    }
}
```

### Modelagem

<center>
Figura 1 - Strategy

![Diagrama de Classes - Estratégia de Lógica](https://raw.githubusercontent.com/UnBArqDsw2024-2/2024.2_G1_Logic_Thinkering_Entrega_03/refs/heads/main/assets/strategy_logica.png)

<b>Fonte:</b> Silva, André, 2025.
A implementação do padrão Strategy permitiu uma interface flexível para a criação de funções lógicas, permitindo a criação de várias portas lógicas intercambiáveis

</center>

## Conclusão

<!--
-   **Resuma os pontos principais do trabalho.**
-   **Avalie se os objetivos foram alcançados e o impacto do trabalho.**
-   **Apresente perspectivas para melhorias ou trabalhos futuros.**
-->

<div align="justify">&emsp;&emsp;
A aplicação do padrão Strategy na implementação da lógica booleana mineração provou ser uma abordagem eficaz para lidar com os diferentes tipos de funções booleanas de portas lógicas que podem ser adicionadas futuramente. Com a separação da lógica em uma interface distinta, foi possível garantir flexibilidade na escolha e alteração das expressões lógicas.
</div>

## Bibliografia

<!-- - **Altere!**-->

> [1] REFACTORING GURU. Builder. Disponível em: <https://refactoring.guru/design-patterns/builder>.

<center>

## Participantes

</center>

<!-- de preferência: em ordem alfabética, seguindo o exemplo: -->

<div style="margin: 0 auto; width: fit-content;">

| Matrícula | Aluno                        | Git                                       |
| --------- | ---------------------------- | ----------------------------------------- |
| 221007813 | André Emanuel Bispo da Silva | [Hunter104](https://github.com/Hunter104) |
| 211062526 | Thomas Queiroz Souza Alves   | [Thomas Q](https://github.com/Hunter104)  |

</div>

---

<center>

## Histórico de Versão

</center>

<!-- Lembre de alterar a data -->
<!-- É PRA POR O NOME, NÃO O USER DO GITHUB -->

<div style="margin: 0 auto; width: fit-content;">

| Versão | Data da alteração |      Alteração       |                         Responsável                          | Revisor | Data de revisão |
| :----: | :---------------: | :------------------: | :----------------------------------------------------------: | :-----: | :-------------: |
|  1.0   |       05/01       | Criação do documento | [André Emanuel Bispo da Silva](https://github.com/Hunter104) |         |                 |

</div>

## Controle de Revisão

| Revisor(es) | O que foi realizado |
| :---------: | :-----------------: |
|             |                     |
