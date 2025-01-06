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
package com.logic_thinkering.registration

import net.minecraft.block.AbstractBlock.Settings
import net.minecraft.block.Block
import net.minecraft.item.ItemGroup
import net.minecraft.registry.RegistryKey



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
 * Registers a list of blocks in the Minecraft registry with a specified group and settings.
 *
 * @param init A lambda function used to initialize the `BlockRegistryBuilder` and register blocks.
 */
fun buildRegistryHelper(init: BlockRegistryBuilder.() -> Unit) : BlockRegistryHelper {
    val builder = BlockRegistryBuilder()
    builder.init()
    return builder.build()
}

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


### Modelagem

<center>
Figura 1 - Decorador

![Modelagem - Decorador](https://raw.githubusercontent.com/UnBArqDsw2024-2/2024.2_G1_Logic_Thinkering_Entrega_03/refs/heads/main/assets/decorador.png)

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
|  1.0   |       05/01       | Criação do documento | [André Emanuel Bispo da Silva](https://github.com/Hunter104) | [Patricia Silva](https://github.com/patyhelenaa)        |   06/01/2025              |

</div>

## Controle de Revisão

|                    Revisor(es)                     |                     O que foi realizado                      |
| :------------------------------------------------: | :----------------------------------------------------------: |
| [Patricia Silva](https://github.com/patyhelenaa)   | Adição dos links para os subtópicos e código da classe       |
|                                                    |                                                              |
