# Builder

## Introdução

<div align="justify">&emsp;&emsp;
O Builder é um padrão de design criacional que permite construir objetos completos passo a passo¹. permitindo a produção de diferentes representações do mesmo objeto usando o mesmo código
</div>

## Objetivo

<div align="justify">&emsp;&emsp;
O objetivo do builder é simplificar a criação de objetos complexos, com vários campos e ou objetos internos, com construtores com vários parâmetros, dentre outras complicações
</div>

## Metodologia
<div align="justify">&emsp;&emsp; Foram criadas classes chamadas BlockRegistryHelper e ItemRegitryHelper para simplificar o registro de vários blocos através de um API simples, pois o registro de vários blocos é verboso e complicado. Como a classe é complexa cada classe é acompanhada de um builder, que especifica uma API simples para criação dos objetos.

## Resultados

### Classe: BlockRegistryHelper

```kotlin
class BlockRegistryHelper(
    private val blocks : List<Pair<BlockInit, String>>,
    private val itemGroup: RegistryKey<ItemGroup>?,
    private val baseSettings: AbstractBlock.Settings,
    private val registerItems: Boolean,
    private val decorator: Decorator?,
) {
    private fun registerBlock(init: BlockInit, name: String, registerItem: Boolean) : Pair<Block, String> {
        logger.info("Registering $name block")
        val id = Identifier.of(MOD_ID, name)
        val key = RegistryKey.of(RegistryKeys.BLOCK, id)
        val settings = baseSettings.registryKey(key)
        val block = init(settings)
        Registry.register(Registries.BLOCK, id, block).also {
            if (registerItem) {
                val itemKey = RegistryKey.of(RegistryKeys.ITEM, id)
                val itemSettings = Item.Settings().registryKey(itemKey)
                Registry.register(Registries.ITEM, id, BlockItem(it, itemSettings))
                ItemGroupEvents.modifyEntriesEvent(itemGroup).register {it.add(block)}
            }
        }

        return block to name
    }

    /**
     * Registers all blocks in the registry, using the provided settings and item group.
     * This method also ensures that the all the items will be registered for each block
     * if registerItems is set
     *
     * @throws IllegalStateException If settings are not defined before block registration.
     */
    fun register() {
        if (registerItems && itemGroup == null)
            throw IllegalStateException("Item group must be set before block item registration")
        if (decorator == null) {
            blocks.forEach {(init, name) -> registerBlock(init, name, registerItems)}
        } else {
            blocks
                .map { (init, name) -> registerBlock(init, name, false)}
                .forEach { (block, name) -> registerBlock({ decorator!!(block)}, name+"_decorated", registerItems) }
        }
    }
}
```

### Função: buildRegistryHelper

```kotlin
/**
 * Registers a list of blocks in the Minecraft registry with a specified group and settings.
 *
 * @param init A lambda function used to initialize the `BlockRegistryBuilder` and register blocks.
 */
fun buildRegistryHelper(init: BlockRegistryBuilder.() -> Unit) : BlockRegistryHelper {
    val builder = BlockRegistryBuilder()
    builder.init()
    return builder.build()
}

```

### Classe: ItemRegistryHelper

