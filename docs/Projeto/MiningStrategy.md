# Strategy - Robô de mineração

## Introdução
<div align="justify">&emsp;&emsp;
Este documento apresenta o padrão de projeto Strategy do robô de mineração, com o objetivo de modularizar e definir métodos específicos de mineração de forma flexível.
</div>

## Objetivo
<div align="justify">&emsp;&emsp;
O objetivo do strategy é permitir que um conjunto de algoritmos seja definido e encapsulado separadamente de forma que a escolha de qual será usado possa ser feita em tempo de execução. Resumidamente, o strategy promove a separação entre o comportamento de uma classe e a lógica que a implementa.
</div>

## Metodologia
<div align="justify">&emsp;&emsp;
No processo de desenvolvimento do robô de mineração, percebemos a necessidade de adaptar suas operações a diferentes contextos e demandas. Para enfrentar esse desafio, implementamos o padrão Strategy, que separa cada comportamento de mineração em classes especializadas. Essa abordagem facilita a modularidade do código, melhora sua manutenção e permite a troca dinâmica de estratégias conforme os requisitos evoluem.
</div>

## Resultados

<!--
- **Apresente o produto final, como o diagrama ou solução desenvolvida.**
- **Desenvolva ao menos um parágrafo referenciando a figura**
- **Adicione "Figura 1 - Título da Figura/Quadro/Tabela" acima e "Fonte: " abaixo dela**
- Destaque os pontos principais ou insights obtidos durante o processo.
- **APRESENTE AS VERSÕES DO DIAGRAMA!! Podem usar o formato abaixo para poluir menos a página**
-->

### Interface: MiningStrategy

```java
package com.logic_thinkering.block.miningmachine;

import net.minecraft.server.world.ServerWorld;
import net.minecraft.util.math.BlockPos;

import java.util.List;

public interface MiningStrategy {
    void mine(ServerWorld world, BlockPos pos, List<BlockPos> blocksToMine);
}
```

### Classe: RadiusMiningStrategy

```java
package com.logic_thinkering.block.miningmachine;

import net.minecraft.block.BlockState;
import net.minecraft.block.Blocks;
import net.minecraft.server.world.ServerWorld;
import net.minecraft.util.math.BlockPos;
import net.minecraft.util.math.Vec3i;

import java.util.List;

public class RadiusMiningStrategy implements MiningStrategy {
    private final int radius;

    public RadiusMiningStrategy(int radius) {
        this.radius = radius;
    }

    @Override
    public void mine(ServerWorld world, BlockPos pos, List<BlockPos> blocksToMine) {
        for (int x = -radius; x <= radius; x++) {
            for (int y = -radius; y <= radius; y++) {
                for (int z = -radius; z <= radius; z++) {
                    BlockPos targetPos = pos.add(new Vec3i(x, y, z));
                    BlockState targetState = world.getBlockState(targetPos);

                    // Verificar se não é o próprio bloco nem Bedrock
                    if (targetPos.equals(pos) || targetState.getBlock() == Blocks.BEDROCK) {
                        continue;
                    }

                    // Adicionar o bloco à lista de blocos a serem minerados
                    if (!targetState.isAir()) {
                        blocksToMine.add(targetPos);
                    }
                }
            }
        }
    }
}
```

### Classe: LineMiningStrategy

```java
package com.logic_thinkering.block.miningmachine;

import net.minecraft.block.BlockState;
import net.minecraft.block.Blocks;
import net.minecraft.server.world.ServerWorld;
import net.minecraft.util.math.BlockPos;

import java.util.List;

public class LineMiningStrategy implements MiningStrategy {

    private final int length;

    public LineMiningStrategy(int length) {
        this.length = length;
    }

    @Override
    public void mine(ServerWorld world, BlockPos pos, List<BlockPos> blocksToMine) {
        for (int i = 1; i < length; i++) {
            // A cada iteração, a posição se move para frente ao longo do eixo X
            BlockPos targetPos = pos.add(0, 0, i);

            // Minerando uma área 3x3 ao redor de targetPos, incluindo o eixo Y
            for (int x = -1; x <= 1; x++) {
                for (int y = 0; y <= 2; y++) {
                    for (int z = -1; z <= 1; z++) {
                        BlockPos areaPos = targetPos.add(x, y, z);
                        BlockState targetState = world.getBlockState(areaPos);

                        if (areaPos.equals(pos) || targetState.getBlock() == Blocks.BEDROCK) {
                            continue;
                        }

                        if (!targetState.isAir()) {
                            blocksToMine.add(areaPos);
                        }
                    }
                }
            }
        }
    }
}
```

### Modelagem
</center>
Figura 1 - Strategy

![Diagrama de Classes - Estratégia de Mineração](https://raw.githubusercontent.com/UnBArqDsw2024-2/2024.2_G1_Logic_Thinkering_Entrega_03/refs/heads/main/assets/miningstrategy.png)

 <b>Fonte:</b> Reis, Santos, 2025.
</center>

## Conclusão

<!--
-   **Resuma os pontos principais do trabalho.**
-   **Avalie se os objetivos foram alcançados e o impacto do trabalho.**
-   **Apresente perspectivas para melhorias ou trabalhos futuros.**
-->

<div align="justify">&emsp;&emsp;
A aplicação do padrão Strategy na implementação da lógica de mineração do robô de mineração provou ser uma abordagem eficaz para lidar com os diferentes tipos de estratégias de mineração que o robô pode adotar. Com a separação das lógicas de mineração em classes distintas, foi possível garantir flexibilidade na escolha e alteração das estratégias de mineração, como mineração em linha ou em área, sem comprometer a estrutura do código. Esse design também permite futuras expansões, como a adição de novas estratégias, sem a necessidade de modificar as partes centrais do sistema.
</div>

## Bibliografia

<!-- - **Altere!**-->

> [1] SERRANO, Milene. Videoaula: [10b - Vídeo-Aula - DSW - GoFs - Comportamentais - Strategy](https://unbbr-my.sharepoint.com/:v:/g/personal/mileneserrano_unb_br/EUZop92j5WlIvdppY_A5wGcBt5xkL_HFpF8USoR6BJh4lw?e=JRjZ7B). [Online]. Disponível em: https://unbbr-my.sharepoint.com/:v:/g/personal/mileneserrano_unb_br/EUZop92j5WlIvdppY_A5wGcBt5xkL_HFpF8USoR6BJh4lw?e=JRjZ7B. Acesso em: 02 jan. 2025.

<center>

## Participantes

</center>

<!-- de preferência: em ordem alfabética, seguindo o exemplo: -->

<div style="margin: 0 auto; width: fit-content;">

| Matrícula | Aluno                             | Git                                               |
| --------- | --------------------------------- | ------------------------------------------------- |
| 221021886 | Cássio Sousa dos Reis | [csreis72](https://github.com/csreis72) |
| 202017263 | Vinicius de Oliveira Santos     | [ViniciussdeOliveira](https://github.com/ViniciussdeOliveira)       |

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
|  1.0   |       04/01       | Criação do documento | [Cássio Sousa dos Reis](https://github.com/csreis72) |         |                 |

</div>

## Controle de Revisão

|                        Revisor(es)                        | O que foi realizado |
| :-------------------------------------------------------: |:-------------------:|
| |                     |