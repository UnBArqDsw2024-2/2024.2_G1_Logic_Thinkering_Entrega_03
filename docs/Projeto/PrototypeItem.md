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
### Classe abstrata: PrototypeItem (Adaptação)
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

## Utilização do prototype

<div align="justify">&emsp;&emsp; O padrão de projeto Prototype foi utilizado para otimizar a criação de itens no sistema. Para isso, implementamos três funções principais: `insertCommonItems`, `insertAllArmors` e `insertAllTools`. Essas funções permitem clonar objetos base (prototypes) e modificar apenas os atributos necessários para atender às especificações de cada item. Esse método reduz significativamente o impacto de performance, aproveitando ao máximo a eficiência do padrão Prototype ao evitar a criação redundante de objetos do zero. </div>

```java
package com.logic_thinkering;

import com.logic_thinkering.itens.*;
import net.minecraft.item.equipment.EquipmentModels;
import net.minecraft.registry.tag.BlockTags;
import net.minecraft.registry.tag.ItemTags;
import net.minecraft.sound.SoundEvents;

import java.util.ArrayList;
import java.util.List;

public class LogicThinkeringItem {
    public List<ConcreteItem> items = new ArrayList<>();
    public List<ConcreteArmor> armors = new ArrayList<>();
    public List<ConcreteTool> tools = new ArrayList<>();

    public List<LogicThinkeringArmorMaterial> armorMaterials = new ArrayList<>();
    public List<LogicThinkeringToolMaterial> toolMaterials = new ArrayList<>();

    public LogicThinkeringItem() {
        items.add(new ConcreteItem("reinforced_copper_ingot"));
        insertCommonItem("reinforced_emerald_shard");
        insertCommonItem("reinforced_amethyst_shard");

        armorMaterials.add(new LogicThinkeringArmorMaterial("REINFORCED_COPPER", 37, new int[]{3, 6, 8, 3, 11}, 15, SoundEvents.ITEM_ARMOR_EQUIP_NETHERITE, 3.0F, 0.1F, ItemTags.REPAIRS_GOLD_ARMOR, EquipmentModels.NETHERITE));
        armorMaterials.add(new LogicThinkeringArmorMaterial("REINFORCED_EMERALD", 37, new int[]{3, 6, 8, 3, 11}, 15, SoundEvents.ITEM_ARMOR_EQUIP_NETHERITE, 3.0F, 0.1F, ItemTags.REPAIRS_GOLD_ARMOR, EquipmentModels.NETHERITE));
        armorMaterials.add(new LogicThinkeringArmorMaterial("REINFORCED_AMETHYST", 37, new int[]{3, 6, 8, 3, 11}, 15, SoundEvents.ITEM_ARMOR_EQUIP_NETHERITE, 3.0F, 0.1F, ItemTags.REPAIRS_GOLD_ARMOR, EquipmentModels.NETHERITE));

        toolMaterials.add(new LogicThinkeringToolMaterial("REINFORCED_COPPER", BlockTags.INCORRECT_FOR_STONE_TOOL, 200, 6.5F, 2.0F, 14, ItemTags.STONE_TOOL_MATERIALS));
        toolMaterials.add(new LogicThinkeringToolMaterial("REINFORCED_EMERALD", BlockTags.INCORRECT_FOR_STONE_TOOL, 250, 6.5F, 3.0F, 14, ItemTags.STONE_TOOL_MATERIALS));
        toolMaterials.add(new LogicThinkeringToolMaterial("REINFORCED_AMETHYST", BlockTags.INCORRECT_FOR_STONE_TOOL, 500, 6.5F, 5.0F, 14, ItemTags.STONE_TOOL_MATERIALS));

        insertAllArmors();
        insertAllTools();
    }

    private void insertCommonItem(String id){
        ConcreteItem clone = items.getFirst().clone();
        clone.register(id);
        items.add(clone);
    }

    private void insertAllArmors() {
        for (ArmorType armorType : ArmorType.values()) {
            ConcreteArmor prototype = new ConcreteArmor(armorMaterials.getFirst().getName().toLowerCase() + "_" + armorType.name().toLowerCase(), armorType, armorMaterials.getFirst());
            armors.add(prototype);

            for (int i = 1; i < armorMaterials.size(); i++) {
                ConcreteArmor clone = prototype.clone();
                clone.updateMaterial(armorMaterials.get(i));
                clone.register(armorMaterials.get(i).getName().toLowerCase() + "_" + armorType.name().toLowerCase());
                armors.add(clone);
            }
        }
    }

    private void insertAllTools() {
        for (ToolType toolType : ToolType.values()) {
            ConcreteTool prototype = new ConcreteTool(toolMaterials.getFirst().getName().toLowerCase() + "_" + toolType.name().toLowerCase(), toolType, toolMaterials.getFirst());
            tools.add(prototype);

            for (int i = 1; i < toolMaterials.size(); i++) {
                ConcreteTool clone = prototype.clone();
                clone.updateMaterial(toolMaterials.get(i));
                clone.register(toolMaterials.get(i).getName().toLowerCase() + "_" + toolType.name().toLowerCase());
                tools.add(clone);
            }
        }
    }

}
```

