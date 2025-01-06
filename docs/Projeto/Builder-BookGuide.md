# Builder - BookGuide

## Introdução
  O Builder é um padrão de design que permite construir objetos complexos passo a passo. Ele separa a construção de um objeto da sua representação, permitindo criar diferentes representações do mesmo objeto com o mesmo processo de construção.

## Objetivo
  O objetivo do Builder é facilitar a criação de objetos complexos, encapsulando a lógica de construção em uma estrutura clara e reutilizável.

## Metodologia
  Durante a implementação do GuideBook, enfrentamos vários desafios devido à nossa falta de experiência com desenvolvimento de mods para o Minecraft e também com a linguagem Java. A maioria do código foi feito em conjunto pelo Discord, mas inicialmente também trabalhamos individualmente para tentar superar as dificuldades iniciais (divisão e conquista). 
  Para a criação do diagrama, seguimos as videoaulas da professora e nos reunimos no discord, afim de realizar o trabalho em conjunto.

## Resultados

### Interface: BookGuideBuilder

```java
package com.logic_thinkering;

interface BookGuideBuilder {
    void addPage(String title, String text, String imagePath);
    BookGuide build();
}
```

### Classe: BookGuideDirector

```java
package com.logic_thinkering;

import java.util.ArrayList;
import java.util.List;

public class BookGuideDirector {
    private BookGuideBuilder builder;

    public BookGuideDirector(BookGuideBuilder builder) {
        this.builder = builder;
    }

    public BookGuide construct() {
        return builder.build();
    }
}
```

### Classe: StandardBookGuideBuilder

```java
package com.logic_thinkering;

import java.util.ArrayList;
import java.util.List;

class StandardBookGuideBuilder implements BookGuideBuilder {
    private final List<BookPage> pages = new ArrayList<>();

    @Override
    public void addPage(String title, String text, String imagePath) {
        pages.add(new BookPage(title, text, imagePath));
    }

    @Override
    public BookGuide build() {
        return new BookGuide(pages);
    }
}
```

### Classe: BookGuide

```java
package com.logic_thinkering;

import java.util.List;

class BookGuide {
    private final List<BookPage> components;
    private int currentPageIndex = 0;

    public BookGuide(List<BookPage> components) {
        this.components = components;
    }

    public BookComponent getCurrentComponent() {
        return components.get(currentPageIndex);
    }

    public List<BookPage> getComponents() {
        return components;
    }

    public boolean hasNextComponent() {
        return currentPageIndex < components.size() - 1;
    }

    public boolean hasPreviousComponent() {
        return currentPageIndex > 0;
    }

    public void nextComponent() {
        if (hasNextComponent()) {
            currentPageIndex++;
        }
    }

    public void previousComponent() {
        if (hasPreviousComponent()) {
            currentPageIndex--;
        }
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

![Modelagem - Builder](https://raw.githubusercontent.com/UnBArqDsw2024-2/2024.2_G1_Logic_Thinkering_Entrega_03/refs/heads/main/assets/BookGuide.png)

## Conclusões
O padrão Builder foi aplicado com sucesso para implementar o GuideBook, permitindo organizar e simplificar o processo de construção do objeto.
  
## Bibliografia
  
  [1] SERRANO, Milene. Videoaula: 08d - Video-Aula - DSW - GoFs - Criacionais - Demais [Online]. Disponível em: https://unbbr-my.sharepoint.com/personal/mileneserrano_unb_br/_layouts/15/stream.aspx?id=%2Fpersonal%2Fmileneserrano_unb_br%2FDocuments%2FArqDSW%20-%20VídeosOriginais%2F08d%20-%20Video-Aula%20-%20DSW%20-%20GoFs%20-%20Criacionais%20-%20Demais.mp4&ga=1&referrer=StreamWebApp.Web&referrerScenario=AddressBarCopied.view.79c9410e-1f56-4149-ae94-f98a25e8a02a\. Acesso em: 05 jan. 2025.

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
