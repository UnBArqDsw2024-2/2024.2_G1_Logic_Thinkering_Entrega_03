# Diagrama de Classes dos Itens

## Introdução

<!--
- **Apresente o tema do projeto ou estudo;**
- **Busque trazer referências no decorrer do texto;**
- Destaque a relevância do diagrama ou abordagem para a área de aplicação.
- Mencione brevemente os principais aspectos que serão abordados no documento.
-->

<div align="justify">&emsp;&emsp;
O uso de diagramas de classes é fundamental em engenharia de software, pois promove clareza na modelagem do sistema, garantindo rastreabilidade e organização dos requisitos. Ao ilustrar a estrutura de classes, destacam-se relações, atributos e métodos que permitem maior entendimento da arquitetura proposta, favorecendo a implementação colaborativa e eficiente.
No desenvolvimento de mods para Minecraft, o uso de diagramas de classes é fundamental para estruturar e organizar os componentes de forma clara e eficiente. Este projeto, voltado para a criação de itens personalizados, como armaduras e ferramentas, utiliza padrões de design de software, como o padrão Prototype, para promover flexibilidade e reutilização de código. O diagrama apresentado detalha a estrutura e as interações entre os diversos elementos do sistema, facilitando a compreensão e manutenção do mod. 
</div>

<div align="justify">&emsp;&emsp;
Durante o desenvolvimento do mod "Logic Thinkering" para Minecraft, o uso de diagramas de classes é fundamental para estruturar e organizar os componentes de forma clara e eficiente. No escopo dos itens, como armaduras e ferramentas, utilizam-se padrões de design de software, como o padrão criacional Prototype, usado para evitar complexidade desnecessária quando a criação repetitiva de objetos é custosa. Além disso, o diagrama apresentado reflete a estrutura do mod após várias refatorações feitas durante a implementação, alinhando-se às mudanças realizadas no código. Essa abordagem garantiu que as relações entre classes, os atributos e os métodos fossem modelados com precisão e clareza, atendendo às demandas específicas da API Fabric e garantindo compatibilidade com a lógica do Minecraft. Como resultado, o diagrama de classes não apenas documenta a solução, mas também atua como um guia prático para futuras expansões e/ou ajustes no mod.
</div>

## Objetivo

<!--
- **Declare o que se pretende alcançar com o diagrama em projetos no geral; Busque referenciar!**
- **Declare o que se pretende alcançar com o diagrama para equipe neste contexto;**
- **Destaque os resultados esperados, como soluções para problemas, melhorias no entendimento ou suporte à tomada de decisões.**
-->

<div align="justify">&emsp;&emsp;
O diagrama de classes visa facilitar a compreensão e implementação das funcionalidades do mod, oferecendo uma visão estruturada dos blocos, itens e sistemas introduzidos. Para a equipe, o objetivo é mitigar ambiguidades durante o desenvolvimento, evidenciar dependências entre classes e suportar a expansão futura do sistema. Especificamente neste contexto, espera-se que o diagrama funcione como uma referência centralizada para a modelagem do código, promovendo padronização e integração das novas funcionalidades ao código original do jogo.
</div>

## Metodologia

<!--
- **Explique o processo utilizado para desenvolver o trabalho. COMO foi feito?**
- **Descreva as ferramentas, técnicas ou referências utilizadas na construção do diagrama ou solução. Se houver alguma ferramenta específica determinada pela professora, a sugestão é usá-la sendo em qualquer etapa do processo. Podem começar com uma ferramenta que já são familiarizados e depois explorar outras ferramentas.**
- Se desejarem, podem citar os desafios encontrados seguindo a metodologia, propostas de melhoria, etc.
-->