### Imagens

 <center>
Figura 2 - Prototype

![resumo](https://raw.githubusercontent.com/UnBArqDsw2024-2/2024.2_G1_Logic_Thinkering_Entrega_03/refs/heads/main/assets/creativeMenu.png)

 <b>Fonte:</b> Carvalho, Sandes, 2024.
</center>

 <center>
Figura 3 - Barra do criativo com os itens adicionados

![v2](https://raw.githubusercontent.com/UnBArqDsw2024-2/2024.2_G1_Logic_Thinkering_Entrega_03/refs/heads/main/assets/prototypev2.png)

 <b>Fonte:</b> Carvalho, Sandes, 2024.
</center>



## Conclusão

<!--
-   **Resuma os pontos principais do trabalho.**
-   **Avalie se os objetivos foram alcançados e o impacto do trabalho.**
-   **Apresente perspectivas para melhorias ou trabalhos futuros.**
-->

<div align="justify">&emsp;&emsp;
Com o uso do padrão Prototype, foi possível otimizar a criação de itens no mod, garantindo eficiência e redução de custos relacionados 
à criação de instâncias repetitivas. Essa abordagem destacou-se como uma solução prática para os desafios enfrentados, 
contribuindo para a organização e manutenção do código. Além disso, optamos por utilizar uma classe absstrada no lugar de interface
para relacionar melhor o prototype com outros designs de projeto, vale resaltar que foi realizado uma cópia profunda pra garantir
que o objeto clonado nao compartilhe referências com o protótipo, sendo assim o custo para clonagem se iguala ao custo de criar uma 
nova instância.
</div>


### Versões Anteriores

<details>
<summary>Visualizar versão 1.0</summary>

### Versão 1.0

<!-- Aqui documente as mudanças de uma versão para a outra -->

A Figura 4 apresenta a versão 1.0 do UML do prototype.

<center>
Figura 4 - Prototype versão 1

![v1](https://raw.githubusercontent.com/UnBArqDsw2024-2/2024.2_G1_Logic_Thinkering_Entrega_03/refs/heads/main/assets/prototype.png)

<b>Fonte:</b> Reis, Carvalho, Sandes, 2025.
</center>

</details>

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

| Versão | Data da alteração |              Alteração               |                                                                         Responsável                                                                          | Revisor | Data de revisão |
|:------:|:-----------------:|:------------------------------------:|:------------------------------------------------------------------------------------------------------------------------------------------------------------:| :-----: | :-------------: |
|  1.0   |       28/12       |         Criação do documento         |  [Eduardo Sandes](https://github.com/DiceRunner714), [Gabriela Lemos](https://github.com/heylisten64), [João Antonio G. Carvalho](https://github.com/i-JSS)  |         |                 |
|  2.0   |       04/01       | Adiciona segunda versão do prototype | [Eduardo Sandes](https://github.com/DiceRunner714), [Gabriela Lemos](https://github.com/heylisten64), [João Antonio G. Carvalho](https://github.com/i-JSS), [Cássio Sousa dos Reis](https://github.com/csreis72)  |         |                 |

</div>

## Controle de Revisão

|                        Revisor(es)                        | O que foi realizado |
| :-------------------------------------------------------: |:-------------------:|
| |                     |