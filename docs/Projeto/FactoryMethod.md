# Factory Method

## Introdução

<div align="justify">&emsp;&emsp;
O Factory Method é um padrão de design apresentado por Gamma et al. (1994), que define uma interface para criar objetos em uma superclasse, permitindo que subclasses decidam qual classe concreta instanciar. Esse padrão é utilizado para delegar a responsabilidade de criação de objetos para subclasses, evitando a dependência direta de classes concretas.
</div>

## Objetivo

<div align="justify">&emsp;&emsp;
O objetivo do Factory Method é delegar a criação de objetos a subclasses, permitindo que o código principal trabalhe com abstrações em vez de classes concretas. Essa abordagem reduz o acoplamento, melhora a reutilização e facilita a extensão do sistema (Gamma et al., 1994).
</div>

## Metodologia
<div align="justify">&emsp;&emsp; Durante a implementação do ClockEnergy (bloco de clock), a principal dificuldade foi gerenciar a variação no comportamento do bloco com diferentes tipos de geradores de pulso. Criar e gerenciar instâncias para cada tipo de comportamento de geração de pulso resultaria em duplicação de código e complexidade crescente, especialmente à medida que novos tipos de geradores pudessem ser adicionados. Além disso, a manutenção do código se tornaria difícil, pois qualquer modificação na lógica de alternância de estado ou agendamento de pulsos exigiria mudanças em várias partes do sistema.</div> <div align="justify">&emsp;&emsp; Para solucionar esse problema, utilizamos o padrão de projeto Factory Method, que permite criar diferentes tipos de geradores de pulso de forma centralizada e flexível. Esse padrão separa a lógica de criação do gerador da lógica principal de alternância de estado do ClockEnergy, permitindo que o tipo de gerador seja facilmente alterado ou expandido sem impactar o restante do sistema.</div>

## Resultados

### Interface: PulseGenerator

```java
package com.logic_thinkering.block.clock;

public interface PulseGenerator {
    public int generatePulse();
}
```

### Classe: RegularPulseGenerator

```java
package com.logic_thinkering.block.clock;

public class RegularPulseGenerator implements PulseGenerator {
    private final int interval;

    public RegularPulseGenerator(int interval) {
        this.interval = interval;
    }

    @Override
    public int generatePulse() {
        return interval;
    }
}
```

### Classe: RandomPulseGenerator

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

### Classe Abstrata: ClockEnergy

```java
package com.logic_thinkering.block.clock;

import net.minecraft.block.AbstractBlock;
import net.minecraft.block.Block;
import net.minecraft.block.BlockState;
import net.minecraft.entity.LivingEntity;
import net.minecraft.item.ItemStack;
import net.minecraft.server.world.ServerWorld;
import net.minecraft.state.StateManager;
import net.minecraft.state.property.BooleanProperty;
import net.minecraft.util.math.BlockPos;
import net.minecraft.util.math.Direction;
import net.minecraft.util.math.random.Random;
import net.minecraft.world.BlockView;
import net.minecraft.world.World;

public abstract class ClockEnergy extends Block {
    public static final BooleanProperty ACTIVE = BooleanProperty.of("active");

    public ClockEnergy(AbstractBlock.Settings settings) {
        super(settings);
        // Define o estado inicial do bloco como "inativo"
        this.setDefaultState(this.stateManager.getDefaultState().with(ACTIVE, false));
    }

    protected abstract PulseGenerator createPulseGenerator();

    @Override
    protected void appendProperties(StateManager.Builder<Block, BlockState> builder) {
        builder.add(ACTIVE); // Adiciona a propriedade "active" ao bloco
    }

    @Override
    public boolean emitsRedstonePower(BlockState state) {
        return state.get(ACTIVE); // Só emite sinal se estiver "ativo"
    }

    @Override
    public int getWeakRedstonePower(BlockState state, BlockView world, BlockPos pos, Direction direction) {
        return state.get(ACTIVE) ? 15 : 0; // Emite potência máxima quando "ativo", senão 0
    }

    @Override
    public void scheduledTick(BlockState state, ServerWorld world, BlockPos pos, Random random) {
        // Alterna o estado do bloco (ativo/inativo)
        boolean isActive = state.get(ACTIVE);
        world.setBlockState(pos, state.with(ACTIVE, !isActive), Block.NOTIFY_ALL);

        PulseGenerator pulseGenerator = createPulseGenerator();
        int nextInterval = pulseGenerator.generatePulse();

        // Solicita uma nova atualização (20 ticks = 1 segundo)
        world.scheduleBlockTick(pos, this, nextInterval);
    }

    @Override
    public void onPlaced(World world, BlockPos pos, BlockState state, LivingEntity placer, ItemStack itemStack) {
        // Quando o bloco é colocado, inicia o ciclo de atualizações
        if (!world.isClient) {
            world.scheduleBlockTick(pos, this, 100); // Primeira atualização 20 ticks = 1 segundo
        }
    }
}
```