<div align="justify">&emsp;&emsp;
A construção do diagrama seguiu os princípios de orientação a objetos, alinhando-se à arquitetura do Minecraft na versão Fabric 1.21.X, toda
a documentação referente a essa versão está disponível em <a href="https://docs.fabricmc.net/">https://docs.fabricmc.net/</a>, além disso
funcionalidades que não estavam de acordo com a documentação foram implementadas através de engenharia reversa do código fonte do minecraft
disponível em <a href="https://github.com/FabricMC/fabric-example-mod">https://github.com/FabricMC/fabric-example-mod</a>.
</div>
<div align="justify">&emsp;&emsp;
Sendo assim a base foi estruturada considerando as classes principais do jogo original, que foram estendidas para incluir novos comportamentos. Ferramentas de modelagem UML foram utilizadas para criar o diagrama, permitindo representação visual das relações, como heranças e associações. O processo foi iterativo, revisando e validando os requisitos junto à equipe, e ajustando a modelagem para garantir consistência com os objetivos do projeto.
</div>
<div align="justify">&emsp;&emsp;
Após a inserção de cada padrão de projeto no diagrama original, foi gerado um UML correspondente para guiar a implementação em código, que quando finalizada, serviu de guia para a refatoração do diagrama anterior, esse processo se repetiu durante toda a implementação relativa aos itens adicionados.
</div>

## Resultados

<!--
- **Apresente o produto final, como o diagrama ou solução desenvolvida.**
- **Desenvolva ao menos um parágrafo referenciando a figura**
- **Adicione "Figura 1 - Título da Figura/Quadro/Tabela" acima e "Fonte: " abaixo dela**
- Destaque os pontos principais ou insights obtidos durante o processo.
- **APRESENTE AS VERSÕES DO DIAGRAMA!! Podem usar o formato abaixo para poluir menos a página**
-->

<center>
Figura 1 - Diagrama de Classes dos itens

