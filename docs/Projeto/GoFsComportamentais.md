# GoFs Comportamentais

## Introdução 
<!--  
- **Apresente o tema do projeto ou estudo;**
- **Busque trazer referências no decorrer do texto;**
- Destaque a relevância do diagrama ou abordagem para a área de aplicação.
- Mencione brevemente os principais aspectos que serão abordados no documento.
-->
<div align="justify">
&emsp;&emsp;
Os padrões comportamentais do GoF desempenham um papel crucial na definição de interações e responsabilidades entre os objetos em um sistema de software. No contexto do desenvolvimento do projeto, esses padrões oferecem uma base sólida para implementar funcionalidades complexas. Os elementos do mod exigem um sistema bem organizado para gerenciar o fluxo de dados, decisões lógicas e a comunicação eficiente entre componentes.

&emsp;&emsp;
O uso dos padrões comportamentais no mod busca garantir que as funcionalidades sejam implementadas de forma flexível, permitindo sua expansão futura e mantendo a jogabilidade intuitiva para os usuários. Eles auxiliam na criação de sistemas dinâmicos, nos quais as ações dos jogadores e os eventos do jogo interagem harmoniosamente com os novos elementos introduzidos.
</div>

## Objetivo
<!--  
- **Declare o que se pretende alcançar com o diagrama em projetos no geral; Busque referenciar!**
- **Declare o que se pretende alcançar com o diagrama para equipe neste contexto;**
- **Destaque os resultados esperados, como soluções para problemas, melhorias no entendimento ou suporte à tomada de decisões.**
-->

<div align="justify">&emsp;&emsp;
Os objetivos da utilização de padrões comportamentais no desenvolvimento do mod são voltados à criação de um sistema robusto e dinâmico que facilite a interação entre os novos elementos introduzidos no jogo, como portas lógicas, fios condutores e blocos de redstone. Busca-se estabelecer uma comunicação eficiente entre os componentes, promovendo decisões dinâmicas durante a jogabilidade e utilizando padrões como Strategy e State para gerenciar comportamentos variáveis. Além disso, visa-se garantir a flexibilidade e a expansão futura das funcionalidades, assegurando que novas implementações possam ser integradas sem comprometer a estrutura existente. Outro objetivo essencial é reduzir a complexidade de desenvolvimento, adotando práticas que simplifiquem o fluxo de controle e as interações entre os elementos do sistema, sempre promovendo uma experiência fluida e intuitiva para os jogadores.
</div>

## Metodologia
<!--  
- **Explique o processo utilizado para desenvolver o trabalho. COMO foi feito?**
- **Descreva as ferramentas, técnicas ou referências utilizadas na construção do diagrama ou solução. Se houver alguma ferramenta específica determinada pela professora, a sugestão é usá-la sendo em qualquer etapa do processo. Podem começar com uma ferramenta que já são familiarizados e depois explorar outras ferramentas.**
- Se desejarem, podem citar os desafios encontrados seguindo a metodologia, propostas de melhoria, etc.
-->

<div align="justify">

&emsp;&emsp;
Inicialmente, dividimos as funcionalidades do projeto em quatro categorias. Cada subgrupo foi designado a uma funcionalidade proporcional ao seu nível de dificuldade, de modo a manter um equilíbrio no trabalho entre as equipes. Os subgrupos tiveram a liberdade de escolher e implementar o padrão de projeto (GoF) comportamental que considerassem mais adequado. A seguir, apresentamos a tabela com os integrantes de cada subgrupo, suas respectivas responsabilidades e os padrões de projeto utilizados:
</div>


| Grupo | Alunos                                                      | Responsabilidade           | GOF                                                        |
|-------|-------------------------------------------------------------|----------------------------|------------------------------------------------------------|
| 1     | André Silva, Artur Bartz, Samara Santos e Thomas Alves      | Portas lógica e antena     |                                                            |
| 2     | Carlos Rodrigues, Danilo Melo e Patrícia Silva              | Livro Guia e Minimapa      |                                                            |
| 3     | Cássio Reis, Vinicius Santos, Sunamita Santos                            | Bloco clocker e mineradora | [Strategy - Robô de Mineração](/Projeto/MiningStrategy.md) |
| 4     | Eduardo Sandes, Gabriela Lemos e João Carvalho | Itens (todos os itens)     | [Strategy - Item](/Projeto/StrategyRegister.md)            |


## Conclusão
<!--  
-   **Resuma os pontos principais do trabalho.**
-   **Avalie se os objetivos foram alcançados e o impacto do trabalho.**
-   **Apresente perspectivas para melhorias ou trabalhos futuros.**
-->

<div align="justify">
&emsp;&emsp;
A utilização dos padrões comportamentais do GoF foi essencial para o desenvolvimento de um mod dinâmico e funcional. Esses padrões permitiram a criação de sistemas flexíveis e bem organizados, que respondem eficientemente às ações dos jogadores e aos eventos do jogo.

