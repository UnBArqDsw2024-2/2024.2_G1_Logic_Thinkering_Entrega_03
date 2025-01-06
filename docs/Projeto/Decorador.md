# Decorador do logger

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
| --------- | ---------------------------- | ----------------------------------------- |
| 221007813 | André Emanuel Bispo da Silva | [Hunter104](https://github.com/Hunter104) |

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

# Decorador das armas

## Introdução

<div align="justify">
Este documento apresenta o padrão de projeto Decorador para as armas, um padrão de projeto que permite redefinir o comportamento de um objeto em tempo de execução através de classes "embalagem"¹.
</div>

## Objetivo

<div align="justify">
O objetivo principal do uso do padrão Decorador é adicionar novos comportamentos para objetos de forma flexível em tempo de execução, evitando problemas como herança múltipla e alteração de comportamento em tempo de execução
</div>

## Metodologia

<div align="justify">
Foi criada uma classe decoradora que implementa funcionalidade de registro para o seu objeto "embalado", a fim de reduzir as redundâncias requeridas pela API de modificação do jogo. Foi necessário mudar forma como o jogo calcula dano levado pelos inimigos, quanto de durabilidade será deduzido do item, modificações do estado no inventário, emissão de partículas, etc. Como isso precisa ser feito em cada item, pareceu interessante -principalmente do ponto de vista da manutenibilidade- implementar como um decorador.
</div>

## Resultados


### Interface: WeaponBehavior
```kotlin
interface WeaponBehavior {
    fun onHit(stack: ItemStack, target: LivingEntity, attacker: LivingEntity): Boolean
    fun appendWeaponTooltip(stack: ItemStack, world: TooltipContext?, tooltip: MutableList<Text>, context: TooltipType)
}

abstract class WeaponDecorator(private val weapon: WeaponBehavior) : WeaponBehavior {
    override fun onHit(stack: ItemStack, target: LivingEntity, attacker: LivingEntity): Boolean {
        return weapon.onHit(stack, target, attacker)
    }

    override fun appendWeaponTooltip(stack: ItemStack, world: Item.TooltipContext?, tooltip: MutableList<Text>, context: TooltipType) {
        weapon.appendWeaponTooltip(stack, world, tooltip, context)
    }
}
```

### Classe: ChargeableWeaponDecorator
```kotlin
class ChargeableWeaponDecorator(
    weapon: WeaponBehavior,
    private val maxCharge: Int = 3,
    private val baseAttackDamage: Float // Add this parameter
) : WeaponDecorator(weapon) {
    companion object {
        private const val DEFAULT_CHARGE = 0
        private const val SOUND_VOLUME = 1.0f
        private const val SOUND_PITCH = 1.0f
    }

    override fun onHit(stack: ItemStack, target: LivingEntity, attacker: LivingEntity): Boolean {
        if (attacker !is PlayerEntity) {
            return super.onHit(stack, target, attacker)
        }

        processChargedAttack(stack, target, attacker)
        return super.onHit(stack, target, attacker)
    }

    private fun processChargedAttack(stack: ItemStack, target: LivingEntity, attacker: PlayerEntity) {
        val currentCharge = getCurrentCharge(stack)
        if (currentCharge <= 0) return

        dealChargedDamage(target, attacker, currentCharge)
        resetCharge(stack)
        playAttackSound(target, attacker)
    }

    private fun getCurrentCharge(stack: ItemStack): Int =
        stack.get(CHARGE_COMPONENT) ?: DEFAULT_CHARGE

    private fun dealChargedDamage(target: LivingEntity, attacker: PlayerEntity, chargeLevel: Int) {
        // Use the baseAttackDamage parameter instead of trying to cast
        val damageDealt = baseAttackDamage * (kotlin.math.max(chargeLevel, maxCharge) + 1)
        val world = target.world
        target.damage(world as ServerWorld,target.damageSources.playerAttack(attacker), damageDealt)
    }

    private fun resetCharge(stack: ItemStack) {
        stack.set(CHARGE_COMPONENT, DEFAULT_CHARGE)
    }

    private fun playAttackSound(target: LivingEntity, attacker: PlayerEntity) {
        attacker.world.playSound(
            null,
            target.x,
            target.y,
            target.z,
            SoundEvents.BLOCK_COPPER_BREAK,
            SoundCategory.PLAYERS,
            SOUND_VOLUME,
            SOUND_PITCH
        )
    }

    override fun appendWeaponTooltip(stack: ItemStack, world: TooltipContext?, tooltip: MutableList<Text>, context: TooltipType) {
        super.appendWeaponTooltip(stack, world, tooltip, context)
        addChargeTooltip(stack, tooltip)
    }

    private fun addChargeTooltip(stack: ItemStack, tooltip: MutableList<Text>) {
        val chargeLevel = getCurrentCharge(stack)
        if (chargeLevel > 0) {
            tooltip.add(
                Text.translatable("item.logic_thinkering.counter.info", chargeLevel)
                    .formatted(Formatting.GOLD)
            )
        }
    }
}
```


### Classe: ReinforcedCopperSword
```kotlin
class ReinforcedCopperSword(
    material: ToolMaterial = ToolMaterial(
        BlockTags.INCORRECT_FOR_STONE_TOOL,
        200,
        6.5f,
        2.0f,
        14,
        ItemTags.STONE_TOOL_MATERIALS),
    name: String = "reinforced_copper_sword",
    settings: Settings = Settings().registryKey(
        RegistryKey.of(
            RegistryKeys.ITEM,
            Identifier.of(MOD_ID, name)
        )
    )
) : SwordItem(material, 4.0F, 5.0F, settings), WeaponBehavior {

    private val decorator: WeaponBehavior = ChargeableWeaponDecorator(this, 2, 4.0F)

    override fun postHit(stack: ItemStack, target: LivingEntity, attacker: LivingEntity): Boolean {
        return decorator.onHit(stack, target, attacker)
    }

    override fun onHit(stack: ItemStack, target: LivingEntity, attacker: LivingEntity): Boolean {
        return super.postHit(stack, target, attacker)
    }

    override fun appendWeaponTooltip(
        stack: ItemStack,
        world: TooltipContext?,
        tooltip: MutableList<Text>,
        context: TooltipType
    ) {
        super.appendTooltip(stack, world, tooltip, context)
    }
}
```

### Modelagem

<center>
Figura 2 - Decorador

![Modelagem - Decorador](https://raw.githubusercontent.com/UnBArqDsw2024-2/2024.2_G1_Logic_Thinkering_Entrega_03/refs/heads/main/assets/decorador2.png)

<b>Fonte:</b> Queiroz, Thomas, 2025.
A implementação do padrão Decorador permitiu redefinir as funções utilizadas pelas armas do jogo de forma mais eficiente e menos redundante. Agora seria mais fácil fazer qualquer modificação para a lógica.

</center>

## Conclusão

<!--
-   **Resuma os pontos principais do trabalho.**
-   **Avalie se os objetivos foram alcançados e o impacto do trabalho.**
-   **Apresente perspectivas para melhorias ou trabalhos futuros.**
-->

<div align="justify">&emsp;&emsp;
A implementação deixou o código mais compreensível, e foi um exercício interessante no aprendizado da linguagem Kotlin. Por causa da forma que as funções são chamadas pelo jogo, não foi possível definir todas as funções que eu queria dentro do decorador, pois isso causaria um loop infinito. No entanto dentro do que foi possível, foi alcançado um bom avanço
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

| Matrícula | Aluno                      | Git                                   |
| --------- | -------------------------- | ------------------------------------- |
| 211062526 | Thomas Queiroz Souza Alves | [Thomas Q](https://github.com/thmasq) |

</div>

---

<center>

## Histórico de Versão

</center>

<!-- Lembre de alterar a data -->
<!-- É PRA POR O NOME, NÃO O USER DO GITHUB -->

<div style="margin: 0 auto; width: fit-content;">

| Versão | Data da alteração |      Alteração       |              Responsável              | Revisor | Data de revisão |
| :----: | :---------------: | :------------------: | :-----------------------------------: | :-----: | :-------------: |
|  1.0   |       05/01       | Criação do documento | [Thomas Q](https://github.com/thmasq) |   [Patricia Silva](https://github.com/patyhelenaa)      |    06/01/2025             |

</div>

## Controle de Revisão

|                    Revisor(es)                     |                     O que foi realizado                      |
| :------------------------------------------------: | :----------------------------------------------------------: |
| [Patricia Silva](https://github.com/patyhelenaa)   | Adição dos links para os subtópicos e titulos para os codigos no Decorador de Armas       |
|                                                    |                                                              |
