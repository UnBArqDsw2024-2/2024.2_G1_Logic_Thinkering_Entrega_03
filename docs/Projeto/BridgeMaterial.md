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
package com.logic_thinkering.itens;

public abstract class PrototypeItem {
    protected StrategyRegister strategy;
    protected Material material;

    public abstract PrototypeItem clone();
    public abstract void register(String id);
    public abstract void updateMaterial(Material material);
}
```

### Classe: ConcreteArmor

```java
package com.logic_thinkering.itens;

import net.minecraft.item.Item;

public class ConcreteArmor extends PrototypeItem {

    public static Item ITEM;
    private String id;
    private ArmorType type;

    public ConcreteArmor(String id, ArmorType type, Material material) {
        if (material instanceof LogicThinkeringArmorMaterial materialArmadura) {
            strategy = new ConcreteRegisterArmor();
            this.type = type;
            this.material = materialArmadura;
            if(id != null) setId(id);
        }
    }

    @Override
    public ConcreteArmor clone() {
        return new ConcreteArmor(null, this.type, this.material);
    }

    @Override
    public void register(String id) {
        ITEM = strategy.register(id, this.material, this.type.name());
    }

    public void setId(String id) {
        this.id = id;
        register(this.id);
    }

    public void setType(String type){
        this.type = ArmorType.valueOf(type.toUpperCase());
    }

    public void updateMaterial(Material material){
        if (material instanceof LogicThinkeringArmorMaterial materialArmadura) {
            this.material = materialArmadura;
        }
    }

}
```

### Classe: ConcreteItem

```java
package com.logic_thinkering.itens;

import net.minecraft.item.Item;
import net.minecraft.item.BlockItem;
import net.minecraft.registry.Registries;
import net.minecraft.registry.Registry;
import net.minecraft.registry.RegistryKey;
import net.minecraft.registry.RegistryKeys;
import net.minecraft.util.Identifier;
import java.util.function.Function;

public class ConcreteItem extends PrototypeItem {
    private String id;
    public static Item ITEM;

    public ConcreteItem(String id) {
        this.material = null;
        strategy = new ConcreteRegisterItem();
        if(id != null) setId(id);
    }

    @Override
    public ConcreteItem clone() {
        return new ConcreteItem(null);
    }

    @Override
    public void register(String id) {
        ITEM = strategy.register(id);
    }

    @Override
    public void updateMaterial(Material material) {
        throw new IllegalArgumentException("This class does not support updateMaterial");
    }

    public void setId(String id) {
        this.id = id;
        register(id);
    }

}
```

### Classe: ConcreteTool

```java
package com.logic_thinkering.itens;

import net.minecraft.item.Item;

public class ConcreteTool extends PrototypeItem {

    public static Item ITEM;
    private String id;
    private ToolType type;

    public ConcreteTool(String id, ToolType type, Material material) {
        if (material instanceof LogicThinkeringToolMaterial materialtool) {
            strategy = new ConcreteRegisterTool();
            this.type = type;
            this.material = materialtool;
            if(id != null) setId(id);
        }
    }

    @Override
    public ConcreteTool clone() {
        return new ConcreteTool(null, this.type, this.material);
    }

    @Override
    public void register(String id) {
        ITEM = strategy.register(id, this.material, this.type.name());
    }

    public void setType(String type) {
        this.type = ToolType.valueOf(type.toUpperCase());
    }

    public void updateMaterial(Material material) {
        if (material instanceof LogicThinkeringToolMaterial materialtool) {
            this.material = materialtool;
        }
    }

    public void setId(String id) {
        this.id = id;
        register(id);
    }

}
```

### Interface: Material

```java
package com.logic_thinkering.itens;

import net.minecraft.item.Item;
import net.minecraft.registry.tag.TagKey;

public interface Material {
    void updateSettings(int i, int j, float k, float l);
    void updateItemTag(TagKey<Item> repairIngredient);
    String getName();
}
```

### Classe: ModArmorMaterial

```java
package com.logic_thinkering.itens;

import net.minecraft.item.Item;
import net.minecraft.item.equipment.ArmorMaterial;
import net.minecraft.item.equipment.EquipmentType;
import net.minecraft.registry.entry.RegistryEntry;
import net.minecraft.registry.tag.TagKey;
import net.minecraft.sound.SoundEvent;
import net.minecraft.util.Identifier;
import java.util.EnumMap;


public class LogicThinkeringArmorMaterial implements Material {

    private ArmorMaterial material;
    private String name;

    public LogicThinkeringArmorMaterial(
            String name,
            int durability,
            int[] defense,
            int enchantmentValue,
            RegistryEntry<SoundEvent> equipSound,
            float toughness,
            float knockbackResistance,
            TagKey<Item> repairIngredient,
            Identifier modelId
    ) {
        EnumMap<EquipmentType, Integer> map = new EnumMap<>(EquipmentType.class);
        map.put(EquipmentType.BOOTS, defense[0]);
        map.put(EquipmentType.LEGGINGS, defense[1]);
        map.put(EquipmentType.CHESTPLATE, defense[2]);
        map.put(EquipmentType.HELMET, defense[3]);
        map.put(EquipmentType.BODY, defense[4]);

        this.name = name;
        this.material = new ArmorMaterial(
                durability,
                map,
                enchantmentValue,
                equipSound,
                toughness,
                knockbackResistance,
                repairIngredient,
                modelId);
    }