![Modelagem - Factory Method](https://raw.githubusercontent.com/UnBArqDsw2024-2/2024.2_G1_Logic_Thinkering_Entrega_03/refs/heads/main/assets/classitem.png)

 <b>Fonte:</b> Sandes, Lemos, Carvalho, 2025.
</center>


<!--
-   **Resuma os pontos principais do trabalho.**
-   **Avalie se os objetivos foram alcançados e o impacto do trabalho.**
-   **Apresente perspectivas para melhorias ou trabalhos futuros.**
-->

## Bibliografia

> [1] SERRANO, Milene. Videoaula: [05b - VideoAula - DSW - Modelagem - Diagrama de Classe](https://unbbr-my.sharepoint.com/:v:/g/personal/mileneserrano_unb_br/EZPNTlqmVjNOqXT1Bm2uUrcBsfMyv68qkyEL_rFMNSEy0g?e=lhFYa3). [Online]. Disponível em: [https://unbbr-my.sharepoint.com/:v:/g/personal/mileneserrano_unb_br/EZPNTlqmVjNOqXT1Bm2uUrcBsfMyv68qkyEL_rFMNSEy0g?e=lhFYa3](https://unbbr-my.sharepoint.com/:v:/g/personal/mileneserrano_unb_br/EZPNTlqmVjNOqXT1Bm2uUrcBsfMyv68qkyEL_rFMNSEy0g?e=lhFYa3) . Acesso em: 21 nov. 2024.

> [2] Reis, fabio. Videoaula: [Curso de UML O que é um Diagrama de Classes](https://youtu.be/JQSsqMCVi1k) . [Online]. Disponível em: <https://youtu.be/JQSsqMCVi1k>. Acesso em: 24 nov. 2024.

> [3] Reis, fabio. Videoaula: [Curso de UML - Diagrama de Classes - Relacionamentos](https://youtu.be/IJtQWLnHvcQ) . [Online]. Disponível em: <https://youtu.be/IJtQWLnHvcQ>. Acesso em: 24 nov. 2024.

> [4] SERRANO, Milene. Videoaula: [08d - Vídeo-Aula - DSW - GoFs - Criacionais - Demais Padrões - Visão Rápida](https://unbbr-my.sharepoint.com/:v:/g/personal/mileneserrano_unb_br/EdG2MN8pFrJOpSMnzcCDJFkB2FCnvQlUOuGUs6ehjZYv6A?e=yl3dpY). [Online]. Disponível em: [https://unbbr-my.sharepoint.com/:v:/g/personal/mileneserrano_unb_br/EdG2MN8pFrJOpSMnzcCDJFkB2FCnvQlUOuGUs6ehjZYv6A?e=yl3dpY](https://unbbr-my.sharepoint.com/:v:/g/personal/mileneserrano_unb_br/EdG2MN8pFrJOpSMnzcCDJFkB2FCnvQlUOuGUs6ehjZYv6A?e=yl3dpY) . Acesso em: 28 dez. 2024.

> [5] MIRANDA, Ótavio. Videoaula: [Prototype Teoria - Padrões de Projeto - Parte 8/45](https://youtu.be/Z-_smcjkdwM?si=acuF2pyFHgE5ewok). [Online]. Disponível em: [https://youtu.be/Z-_smcjkdwM?si=acuF2pyFHgE5ewok](https://youtu.be/Z-_smcjkdwM?si=acuF2pyFHgE5ewok) . Acesso em: 27 dez. 2024.

> [6] BANAS, derek. Videoaula: [Prototype Design Pattern Tutorial](https://youtu.be/AFbZhRL0Uz8?si=Ummno3Obz_fIImEX). [Online]. Disponível em: [https://youtu.be/AFbZhRL0Uz8?si=Ummno3Obz_fIImEX](https://youtu.be/AFbZhRL0Uz8?si=Ummno3Obz_fIImEX) . Acesso em: 27 dez. 2024.

> [7] SERRANO, Milene. Videoaula: [09d - Vídeo-Aula - DSW - GoFs - Estruturais - Demais Padrões - Visão Rápida](https://unbbr-my.sharepoint.com/:v:/g/personal/mileneserrano_unb_br/EZzj0BVtpTxMnBCtiUlE0r8BD7Tw_XOeK1J8Jn__qgod0A?e=GQQXjZ). [Online]. Disponível em: [https://unbbr-my.sharepoint.com/:v:/g/personal/mileneserrano_unb_br/EZzj0BVtpTxMnBCtiUlE0r8BD7Tw_XOeK1J8Jn__qgod0A?e=GQQXjZ](https://unbbr-my.sharepoint.com/:v:/g/personal/mileneserrano_unb_br/EZzj0BVtpTxMnBCtiUlE0r8BD7Tw_XOeK1J8Jn__qgod0A?e=GQQXjZ) . Acesso em: 04 jan. 2025.

> [8] SERRANO, Milene. Videoaula: [10b - Vídeo-Aula - DSW - GoFs - Comportamentais - Strategy](https://unbbr-my.sharepoint.com/:v:/g/personal/mileneserrano_unb_br/EUZop92j5WlIvdppY_A5wGcBt5xkL_HFpF8USoR6BJh4lw?e=JRjZ7B). [Online]. Disponível em: [https://unbbr-my.sharepoint.com/:v:/g/personal/mileneserrano_unb_br/EUZop92j5WlIvdppY_A5wGcBt5xkL_HFpF8USoR6BJh4lw?e=JRjZ7B](https://unbbr-my.sharepoint.com/:v:/g/personal/mileneserrano_unb_br/EUZop92j5WlIvdppY_A5wGcBt5xkL_HFpF8USoR6BJh4lw?e=JRjZ7B) . Acesso em: 02 jan. 2025.



<center>

## Participantes

</center>

<!-- de preferência: em ordem alfabética, seguindo o exemplo: -->

<div style="margin: 0 auto; width: fit-content;">

| Matrícula | Aluno                             | Git                                               |
| --------- | --------------------------------- | ------------------------------------------------- |
| 221008024 | Eduardo Matheus dos Santos Sandes | [DiceRunner714](https://github.com/DiceRunner714) |
| 170010872 | Gabriela de Oliveira Lemos        | [heylisten64](https://github.com/heylisten64)     |
| 221008150 | João Antonio Ginuino Carvalho     | [i-JSS](https://github.com/i-JSS)                 |
|           |                                   |                                                   |

</div>

---

<center>

## Histórico de Versão

</center>

<!-- Lembre de alterar a data -->
<!-- É PRA POR O NOME, NÃO O USER DO GITHUB -->

<div style="margin: 0 auto; width: fit-content;">

| Versão | Data da alteração |      Alteração       |                                                                        Responsável                                                                         | Revisor | Data de revisão |
| :----: | :---------------: | :------------------: | :--------------------------------------------------------------------------------------------------------------------------------------------------------: | :-----: | :-------------: |
|  1.0   |       05/01/2025       | Criação do documento | [Eduardo Sandes](https://github.com/DiceRunner714), [Gabriela Lemos](https://github.com/heylisten64), [João Antonio G. Carvalho](https://github.com/i-JSS) |         |                 |
|        |                   |                      |                                                                                                                                                            |         |                 |

</div>

## Controle de Revisão

| Revisor(es) | O que foi realizado |
| :---------: | :-----------------: |
|             |                     |
|             |                     |