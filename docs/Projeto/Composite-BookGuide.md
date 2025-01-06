# Composite - BookGuide


## Introdução 

O Composite é um padrão de projeto estrutural do catálogo GoF (Gang of Four) que permite a composição de objetos em estruturas de árvore para representar hierarquias de parte-todo. Ele trata objetos individuais e composições de objetos de forma uniforme, permitindo que o cliente interaja com a estrutura hierárquica sem se preocupar com os detalhes de implementação.



## Objetivos

O objetivo do Composite dentro do GOF Estrutural é Agrupar objetos que fazem parte e uma relação parte-todo de forma a tratá-los sem distinção tendo como vantagem simplifica o código cliente ao permitir que ele interaja com objetos simples e composições de maneira uniforme.

## Metodologia 
A implementação do padrão Composite segue uma estrutura clara baseada na interação entre três elementos principais: Component, Leaf e Composite. Cada elemento desempenha um papel específico na construção e manipulação de estruturas hierárquicas.

1. Component
Representa a interface ou classe abstrata base que define os métodos comuns implementados tanto pelas folhas quanto pelos compositores.
Funciona como um contrato que permite que todos os objetos na hierarquia sejam tratados de forma uniforme, sejam simples ou compostos.
Geralmente inclui métodos para:
Executar operações comuns.
Gerenciar a hierarquia, como adicionar ou remover filhos (opcional, dependendo do caso de uso).
2. Leaf
Representa os elementos atômicos da composição, aqueles que não contêm outros componentes.
Implementa os métodos definidos na interface Component, mas não suporta a adição ou remoção de filhos. Essas operações podem ser ignoradas ou resultar em uma exceção.
Realiza operações diretamente, sem delegar a outros componentes.
3. Composite
Representa os elementos que podem conter outros Components, formando a estrutura hierárquica.
Implementa os métodos definidos em Component e gerencia uma lista de filhos, que pode incluir outras instâncias de Composite ou Leaf.
É responsável por:
Adicionar e remover filhos.
Delegar ou executar operações sobre os filhos de forma recursiva.
IMPLEMENTAÇÃO
A implementação do padrão Composite segue etapas estruturadas:

Definir a interface Component:
Criar uma interface ou classe abstrata que define os métodos básicos, como operações e métodos de gerenciamento da hierarquia.

Criar a classe Leaf:
Implementar a interface Component, fornecendo uma implementação específica para as operações e ignorando métodos de gerenciamento de hierarquia.

Criar a classe Composite:
Implementar a interface Component, gerenciando uma coleção de filhos. Operações de adição, remoção e execução são implementadas para manipular e delegar ações recursivamente.

Interagir com os objetos Component:
A interação com a estrutura ocorre por meio da interface comum, sem distinção entre folhas e compositores.

O desenvolvimento do diagrama pode ser observado na seguinte gravação: https://youtu.be/9GSyYbFbf60


## Resultados

### Interface: BookComponent

```java
package com.logic_thinkering;

interface BookComponent {
    String getTitle();
    String getText();
    String getImagePath();
}
```

### Classe: BookChapter

```java
package com.logic_thinkering;

import java.util.ArrayList;
import java.util.List;

class BookChapter implements BookComponent {
    private final String title;
    private final List<BookComponent> components = new ArrayList<>();

    public BookChapter(String title) {
        this.title = title;
    }

    public void addComponent(BookComponent component) {
        components.add(component);
    }

    public List<BookComponent> getComponents() {
        return components;
    }

    @Override
    public String getTitle() {
        return title;
    }

    @Override
    public String getText() {
        StringBuilder text = new StringBuilder();
        for (BookComponent component : components) {
            text.append(component.getText()).append("\n");
        }
        return text.toString();
    }

    @Override
    public String getImagePath() {
        return "";
    }
}
```

### Classe: BookPage

```java
package com.logic_thinkering;

class BookPage implements BookComponent {
    private final String title;
    private final String text;
    private final String imagePath;

    public BookPage(String title, String text, String imagePath) {
        this.title = title;
        this.text = text;
        this.imagePath = imagePath == null ? "" : imagePath;
    }

    @Override
    public String getTitle() {
        return title;
    }

    @Override
    public String getText() {
        return text;
    }

    @Override
    public String getImagePath() {
        return imagePath;
    }
}
```