```kotlin
/**
 * Type alias for a function that initializes a block with settings and returns a `Block` instance.
 *
 * @param settings Block settings (used in block registration).
 * @return A newly created `Block` instance.
 */
typealias BlockInit = (Settings) -> Block
typealias Decorator = (Block) -> Block

@DslMarker
annotation class BlockRegistryDsl
/**
 * A builder class for registering blocks with specific settings.
 * It supports configuring settings, item group, and whether items should be registered for blocks.
 */
@BlockRegistryDsl
class BlockRegistryBuilder {
    private val blocks = mutableListOf<Pair<BlockInit, String>>()
    private var itemGroup: RegistryKey<ItemGroup>? = null
    private var baseSettings: Settings? = null
    private var registerItems = true
    private var decorator: Decorator? = null

    /**
     * Sets a decorator for the blocks being registered
     *
     * @param decorator the decorator to be applied to the blocks
     * @return The current instance of 'BlockRegistryBuilder'
     */
    fun decorator(decorator: Decorator) = apply { this.decorator = decorator }

    /**
     * Sets the settings for the blocks being registered.
     *
     * @param settings The settings to be applied to the blocks.
     * @return The current instance of `BlockRegistryBuilder`.
     */
    fun settings(settings: Settings) = apply { this.baseSettings = settings }

    /**
     * Sets the item group that the block items will be added to.
     *
     * @param group The item group to add the block items to.
     * @return The current instance of `BlockRegistryBuilder`.
     */
    fun group(group: RegistryKey<ItemGroup>) = apply { itemGroup = group }

    /**
     * Sets whether to register the items for each block.
     * defaults to True.
     *
     * @param group The item group to add the block items to.
     * @return The current instance of `BlockRegistryBuilder`.
     */
    fun registerItems(registerItems: Boolean) = apply { this.registerItems = registerItems }

    /**
     * Registers a block initialization function with a name.
     *
     * @param name The name for the block being registered.
     */
    infix fun BlockInit.with(name: String) {
        blocks += this to name
    }

    fun build() : BlockRegistryHelper {
        if (baseSettings == null)
            throw IllegalStateException("Settings must be set before block registration")
        return BlockRegistryHelper(blocks, itemGroup, baseSettings!!, registerItems, decorator)
    }
}
```

### Classe: BlockRegistryBuilder

```java
package com.logic_thinkering.block.clock;

import java.util.Random;

public class RandomPulseGenerator implements PulseGenerator{
    private final Random random = new Random();

    @Override
    public int generatePulse() {
        return 20 + random.nextInt(100);
    }
}
```

### Função: registerItems

```kotlin
/**
 * Registers a list of items in the Minecraft registry with a specified group and settings.
 *
 * @param init A lambda function used to initialize the `ItemRegistryBuilder` and register items.
 */
fun registerItems(init: ItemRegistryBuilder.() -> Unit) : ItemRegistryHelper {
    val builder = ItemRegistryBuilder()
    builder.init()
    return builder.build()
}

```

### Classe: ItemRegistryBuilder

```kotlin
@DslMarker
annotation class ItemRegistryDsl

/**
 * A builder class for registering Items with specific settings and item groups.
 */
@ItemRegistryDsl
class ItemRegistryBuilder {
    private val items = mutableListOf<Pair<Item, String>>()
    private var itemGroup: RegistryKey<ItemGroup>? = null

    /**
     * Sets the item group that the items will be added to.
     *
     * @param group The group to add the items to.
     * @return The current instance of `ItemRegistryBuilder`.
     */
    fun group(group: RegistryKey<ItemGroup>) = apply { itemGroup = group }

    /**
     * Adds an item with a specific identifier to be registered.
     *
     * @param name The name for the item being registered.
     */
    infix fun Item.with(name: String) {
        items += this to name
    }

    fun build() : ItemRegistryHelper {
        if (itemGroup == null)
            throw IllegalStateException("Item group must be set before registration")
        return ItemRegistryHelper(items, itemGroup!!)
    }
}
```
### Modelagem

<center>
Figura 1 - Builder

![Modelagem - Factory Method](https://raw.githubusercontent.com/UnBArqDsw2024-2/2024.2_G1_Logic_Thinkering_Entrega_03/refs/heads/main/assets/builder.png)

<b>Fonte:</b> Silva, André, 2025.
implementação do padrão Builder permitiu uma interface simplificada para a criação dos objetos ajudadores de registro commo se pode ver no diagrama, abstraindo a complexidade da criação do objeto e do registro de blocos e itens.

</center>

## Conclusão

<!--
-   **Resuma os pontos principais do trabalho.**
-   **Avalie se os objetivos foram alcançados e o impacto do trabalho.**
-   **Apresente perspectivas para melhorias ou trabalhos futuros.**
-->

<div align="justify">&emsp;&emsp;
Em suma foram utilizadas quatro classes para aplicar o padrão builder para registro de itens e blocos. De acordo com o esperado, a classe builder simplificou a criação dos objetos ajudadores. Além disso, as classes poderiam ser melhoradas no futuro quanto mais necessidades podem aparecer para o registro de itens, como diferentes grupos para diferentes itens, e não só um grupo para todos os itens, adição de múltiplos decoradores, e outras melhorias de flexibilidade
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
