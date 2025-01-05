# GoFs Estruturais

## Introdução 
<!--  
- **Apresente o tema do projeto ou estudo;**
- **Busque trazer referências no decorrer do texto;**
- Destaque a relevância do diagrama ou abordagem para a área de aplicação.
- Mencione brevemente os principais aspectos que serão abordados no documento.
-->

<div align="justify">
&emsp;&emsp;
Os padrões de projeto estruturais (GoFs Estruturais) são amplamente reconhecidos por sua contribuição na organização e composição de classes e objetos em sistemas de software. Eles são projetados para facilitar a construção de estruturas mais flexíveis e reutilizáveis, promovendo boas práticas de design. Entre os padrões estruturais, destacam-se Adapter, Bridge, Composite, Decorator, Façade, Flyweight e Proxy, cada um com aplicações específicas para resolver desafios recorrentes no desenvolvimento de software.
</div>

## Objetivo
<!--  
- **Declare o que se pretende alcançar com o diagrama em projetos no geral; Busque referenciar!**
- **Declare o que se pretende alcançar com o diagrama para equipe neste contexto;**
- **Destaque os resultados esperados, como soluções para problemas, melhorias no entendimento ou suporte à tomada de decisões.**
-->

<div align="justify">
&emsp;&emsp;
O principal objetivo é facilitar a composição e organização de classes e objetos no sistema. Já que esse padrões buscam abstrair detalhes de implementação, promovendo a flexibilidade e a reutilização de componentes. Especificamente, esses padrões visam resolver problemas relacionados à integração de sistemas heterogêneos, redução de acoplamento entre módulos, e simplificação de estruturas complexas.
</div>

## Metodologia
<!--  
- **Explique o processo utilizado para desenvolver o trabalho. COMO foi feito?**
- **Descreva as ferramentas, técnicas ou referências utilizadas na construção do diagrama ou solução. Se houver alguma ferramenta específica determinada pela professora, a sugestão é usá-la sendo em qualquer etapa do processo. Podem começar com uma ferramenta que já são familiarizados e depois explorar outras ferramentas.**
- Se desejarem, podem citar os desafios encontrados seguindo a metodologia, propostas de melhoria, etc.
-->

<div align="justify">

&emsp;&emsp;
Inicialmente, dividimos as funcionalidades do projeto em quatro categorias. Cada subgrupo foi designado a uma funcionalidade proporcional ao seu nível de dificuldade, de modo a manter um equilíbrio no trabalho entre as equipes. Os subgrupos tiveram a liberdade de escolher e implementar o padrão de projeto (GoF) estrutural que considerassem mais adequado. A seguir, apresentamos a tabela com os integrantes de cada subgrupo, suas respectivas responsabilidades e os padrões de projeto utilizados:
</div>

<div style="margin: 0 auto; width: fit-content;">

| Grupo | Alunos                                                      | Responsabilidade           | GOF                                  |
|-------|-------------------------------------------------------------|----------------------------|--------------------------------------|
| 1     | André Silva, Artur Bartz, Samara Santos e Thomas Alves      | Portas lógica e antena     | [Decorator](/Projeto/Decorador.md)   |
| 2     | Carlos Rodrigues, Danilo Melo e Patrícia Silva              | Livro Guia e Minimapa      |                                      |
| 3     | Vinicius Santos, Sunamita Santos                            | Bloco clocker e mineradora |                                      |
| 4     | Cássio Reis, Eduardo Sandes, Gabriela Lemos e João Carvalho | Itens (todos os itens)     | [Bridge](/Projeto/BridgeMaterial.md) |

</div>


## Conclusão
<!--  
-   **Resuma os pontos principais do trabalho.**
-   **Avalie se os objetivos foram alcançados e o impacto do trabalho.**
-   **Apresente perspectivas para melhorias ou trabalhos futuros.**
-->

<div align="justify">
&emsp;&emsp;
A aplicação dos padrões estruturais do GoF é essencial para o sucesso do desenvolvimento do projeto. Esses padrões permitiram a criação de soluções robustas e reutilizáveis, alinhadas à arquitetura modular e extensível do jogo. Além disso, facilitaram a integração das funcionalidades ao sistema existente, garantindo uma experiência coesa e fluida para os jogadores.