### Classe: RegularClockEnergy

```java
package com.logic_thinkering.block.clock;

public class RegularClockEnergy extends ClockEnergy {

    public RegularClockEnergy(Settings settings) {
        super(settings);
    }

    @Override
    protected PulseGenerator createPulseGenerator() {
        return new RegularPulseGenerator(20);
    }
}
```

### Classe: RandomClockEnergy

```java
package com.logic_thinkering.block.clock;

public class RandomClockEnergy extends ClockEnergy {

    public RandomClockEnergy(Settings settings) {
        super(settings);
    }

    @Override
    protected PulseGenerator createPulseGenerator() {
        return new RandomPulseGenerator();
    }
}
```

### Modelagem

<center>
Figura 1 - Factory Method

![Modelagem - Factory Method](https://raw.githubusercontent.com/UnBArqDsw2024-2/2024.2_G1_Logic_Thinkering_Entrega_03/refs/heads/main/assets/factorymethod.png)

 <b>Fonte:</b> Reis, Santos, 2025.
</center>

<div align="justify">&emsp;&emsp;
A implementação do padrão Factory Method no ClockEnergy permitiu uma criação flexível e desacoplada de diferentes tipos de geradores de pulso, como regulares ou aleatórios. Isso centraliza a lógica de alternância de estado e reagendamento de pulsos na classe ClockEnergy, enquanto delega a responsabilidade de definir o intervalo de pulso para subclasses específicas. Foi possível observar que o uso do Factory Method reduz a duplicação de código, facilita a manutenção e expansão do sistema,permitindo a adição de novos comportamentos sem modificar a classe principal.
</div>

<!-- no caso da entrega 01-->

### Requisitos Elicitados
<center> <b>Tabela 01</b> - Requisitos Elicitados no artefato <b>Brainstorming</b></center>

| ID    | Descrição                                                                                                                         |
| ----- | --------------------------------------------------------------------------------------------------------------------------------- |
| RF17  | Permitir ao jogador construir circuitos lógicos sem a necessidade de componentes externos.                                        |
| RNF02 | As novas mecânicas e blocos não devem impactar significativamente o desempenho do jogo, mesmo em servidores com muitos jogadores. |
| RNF05 | Utilizar imagens, ícones e texturas objetivas.                                                                                    |

<!-- no caso da entrega 02-->

### Requisitos Modelados
<center> <b>Tabela 01</b> - Requisitos Modelados no artefato <b>Diagrama de Classes</b></center>

| ID    | Descrição                                                                                                                        | Origem                                                                                                                      |
| ----- | -------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| RF17  | Permitir ao jogador construir circuitos lógicos sem a necessidade de componentes externos (timer, clock…)                        | [Brainstorming](https://unbarqdsw2024-2.github.io/2024.2_G1_Logic_Thinkering_Entrega_01/#/Base/Design_Sprint/brainstorming) |
| RNF02 | As novas mecânicas e blocos não devem impactar significativamente o desempenho do jogo, mesmo em servidores com muitos jogadores | [Brainstorming](https://unbarqdsw2024-2.github.io/2024.2_G1_Logic_Thinkering_Entrega_01/#/Base/Design_Sprint/brainstorming) |
| RNF05 | Utilizar imagens, ícones e texturas objetivas                                                                                    | [Brainstorming](https://unbarqdsw2024-2.github.io/2024.2_G1_Logic_Thinkering_Entrega_01/#/Base/Design_Sprint/brainstorming) |
|       |                                                                                                                                  |                                                                                                                             |

<!-- atençao de respeitas os IDS de cada requisito!! o documento backward from pode ajudar-->

## Conclusão

<!--
-   **Resuma os pontos principais do trabalho.**
-   **Avalie se os objetivos foram alcançados e o impacto do trabalho.**
-   **Apresente perspectivas para melhorias ou trabalhos futuros.**
-->

<div align="justify">&emsp;&emsp;
Em suma, a aplicação do Factory Method no ClockEnergy aprimora a flexibilidade, a modularidade e a organização do código, permitindo que novos tipos de geradores de pulso sejam facilmente adicionados sem alterar a lógica central do bloco, garantindo um design robusto e preparado para futuras melhorias.
</div>


## Bibliografia

<!-- - **Altere!**-->

> [1] SERRANO, Milene. Videoaula: [08b - Vídeo-Aula - DSW - GoFs - Criacionais - Factory Method](https://unbbr-my.sharepoint.com/:v:/g/personal/mileneserrano_unb_br/EaEyZ8_0t2hBqxA18xigI3oBSHrxG6r4evdrZtsrDZelEQ?e=RlmE1C). [Online]. Disponível em: https://unbbr-my.sharepoint.com/:v:/g/personal/mileneserrano_unb_br/EaEyZ8_0t2hBqxA18xigI3oBSHrxG6r4evdrZtsrDZelEQ?e=RlmE1C. Acesso em: 05 jan. 2025.

> [2] GAMMA, Erich; HELM, Richard; JOHNSON, Ralph; VLISSIDES, John. Design Patterns: Elements of Reusable Object-Oriented Software. 1. ed. Addison-Wesley, 1994.

<center>

## Participantes

</center>

<!-- de preferência: em ordem alfabética, seguindo o exemplo: -->

<div style="margin: 0 auto; width: fit-content;">

| Matrícula | Aluno                             | Git                                               |
| --------- | --------------------------------- | ------------------------------------------------- |
| 221021886 | Cássio Sousa dos Reis | [csreis72](https://github.com/DiceRunner714) |
| 202017263 | Vinicius de Oliveira Santos     | [ViniciussdeOliveira](https://github.com/ViniciussdeOliveira)     |

</div>

---

<center>

## Histórico de Versão

</center>

<!-- Lembre de alterar a data -->
<!-- É PRA POR O NOME, NÃO O USER DO GITHUB -->

<div style="margin: 0 auto; width: fit-content;">

| Versão | Data da alteração |      Alteração       |                Responsável                 |                      Revisor                       | Data de revisão |
| :----: | :---------------: | :------------------: | :----------------------------------------: | :------------------------------------------------: | :-------------: |
|  1.0   |       05/01/2025       | Criação do documento | [Cássio Reis](https://github.com/csreis72) | [Eduardo Sandes](https://github.com/DiceRunner714) |   05/01/2025              |

</div>

## Controle de Revisão

|                    Revisor(es)                     |                     O que foi realizado                      |
| :------------------------------------------------: | :----------------------------------------------------------: |
| [Eduardo Sandes](https://github.com/DiceRunner714) | Ano foi inserido nas datas e os requisitos foram adicionados |
|                                                    |                                                              |