&emsp;&emsp;
A aplicação do Strategy e outros padrões comportamentais simplificou a complexidade das interações entre os componentes, garantindo uma integração harmoniosa das novas funcionalidades ao sistema existente. Essa abordagem não apenas melhorou a experiência dos jogadores, mas também facilitou o trabalho dos desenvolvedores na manutenção e expansão do mod.

&emsp;&emsp;
Com base nesse projeto, é possível afirmar que os padrões comportamentais são ferramentas indispensáveis para o desenvolvimento de sistemas complexos e interativos. Eles demonstram como boas práticas de design podem ser aplicadas a cenários de modificação de jogos, contribuindo tanto para a qualidade do software quanto para a inovação na jogabilidade.
</div>

## Bibliografia 

<!-- - **Altere!**-->

> [1] SERRANO, Milene. Videoaula: [10a - Vídeo-Aula - DSW - GoFs - Comportamentais](https://unbbr-my.sharepoint.com/:v:/g/personal/mileneserrano_unb_br/Ed5jtliOrNVOlKoPe-6Llp0BNLgJ9Q6NHPIKnzmshzHmvA?e=517a4O). [Online]. Disponível em: [https://unbbr-my.sharepoint.com/:v:/g/personal/mileneserrano_unb_br/Ed5jtliOrNVOlKoPe-6Llp0BNLgJ9Q6NHPIKnzmshzHmvA?e=517a4O](https://unbbr-my.sharepoint.com/:v:/g/personal/mileneserrano_unb_br/Ed5jtliOrNVOlKoPe-6Llp0BNLgJ9Q6NHPIKnzmshzHmvA?e=517a4O) . Acesso em: 11 dez. 2024.

> [2] SERRANO, Milene. Videoaula: [10b - Vídeo-Aula - DSW - GoFs - Comportamentais - Strategy](https://unbbr-my.sharepoint.com/:v:/g/personal/mileneserrano_unb_br/EUZop92j5WlIvdppY_A5wGcBt5xkL_HFpF8USoR6BJh4lw?e=JRjZ7B). [Online]. Disponível em: [https://unbbr-my.sharepoint.com/:v:/g/personal/mileneserrano_unb_br/EUZop92j5WlIvdppY_A5wGcBt5xkL_HFpF8USoR6BJh4lw?e=JRjZ7B](https://unbbr-my.sharepoint.com/:v:/g/personal/mileneserrano_unb_br/EUZop92j5WlIvdppY_A5wGcBt5xkL_HFpF8USoR6BJh4lw?e=JRjZ7B) . Acesso em: 11 dez. 2024.

> [3] SERRANO, Milene. Videoaula: [10c - Vídeo-Aula - DSW - GoFs - Comportamentais - Template Method](https://unbbr-my.sharepoint.com/:v:/g/personal/mileneserrano_unb_br/ES9ZjAn--55OpigenODIGAABc_QhP1yzrzXupvcaWLHgSg?e=6POebF). [Online]. Disponível em: [https://unbbr-my.sharepoint.com/:v:/g/personal/mileneserrano_unb_br/ES9ZjAn--55OpigenODIGAABc_QhP1yzrzXupvcaWLHgSg?e=6POebF](https://unbbr-my.sharepoint.com/:v:/g/personal/mileneserrano_unb_br/ES9ZjAn--55OpigenODIGAABc_QhP1yzrzXupvcaWLHgSg?e=6POebF) . Acesso em: 11 dez. 2024.

> [4] SERRANO, Milene. Videoaula: [10d - Vídeo-Aula - DSW - GoFs - Comportamentais - Demais Padrões - Visão Rápida](https://unbbr-my.sharepoint.com/:v:/g/personal/mileneserrano_unb_br/EaX4UjioPPBAkXAH72mt9BYBNf6kl8rKoGhSWWiSjGIJTQ?e=L7uUTs). [Online]. Disponível em: [https://unbbr-my.sharepoint.com/:v:/g/personal/mileneserrano_unb_br/EaX4UjioPPBAkXAH72mt9BYBNf6kl8rKoGhSWWiSjGIJTQ?e=L7uUTs](https://unbbr-my.sharepoint.com/:v:/g/personal/mileneserrano_unb_br/EaX4UjioPPBAkXAH72mt9BYBNf6kl8rKoGhSWWiSjGIJTQ?e=L7uUTs) . Acesso em: 11 dez. 2024.

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

| Versão | Data da alteração |            Alteração            |                  Responsável                  |                  Revisor                  | Data de revisão |
| :----: | :---------------: | :-----------------------------: | :-------------------------------------------: |:-----------------------------------------:| :-------------: |
|  1.0   |       11/12       |      Criação do documento       |    [Artur Barlz](https://github.com/H0lzz)    | [João Antonio G. Carvalho](https://github.com/i-JSS)  | 05/01                 | 

</div>

## Controle de Revisão

|                        Revisor(es)                        |  O que foi realizado |
|:---------------------------------------------------------:|:--------------------:|
| [João Antonio G. Carvalho](https://github.com/i-JSS)                                                           | Atualiza metodologia |