    public ArmorMaterial getMaterial() {
        return material;
    }

    @Override
    public String getName() {
        return name;
    }

    @Override
    public void updateSettings(int i, int j, float k, float l) {

        this.material = new ArmorMaterial(
                i,
                material.defense(),
                j,
                material.equipSound(),
                k,
                l,
                material.repairIngredient(),
                material.modelId());
    }

    @Override
    public void updateItemTag(TagKey<Item> repairIngredient) {
        this.material = new ArmorMaterial(
                material.durability(),
                material.defense(),
                material.enchantmentValue(),
                material.equipSound(),
                material.toughness(),
                material.knockbackResistance(),
                repairIngredient,
                material.modelId());
    }
}
```

### Classe: ModToolMaterial

```java
package com.logic_thinkering.itens;

import net.minecraft.block.Block;
import net.minecraft.item.ToolMaterial;
import net.minecraft.registry.tag.TagKey;
import net.minecraft.item.Item;

public class LogicThinkeringToolMaterial implements Material {

    private ToolMaterial material;
    private String name;

    public LogicThinkeringToolMaterial(
            String name,
            TagKey<Block> incorrectBlocksForDrops,
            int durability,
            float speed,
            float attackDamageBonus,
            int enchantmentValue,
            TagKey<Item> repairItems
    ) {
        this.name = name;
        this.material = new ToolMaterial(
                incorrectBlocksForDrops,
                durability,
                speed,
                attackDamageBonus,
                enchantmentValue,
                repairItems
        );
    }

    public ToolMaterial getMaterial() {
        return material;
    }

    @Override
    public void updateSettings(int i, int j, float k, float l) {
        this.material = new ToolMaterial(
                material.incorrectBlocksForDrops(),
                i, l, k, j,
                material.repairItems()
        );
    }

    @Override
    public void updateItemTag(TagKey<Item> repairItems) {
        this.material = new ToolMaterial(
                material.incorrectBlocksForDrops(),
                material.durability(),
                material.speed(),
                material.attackDamageBonus(),
                material.enchantmentValue(),
                repairItems
        );
    }

    @Override
    public String getName() {
        return name;
    }

}
```

<center>
Figura 1 - Bridge

![v2](https://raw.githubusercontent.com/UnBArqDsw2024-2/2024.2_G1_Logic_Thinkering_Entrega_03/refs/heads/main/assets/bridgeMaterial.png)

 <b>Fonte:</b> Carvalho, Sandes, 2025.
</center>

<div align="justify">&emsp;&emsp; A implementação do padrão Bridge trouxe resultados expressivos na organização e flexibilidade do sistema de gerenciamento de itens e materiais do mod "Logic Thinkering". A separação clara entre a abstração e a implementação reduziu a complexidade e a quantidade de subclasses necessárias. Além disso, o desacoplamento das hierarquias permite a rápida adição de novos tipos de materiais ou itens sem necessidade de refatorações extensivas no futuro. 
</div>

 <div align="justify">&emsp;&emsp; 
 Entre os principais benefícios observados estão a escalabilidade do mod, bem como a simplificação do código, que agora é capaz de lidar com variações futuras de maneira eficiente. A modularidade introduzida possibilitou o reaproveitamento de componentes existentes, minimizando duplicações. 
 </div>

<div align="justify">&emsp;&emsp; Por fim, o uso do padrão Bridge foi fundamental para atingir uma arquitetura flexível e extensível, atendendo plenamente os objetivos propostos de desacoplamento e reuso. </div>

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

A Figura 1 apresenta a versão 1.0 do UML.

<center>
Figura 1 - Bridge

![v1](https://raw.githubusercontent.com/UnBArqDsw2024-2/2024.2_G1_Logic_Thinkering_Entrega_03/refs/heads/main/assets/bridgeMaterial.png)

<b>Fonte:</b> Carvalho, Sandes, 2025.
</center>

</details>


## Bibliografia

<!-- - **Altere!**-->

> [1] SERRANO, Milene. Videoaula: [09d - Vídeo-Aula - DSW - GoFs - Estruturais - Demais Padrões - Visão Rápida](https://unbbr-my.sharepoint.com/:v:/g/personal/mileneserrano_unb_br/EZzj0BVtpTxMnBCtiUlE0r8BD7Tw_XOeK1J8Jn__qgod0A?e=GQQXjZ). [Online]. Disponível em: [https://unbbr-my.sharepoint.com/:v:/g/personal/mileneserrano_unb_br/EZzj0BVtpTxMnBCtiUlE0r8BD7Tw_XOeK1J8Jn__qgod0A?e=GQQXjZ](https://unbbr-my.sharepoint.com/:v:/g/personal/mileneserrano_unb_br/EZzj0BVtpTxMnBCtiUlE0r8BD7Tw_XOeK1J8Jn__qgod0A?e=GQQXjZ) . Acesso em: 04 jan. 2025.

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