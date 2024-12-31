# Prototype - Item

## Introdução
<div align="justify">&emsp;&emsp;
Este documento apresenta o protótipo de itens do mod, representando a criação de todos os itens utilizados. A abordagem descrita visa otimizar a reutilização de recursos e atributos previamente configurados.
</div>

## Objetivo
<div align="justify">&emsp;&emsp;
O objetivo do protótipo é reutilizar atributos já configurados, minimizando o custo de criação de novas instâncias. Essa abordagem é particularmente útil em jogos, onde temos instâncias semelhantes, mas não idênticas, permitindo realizar um clone e modificar apenas os atributos necessários. No caso dos itens, essa técnica foi aplicada para reduzir o custo de criar novos itens repetidamente.
</div>

<center>
Figura 1 - Resumo Prototype

![resumo](https://raw.githubusercontent.com/UnBArqDsw2024-2/2024.2_G1_Logic_Thinkering_Entrega_03/refs/heads/main/assets/prototyperesumo.png)

<center><b>Fonte:</b> Carvalho, 2024.</center>
</center>

## Metodologia
<div align="justify">&emsp;&emsp;
Após dividir as responsabilidades de cada subgrupo, analisamos todas as metodologias sugeridas pela professora Milene no Aprender 3. 
Identificamos que o padrão de projeto Prototype seria ideal para o código, uma vez que ele reduz a criação excessiva de instâncias, 
um problema enfrentado no módulo 2. Após selecionar o padrão GoF adequado, testamos o código, ajustando-o aos padrões de arquitetura. 
Após validar o funcionamento correto e alinhado ao padrão, elaboramos o diagrama UML correspondente no Draw.io.
</div>


## Resultados

<!--
- **Apresente o produto final, como o diagrama ou solução desenvolvida.**
- **Desenvolva ao menos um parágrafo referenciando a figura**
- **Adicione "Figura 1 - Título da Figura/Quadro/Tabela" acima e "Fonte: " abaixo dela**
- Destaque os pontos principais ou insights obtidos durante o processo.
- **APRESENTE AS VERSÕES DO DIAGRAMA!! Podem usar o formato abaixo para poluir menos a página**
-->
### Interface: PrototypeItem
```java
package com.example;
import net.minecraft.item.Item;

public interface PrototypeItem {
    default void insertOnGroup(Item item) {
        GuiaInventario.adicionarItem(item);
    }
    PrototypeItem clone();
    void updateItem(String id);
}
```

### Classe: ConcreteItem
```java
package com.example;

import net.minecraft.item.Item;
import net.minecraft.item.BlockItem;
import net.minecraft.registry.Registries;
import net.minecraft.registry.Registry;
import net.minecraft.registry.RegistryKey;
import net.minecraft.registry.RegistryKeys;
import net.minecraft.util.Identifier;
import java.util.function.Function;

public class ConcreteItem implements PrototypeItem {
    private String id;
    public static Item ITEM;

    public ConcreteItem(String id) {
        if(id != null) setId(id);
    }

    @Override
    public ConcreteItem clone() {
        return new ConcreteItem(null);
    }

    @Override
    public void updateItem(String id) {
        RegistryKey<Item> key = RegistryKey.of(RegistryKeys.ITEM, Identifier.of(ExampleMod.MOD_ID, id));
        Function<Item.Settings, Item> factory = Item::new;
        Item item = factory.apply(new Item.Settings().registryKey(key));
        if (item instanceof BlockItem blockItem) blockItem.appendBlocks(Item.BLOCK_ITEMS, item);
        ConcreteItem.ITEM = Registry.register(Registries.ITEM, key, item);
        insertOnGroup(ConcreteItem.ITEM);
    }

    public void setId(String id) {
        this.id = id;
        updateItem(id);
    }

}
```

### Classe: ConcreteArmor
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


public class ConcreteArmor implements PrototypeItem {

    private enum ArmorType {
        HELMET, CHESTPLATE, LEGGINGS, BOOTS
    }
    private enum Material {
        REINFORCED_COPPER, REINFORCED_EMERALD, REINFORCED_AMETHYST
    }

    public static Item ITEM;
    private String id;
    private ArmorType type;
    private Material material;

    public ConcreteArmor(String id, String type, String material) {
        this.type = ArmorType.valueOf(type.toUpperCase());
        this.material = Material.valueOf(material.toUpperCase());
        if(id != null) setId(id);
    }

    @Override
    public ConcreteArmor clone() {
        return new ConcreteArmor(null, this.type.name(), this.material.name());
    }

    @Override
    public void updateItem(String id) {
        Function<Item.Settings, Item> factory = (settings) -> new ArmorItem(
                ModArmorMaterial.valueOf(this.material.name()),
                EquipmentType.valueOf(this.type.name()),
                settings);

        RegistryKey<Item> key = RegistryKey.of(RegistryKeys.ITEM, Identifier.of(ExampleMod.MOD_ID, id));
        Item.Settings settings = new Item.Settings();
        Item item = factory.apply(settings.registryKey(key));
        ConcreteArmor.ITEM = Registry.register(Registries.ITEM, key, item);

        insertOnGroup(ConcreteArmor.ITEM);
    }


    public void setId(String id) {
        this.id = id;
        updateItem(this.id);
    }

    public void setType(String type){
        this.type = ArmorType.valueOf(type.toUpperCase());
    }

    public void setMaterial(String material){
        this.material = Material.valueOf(material.toUpperCase());
    }
}
```

### Classe: ConcreteTool
```java
import net.minecraft.item.Item;
import net.minecraft.item.SwordItem;
import net.minecraft.item.AxeItem;
import net.minecraft.item.PickaxeItem;
import net.minecraft.item.ShovelItem;
import net.minecraft.item.HoeItem;
import net.minecraft.item.ToolMaterial;
import net.minecraft.registry.Registries;
import net.minecraft.registry.Registry;
import net.minecraft.registry.RegistryKey;
import net.minecraft.registry.RegistryKeys;
import net.minecraft.util.Identifier;
import java.util.function.Function;

public class ConcreteTool implements PrototypeItem {
    private enum ToolType {
        AXE, HOE, PICKAXE, SHOVEL, SWORD
    }
    private enum Material {
        REINFORCED_COPPER, REINFORCED_EMERALD, REINFORCED_AMETHYST
    }

    public static Item ITEM;
    private String id;
    private ToolType type;
    private Material material;

    public ConcreteTool(String id, String type, String material) {
        this.id = id;
        this.type = ToolType.valueOf(type.toUpperCase());
        this.material = Material.valueOf(material.toUpperCase());
    }

    @Override
    public ConcreteTool clone() {
        return new ConcreteTool(this.id, this.type.name(), this.material.name());
    }

    @Override
    public void updateItem(String id) {
        Function<Item.Settings, Item> factory = (settings) -> {
            ToolMaterial material = MateriaisFerramenta.valueOf(this.material.name());
            return switch (this.type) {
                case SWORD -> new SwordItem(material, 3, -1.9F, settings);
                case AXE -> new AxeItem(material, 6, -2.7F, settings);
                case PICKAXE -> new PickaxeItem(material, 1, -2.8F, settings);
                case SHOVEL -> new ShovelItem(material, 1.5F, -3.0F, settings);
                case HOE -> new HoeItem(material, -3, 0.0F, settings);
            };
        };

        RegistryKey<Item> key = RegistryKey.of(RegistryKeys.ITEM, Identifier.of(ExampleMod.MOD_ID, id));
        Item.Settings settings = new Item.Settings();
        Item item = factory.apply(settings.registryKey(key));
        ConcreteTool.ITEM = Registry.register(Registries.ITEM, key, item);

        insertOnGroup(ConcreteTool.ITEM);
    }

    public void setType(String type) {
        this.type = ToolType.valueOf(type.toUpperCase());
    }

    public void setMaterial(String material) {
        this.material = Material.valueOf(material.toUpperCase());
    }

    public void setId(String id) {
        this.id = id;
        updateItem(id);
    }

}
```

 <center>
Figura 2 - Prototype

![resumo](https://raw.githubusercontent.com/UnBArqDsw2024-2/2024.2_G1_Logic_Thinkering_Entrega_03/refs/heads/main/assets/creativeMenu.png)

 <b>Fonte:</b> Carvalho, Sandes, 2024.
</center>

 <center>
Figura 3 - Barra do criativo com os itens adicionados

![resumo](https://raw.githubusercontent.com/UnBArqDsw2024-2/2024.2_G1_Logic_Thinkering_Entrega_03/refs/heads/main/assets/prototype.png)

 <b>Fonte:</b> Carvalho, Sandes, 2024.
</center>

<div align="justify">&emsp;&emsp;
O diagrama apresentado descreve a estrutura de classes e os relacionamentos utilizados no desenvolvimento do mod, focando na criação dos itens personalizados. A implementação segue o padrão de projeto Prototype, evidenciado pela interface PrototypeItem, que define os métodos clone() e updateItem(), permitindo a reutilização e modificação de objetos. As classes concretas ConcreteItem, ConcreteArmor e ConcreteTool implementam a interface, especializando os comportamentos, bem como as funcionalidades de cada tipo de item. 
</div>


## Conclusão

<!--
-   **Resuma os pontos principais do trabalho.**
-   **Avalie se os objetivos foram alcançados e o impacto do trabalho.**
-   **Apresente perspectivas para melhorias ou trabalhos futuros.**
-->

<div align="justify">&emsp;&emsp;
Com o uso do padrão Prototype, foi possível otimizar a criação de itens no mod, garantindo eficiência e redução de custos relacionados 
à criação de instâncias repetitivas. Essa abordagem destacou-se como uma solução prática para os desafios enfrentados, 
contribuindo para a organização e manutenção do código. Entretanto, vale resaltar que foi realizado uma cópia profunda pra garantir
que o objeto clonado nao compartilhe referências com o protótipo, sendo assim o custo para clonagem se iguala ao custo de criar uma 
nova instância.
</div>

## Bibliografia

<!-- - **Altere!**-->

> [1] SERRANO, Milene. Videoaula: [08d - Vídeo-Aula - DSW - GoFs - Criacionais - Demais Padrões - Visão Rápida](https://unbbr-my.sharepoint.com/:v:/g/personal/mileneserrano_unb_br/EdG2MN8pFrJOpSMnzcCDJFkB2FCnvQlUOuGUs6ehjZYv6A?e=yl3dpY). [Online]. Disponível em: [https://unbbr-my.sharepoint.com/:v:/g/personal/mileneserrano_unb_br/EdG2MN8pFrJOpSMnzcCDJFkB2FCnvQlUOuGUs6ehjZYv6A?e=yl3dpY](https://unbbr-my.sharepoint.com/:v:/g/personal/mileneserrano_unb_br/EdG2MN8pFrJOpSMnzcCDJFkB2FCnvQlUOuGUs6ehjZYv6A?e=yl3dpY) . Acesso em: 28 dez. 2024.

> [2] MIRANDA, Ótavio. Videoaula: [Prototype Teoria - Padrões de Projeto - Parte 8/45](https://youtu.be/Z-_smcjkdwM?si=acuF2pyFHgE5ewok). [Online]. Disponível em: [https://youtu.be/Z-_smcjkdwM?si=acuF2pyFHgE5ewok](https://youtu.be/Z-_smcjkdwM?si=acuF2pyFHgE5ewok) . Acesso em: 27 dez. 2024.

> [3] BANAS, derek. Videoaula: [Prototype Design Pattern Tutorial](https://youtu.be/AFbZhRL0Uz8?si=Ummno3Obz_fIImEX). [Online]. Disponível em: [https://youtu.be/AFbZhRL0Uz8?si=Ummno3Obz_fIImEX](https://youtu.be/AFbZhRL0Uz8?si=Ummno3Obz_fIImEX) . Acesso em: 27 dez. 2024.

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
|  1.0   |       28/12       | Criação do documento | [Eduardo Sandes](https://github.com/DiceRunner714), [Gabriela Lemos](https://github.com/heylisten64), [João Antonio G. Carvalho](https://github.com/i-JSS) |         |                 |

</div>

## Controle de Revisão

|                        Revisor(es)                        | O que foi realizado |
| :-------------------------------------------------------: |:-------------------:|
| |                     |