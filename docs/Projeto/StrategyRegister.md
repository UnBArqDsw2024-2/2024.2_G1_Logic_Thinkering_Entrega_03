# Strategy - Register

## Introdução
<div align="justify">&emsp;&emsp;
Este documento apresenta o strategy usado para registrar os itens, ferramentas, blocos e armaduras do mod visando encapsular a escolha dos métodos usados para registrar cada item.
</div>

## Objetivo
<div align="justify">&emsp;&emsp;
O objetivo do strategy é permitir que um conjunto de algoritmos seja definido e encapsulado separadamente de forma que a escolha de qual será usado possa ser feita em tempo de execução. Resumidamente, o strategy promove a separação entre o comportamento de uma classe e a lógica que a implementa.
</div>

<!-- <center>
Figura 1 - Resumo Prototype

![resumo](https://raw.githubusercontent.com/UnBArqDsw2024-2/2024.2_G1_Logic_Thinkering_Entrega_03/refs/heads/main/assets/prototyperesumo.png)

<center><b>Fonte:</b> Carvalho, 2024.</center>
</center> -->

## Metodologia
<div align="justify">&emsp;&emsp;
Durante a fase de implementação, notamos não só os itens, mas também as ferramentas, blocos e armaduras precisavam ser registrados no Minecraft. No entanto, cada tipo de item tem propriedades e comportamentos diferentes de modo que seria impossível registrar todos usando o mesmo método. Para resolver essa questão, adotamos o padrão Strategy, permitindo encapsular os comportamentos específicos de cada tipo de objeto em classes separadas.
</div>

## Resultados

<!--
- **Apresente o produto final, como o diagrama ou solução desenvolvida.**
- **Desenvolva ao menos um parágrafo referenciando a figura**
- **Adicione "Figura 1 - Título da Figura/Quadro/Tabela" acima e "Fonte: " abaixo dela**
- Destaque os pontos principais ou insights obtidos durante o processo.
- **APRESENTE AS VERSÕES DO DIAGRAMA!! Podem usar o formato abaixo para poluir menos a página**
-->

### Interface: StrategyRegister
```java
package com.example;

import net.minecraft.item.Item;

public interface StrategyRegister {
    default void insertOnGroup(Item item) {
        ModItemGroup.addItem(item);
    }
    Item register(String id);
    Item register(String id, String material, String type);
}
```

### Classe: ConcreteRegisterItem
```java
package com.example;

import net.minecraft.item.BlockItem;
import net.minecraft.item.Item;
import net.minecraft.registry.Registries;
import net.minecraft.registry.Registry;
import net.minecraft.registry.RegistryKey;
import net.minecraft.registry.RegistryKeys;
import net.minecraft.util.Identifier;

import java.util.function.Function;

public class ConcreteRegisterItem implements StrategyRegister {

    @Override
    public Item register(String id) {
        RegistryKey<Item> key = RegistryKey.of(RegistryKeys.ITEM, Identifier.of(ExampleMod.MOD_ID, id));
        Function<Item.Settings, Item> factory = Item::new;
        Item item = factory.apply(new Item.Settings().registryKey(key));
        if (item instanceof BlockItem blockItem) blockItem.appendBlocks(Item.BLOCK_ITEMS, item);
        Item result =  Registry.register(Registries.ITEM, key, item);
        insertOnGroup(result);
        return(result);
    }

    @Override
    public Item register(String id, String material, String type) {
        throw new UnsupportedOperationException("Register with material and type is not supported for generic items.");
    }

}
```

### Classe: ConcreteRegisterTool
```java
package com.example;

import net.minecraft.item.*;
import net.minecraft.registry.Registries;
import net.minecraft.registry.Registry;
import net.minecraft.registry.RegistryKey;
import net.minecraft.registry.RegistryKeys;
import net.minecraft.util.Identifier;

import java.util.function.Function;

public class ConcreteRegisterTool implements StrategyRegister {

    @Override
    public Item register(String id) {
        throw new UnsupportedOperationException("Use register with material and type for armor.");
    }

    @Override
    public Item register(String id, String material, String type) {
        Function<Item.Settings, Item> factory = (settings) -> {
            ToolMaterial materialtool = MateriaisFerramenta.valueOf(material);

            return switch (type) {
                case "SWORD" -> new SwordItem(materialtool, 3, -1.9F, settings);
                case "AXE" -> new AxeItem(materialtool, 6, -2.7F, settings);
                case "PICKAXE" -> new PickaxeItem(materialtool, 1, -2.8F, settings);
                case "SHOVEL" -> new ShovelItem(materialtool, 1.5F, -3.0F, settings);
                case "HOE" -> new HoeItem(materialtool, -3, 0.0F, settings);
                default -> throw new IllegalStateException("Unexpected value: " + type);
            };

        };

        RegistryKey<Item> key = RegistryKey.of(RegistryKeys.ITEM, Identifier.of(ExampleMod.MOD_ID, id));
        Item.Settings settings = new Item.Settings();
        Item item = factory.apply(settings.registryKey(key));
        Item result = Registry.register(Registries.ITEM, key, item);
        insertOnGroup(result);
        return(result);
    }

}
```

### Classe: ConcreteRegisterArmor
```java
package com.example;

import net.minecraft.item.ArmorItem;
import net.minecraft.item.Item;
import net.minecraft.item.equipment.EquipmentType;
import net.minecraft.registry.Registries;
import net.minecraft.registry.Registry;
import net.minecraft.registry.RegistryKey;
import net.minecraft.registry.RegistryKeys;
import net.minecraft.util.Identifier;

import java.util.function.Function;

public class ConcreteRegisterArmor implements StrategyRegister {

    @Override
    public Item register(String id) {
        throw new UnsupportedOperationException("Use register with material and type for armor.");
    }

    @Override
    public Item register(String id, String material, String type) {
        Function<Item.Settings, Item> factory = (settings) -> new ArmorItem(
                ModArmorMaterial.valueOf(material),
                EquipmentType.valueOf(type),
                settings);

        RegistryKey<Item> key = RegistryKey.of(RegistryKeys.ITEM, Identifier.of(ExampleMod.MOD_ID, id));
        Item.Settings settings = new Item.Settings();
        Item item = factory.apply(settings.registryKey(key));
        Item result = Registry.register(Registries.ITEM, key, item);
        insertOnGroup(result);
        return(result);
    }
}
```

### Classe: ConcreteRegisterBlock
```java
a fazer
```


<center>
Figura 1 - Strategy

![resumo](https://raw.githubusercontent.com/UnBArqDsw2024-2/2024.2_G1_Logic_Thinkering_Entrega_03/refs/heads/main/assets/strategyregister.jpeg)

 <b>Fonte:</b> Carvalho, Sandes, 2025.
</center>

<div align="justify">&emsp;&emsp;
O diagrama apresentado descreve a estrutura de classes e os relacionamentos utilizados para registro dos itens do mod. A implementação segue o padrão de projeto Strategy, evidenciado pela interface StrategyRegister, que define a assinatura dos métodos para registro register(String id) e register(String id, String material, String type). As classes concretas ConcreteRegisterItem, ConcreteRegisterArmor, ConcreteRegisterTool e ConcreteRegisterBlock implementam esses métodos de acordo com as características e necessidades de cada caso, permitindo adaptação em tempo de execução. 
</div>

## Conclusão

<!--
-   **Resuma os pontos principais do trabalho.**
-   **Avalie se os objetivos foram alcançados e o impacto do trabalho.**
-   **Apresente perspectivas para melhorias ou trabalhos futuros.**
-->

<div align="justify">&emsp;&emsp;
Com a implementação do padrão Strategy, foi possível registrar diferentes tipos de objetos no mod de forma flexível e organizada, adaptando o comportamento de registro para atender às características específicas de itens, ferramentas, blocos e armaduras. Essa abordagem promoveu a separação de responsabilidades e facilitou a manutenção do código, permitindo alterações e expansões futuras sem impactar outras partes do sistema. Além disso, o uso do Strategy reduziu a complexidade no gerenciamento dos registros ao eliminar a necessidade de estruturas condicionais extensas oferecendo também maior facilidade de adicionar métodos de registros para outros tipos de objetos futuramente.
</div>

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
|  1.0   |       02/01       | Criação do documento | [Eduardo Sandes](https://github.com/DiceRunner714), [Gabriela Lemos](https://github.com/heylisten64), [João Antonio G. Carvalho](https://github.com/i-JSS) |         |                 |

</div>

## Controle de Revisão

|                        Revisor(es)                        | O que foi realizado |
| :-------------------------------------------------------: |:-------------------:|
| |                     |