&emsp;&emsp;
Com a implementação dos padrões, foi possível otimizar o uso de recursos e reduzir a complexidade do sistema, especialmente ao lidar com a combinação de novos elementos, como portas lógicas e fios condutores, com a mecânica de redstone. Esse processo demonstrou a importância dos padrões estruturais no desenvolvimento de software complexo, evidenciando sua aplicabilidade mesmo em contextos de modificação de jogos.
</div>

## Bibliografia 

<!-- - **Altere!**-->

> [1] SERRANO, Milene. Videoaula: [09a - Vídeo-Aula - DSW - GoFs - Estruturais](https://unbbr-my.sharepoint.com/:v:/g/personal/mileneserrano_unb_br/EdRWnnpbK8BJqcsgzvh1HRUBFjYsL1ncotuK486gTMhePA?e=t1Qd66). [Online]. Disponível em: [https://unbbr-my.sharepoint.com/:v:/g/personal/mileneserrano_unb_br/EdRWnnpbK8BJqcsgzvh1HRUBFjYsL1ncotuK486gTMhePA?e=t1Qd66](https://unbbr-my.sharepoint.com/:v:/g/personal/mileneserrano_unb_br/EdRWnnpbK8BJqcsgzvh1HRUBFjYsL1ncotuK486gTMhePA?e=t1Qd66) . Acesso em: 11 dez. 2024.

> [2] SERRANO, Milene. Videoaula: [09b - Vídeo-Aula - DSW - GoFs - Estruturais - Adapter](https://unbbr-my.sharepoint.com/:v:/g/personal/mileneserrano_unb_br/EdaXbzc0l1FDl8t6sMaG5CMBeMtYDb-Y5LdM3FGFrQX2mw?e=GgHFJT). [Online]. Disponível em: [https://unbbr-my.sharepoint.com/:v:/g/personal/mileneserrano_unb_br/EdaXbzc0l1FDl8t6sMaG5CMBeMtYDb-Y5LdM3FGFrQX2mw?e=GgHFJT](https://unbbr-my.sharepoint.com/:v:/g/personal/mileneserrano_unb_br/EdaXbzc0l1FDl8t6sMaG5CMBeMtYDb-Y5LdM3FGFrQX2mw?e=GgHFJT) . Acesso em: 11 dez. 2024.

> [3] SERRANO, Milene. Videoaula: [09c - Vídeo-Aula - DSW - GoFs - Estruturais - Composite](https://unbbr-my.sharepoint.com/:v:/g/personal/mileneserrano_unb_br/ETbYrohUuU9DnWkIPgxjH2IBzbWEbavD-Fyk1ZWL8pK-lA?e=pmyF2R). [Online]. Disponível em: [https://unbbr-my.sharepoint.com/:v:/g/personal/mileneserrano_unb_br/ETbYrohUuU9DnWkIPgxjH2IBzbWEbavD-Fyk1ZWL8pK-lA?e=pmyF2R](https://unbbr-my.sharepoint.com/:v:/g/personal/mileneserrano_unb_br/ETbYrohUuU9DnWkIPgxjH2IBzbWEbavD-Fyk1ZWL8pK-lA?e=pmyF2R) . Acesso em: 11 dez. 2024.

> [4] SERRANO, Milene. Videoaula: [09d - Vídeo-Aula - DSW - GoFs - Estruturais - Demais Padrões - Visão Rápida](https://unbbr-my.sharepoint.com/:v:/g/personal/mileneserrano_unb_br/EZzj0BVtpTxMnBCtiUlE0r8BD7Tw_XOeK1J8Jn__qgod0A?e=GQQXjZ). [Online]. Disponível em: [https://unbbr-my.sharepoint.com/:v:/g/personal/mileneserrano_unb_br/EZzj0BVtpTxMnBCtiUlE0r8BD7Tw_XOeK1J8Jn__qgod0A?e=GQQXjZ](https://unbbr-my.sharepoint.com/:v:/g/personal/mileneserrano_unb_br/EZzj0BVtpTxMnBCtiUlE0r8BD7Tw_XOeK1J8Jn__qgod0A?e=GQQXjZ) . Acesso em: 11 dez. 2024.

> [5] SERRANO, Milene. Videoaula: [09e - Vídeo-Aula - DSW - GoFs - Estruturais - Flyweight](https://unbbr-my.sharepoint.com/:v:/g/personal/mileneserrano_unb_br/EXfA21HQv_FCoSe8WQyJVRsBJehkACgDoOoG7VMvxhzkiw?e=MybMXx). [Online]. Disponível em: [https://unbbr-my.sharepoint.com/:v:/g/personal/mileneserrano_unb_br/EXfA21HQv_FCoSe8WQyJVRsBJehkACgDoOoG7VMvxhzkiw?e=MybMXx](https://unbbr-my.sharepoint.com/:v:/g/personal/mileneserrano_unb_br/EXfA21HQv_FCoSe8WQyJVRsBJehkACgDoOoG7VMvxhzkiw?e=MybMXx) . Acesso em: 11 dez. 2024.


<center>

## Participantes

</center>

<!-- de preferência: em ordem alfabética, seguindo o exemplo: -->

<div style="margin: 0 auto; width: fit-content;">

| Matrícula | Aluno                                 | Git                                                           |
| --------- | ------------------------------------- |---------------------------------------------------------------|
| 221007813 | André Emanuel Bispo da Silva          | [Hunter104](https://github.com/Hunter104)                     |
| 221007869 | Artur Henrique Holz Bartz             | [H0lzz](https://github.com/H0lzz)                             |
| 221931265 | Carlos Eduardo Rodrigues              | [carlos-kadu](https://github.com/carlos-kadu)                 |
| 221021886 | Cássio Sousa dos Reis                 | [csreis72](https://github.com/csreis72)                       |
| 221031149 | Danilo César Tertuliano Melo          | [DaniloCTM](https://github.com/DaniloCTM)                     |
| 221008024 | Eduardo Matheus dos Santos Sandes     | [DiceRunner714](https://github.com/DiceRunner714)             |
| 170010872 | Gabriela de Oliveira Lemos            | [heylisten64](https://github.com/heylisten64)                 |
| 221008150 | João Antonio Ginuino Carvalho         | [i-JSS](https://github.com/i-JSS)                             |
| 221037993 | Patrícia Helena Macedo da Silva       | [patyhelenaa](https://github.com/patyhelenaa)                 |
| 221008445 | Samara Letícia Alves dos Santos       | [samarawwleticia](https://github.com/samarawwleticia)         |
| 221008697 | Sunamita Vitória Rodrigues dos Santos | [sunamit](https://github.com/sunamit)                         |
| 211062526 | Thomas Queiroz Souza Alves            | [thmasq](https://github.com/thmasq)                           |
| 202017263 | Vinicius de Oliveira Santos           | [ViniciussdeOliveira](https://github.com/ViniciussdeOliveira) |

</div>

---

<center>

## Histórico de Versão

</center>

<!-- Lembre de alterar a data -->
<!-- É PRA POR O NOME, NÃO O USER DO GITHUB -->

<div style="margin: 0 auto; width: fit-content;">

| Versão | Data da alteração |            Alteração            |                  Responsável                  |                      Revisor                       | Data de revisão |
| :----: | :---------------: | :-----------------------------: | :-------------------------------------------: | :------------------------------------------------: |:---------------:|
|  1.0   |       11/12       |      Criação do documento       |    [Artur Barlz](https://github.com/H0lzz)    | [João Antonio G. Carvalho](https://github.com/i-JSS)                                                    | 05/01      | 

</div>

## Controle de Revisão

| Revisor(es) | O que foi realizado  |
|:-----------:|:--------------------:|
|      [João Antonio G. Carvalho](https://github.com/i-JSS)        | Atualiza metodologia |