![Modelagem - Builder](https://raw.githubusercontent.com/UnBArqDsw2024-2/2024.2_G1_Logic_Thinkering_Entrega_03/refs/heads/main/assets/CompositeBookGuide.png)


## Conclusões 

A aplicação do padrão Composite na estrutura do livro mostrou-se eficiente ao organizar capítulos e páginas de forma hierárquica e flexível. A abstração pela interface BookComponent simplifica a manipulação de diferentes componentes, promovendo modularidade e permitindo futuras expansões sem alterações na lógica central.



## Bibliografia

> [1] SERRANO, Milene. Videoaula: [09c - Video-Aula - DSW - GoFs - Estruturais - Composite](https://unbbr-my.sharepoint.com/:v:/g/personal/mileneserrano_unb_br/ETbYrohUuU9DnWkIPgxjH2IBzbWEbavD-Fyk1ZWL8pK-lA?e=UvDZtO&nav=eyJyZWZlcnJhbEluZm8iOnsicmVmZXJyYWxBcHAiOiJTdHJlYW1XZWJBcHAiLCJyZWZlcnJhbFZpZXciOiJTaGFyZURpYWxvZy1MaW5rIiwicmVmZXJyYWxBcHBQbGF0Zm9ybSI6IldlYiIsInJlZmVycmFsTW9kZSI6InZpZXcifX0%3D). [Online]. Disponível em: https://unbbr-my.sharepoint.com/:v:/g/personal/mileneserrano_unb_br/ETbYrohUuU9DnWkIPgxjH2IBzbWEbavD-Fyk1ZWL8pK-lA?e=UvDZtO&nav=eyJyZWZlcnJhbEluZm8iOnsicmVmZXJyYWxBcHAiOiJTdHJlYW1XZWJBcHAiLCJyZWZlcnJhbFZpZXciOiJTaGFyZURpYWxvZy1MaW5rIiwicmVmZXJyYWxBcHBQbGF0Zm9ybSI6IldlYiIsInJlZmVycmFsTW9kZSI6InZpZXcifX0%3D. Acesso em: 05 jan. 2025.

> [2] SERRANO, Milene. Videoaula: [09a - Video-Aula - DSW - GoFs - Estruturais](https://unbbr-my.sharepoint.com/:v:/g/personal/mileneserrano_unb_br/EdRWnnpbK8BJqcsgzvh1HRUBFjYsL1ncotuK486gTMhePA?e=9N2ArH&nav=eyJyZWZlcnJhbEluZm8iOnsicmVmZXJyYWxBcHAiOiJTdHJlYW1XZWJBcHAiLCJyZWZlcnJhbFZpZXciOiJTaGFyZURpYWxvZy1MaW5rIiwicmVmZXJyYWxBcHBQbGF0Zm9ybSI6IldlYiIsInJlZmVycmFsTW9kZSI6InZpZXcifX0%3D). [Online]. Disponível em: https://unbbr-my.sharepoint.com/:v:/g/personal/mileneserrano_unb_br/EdRWnnpbK8BJqcsgzvh1HRUBFjYsL1ncotuK486gTMhePA?e=9N2ArH&nav=eyJyZWZlcnJhbEluZm8iOnsicmVmZXJyYWxBcHAiOiJTdHJlYW1XZWJBcHAiLCJyZWZlcnJhbFZpZXciOiJTaGFyZURpYWxvZy1MaW5rIiwicmVmZXJyYWxBcHBQbGF0Zm9ybSI6IldlYiIsInJlZmVycmFsTW9kZSI6InZpZXcifX0%3D. Acesso em: 05 jan. 2025.

<center>

## Participantes

</center>

<!-- de preferência: em ordem alfabética, seguindo o exemplo: -->

<div style="margin: 0 auto; width: fit-content;">

| Matrícula | Aluno                        | Git                                       |
|-----------|------------------------------|-------------------------------------------|
| 221931265 | Carlos Eduardo Rodrigues | [Carlos-Kadu](https://github.com/carlos-kadu) |
| 221031149 | Danilo César Tertuliano Melo | [Danilo Melo](https://github.com/DaniloCTM) |
| 221037993 | Patrícia Helena Macedo da Silva | [Patyhelenaa](https://github.com/patyhelenaa) |

</div>

---

<center>

## Histórico de Versão

</center>

<!-- Lembre de alterar a data -->
<!-- É PRA POR O NOME, NÃO O USER DO GITHUB -->

<div style="margin: 0 auto; width: fit-content;">

|  Versão   |      Data da alteração       |      Alteração       |                         Responsável                          | Revisor | Data de revisão |
|:---------:|:----------------------------:|:--------------------:|:------------------------------------------------------------:|:-------:|:---------------:|
|    1.0    |            05/01             | Criação do documento | [Danilo Melo](https://github.com/DaniloCTM) |         |
|    2.0    |            05/01             | Adição dos códigos | [Carlos Rodrigues](https://github.com/Carlos-kadu) |         |

</div>

## Controle de Revisão

| Revisor(es) | O que foi realizado |
|:-----------:|:-------------------:|
|             |                     |
