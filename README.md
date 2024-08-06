06-estudo-sass

Este repositório contém todos os recursos, exemplos e notas que compilei enquanto aprendia SASS Meu objetivo é fornecer uma referência útil e prática a partir do que aprendi e documentar meu próprio progresso.

### INDICE

- [NESTING](#nesting)
- [VARIAVEIS](#variaveis)
- [@USE](#use)
- [@FORWARD](#forward)
- [@MIXIN AND INCLUDE](#mixin-and-include)
- [@EXTEND](#extend)
- [@AT-ROOT](#at-root)

#

### NESTING

Nesting no Sass permite aninhar seletores CSS de maneira hierárquica, imitando a estrutura HTML. Isso facilita a manutenção de estilos relacionados.

EX:

```
body {
  margin: 0px;
  font-family: sans-serif;
  color: #7f808c;
}

#chat {
  display: flex;
  flex-direction: column;
  margin: 20px;
  padding: 0px;
  list-style: none;
  li {
    display: flex;
    margin-bottom: 32px;
    .avatar {
      padding: 0 16px;
      display: flex;
      align-items: flex-end;
      img {
        border-radius: 50%;
        width: 48px;
      }
    }
    .message {
      flex: 1;
      background-color: #f5f6fa;
      padding: 16px;
      border-radius: 8px;
      border-bottom-left-radius: 0px;
      strong {
        color: #242939;
      }
      p {
        font-size: 14px;
      }
      time {
        font-size: 12px;
        letter-spacing: 1px;
        opacity: 0.65;
      }
    }
    .menu {
      display: flex;
      align-items: center;

      button {
        border: none;
        background: none;
        outline: none;

        img {
          cursor: pointer;
          width: 16px;
          opacity: 0.5;
        }
      }
    }
  }
}
```

#

### VARIAVEIS

Funcionalidades que permitem armazenar valores par asemrem utilizados no código sempre que forem requisitadas.

EX:

```
$color-header: #ff760c;
$color-header-transparent: rgba($color-header, 0.5);
$color-title: white;
$color-section: rgb(0, 82, 182);

#variaveis {
  margin: 0;
  height: 150px;
  font-family: sans-serif;
  background-color: $color-header-transparent;

  header {
    display: flex;
    align-items: center;
    justify-content: center;
    height: 50px;
    background-color: $color-header;

    h1 {
      margin: 0;
      color: $color-title;
    }
  }

  section {
    display: flex;
    align-items: center;
    justify-content: center;
    color: $color-title;
    background-color: $color-section;
    height: 70px;

    p {
      margin: 0;
    }
  }
}
```

#

### @USE

Diretiva @use substitui a diretiva @import, serve para modularizar e ter uma melhor organização ao código.

O que significa que todos os estilos e variáveis ficam disponíveis, mas com um namespace ( \_name.scss ) para evitar conflitos de nome.

EX:  
`style.scss`

```
@use "theme" as t1; // Para carregar folhas de estilo e podemos dar apelido a ele par afacilitar

body {
  background-color: t1.$bg-color;
}

#chat {
  li {
    .message {
      @include t1.arredondado;
    }
  }
}
```

`_theme.scss`

```
$bg-color: rgba(0, 0, 0, 0.256);
$radius: 15px;

@mixin arredondado {
  border-radius: $radius;
}
```

#

### @FORWARD

A diretiva @forward permite que você agrupe e reexporte módulos, de forma organizada e modular.

@forward reexporta as definições de outro arquivo Sass, tornando-as disponíveis para outros arquivos que importarem o módulo que contém a diretiva @forward.

Isso é útil para criar bibliotecas de estilos que podem ser compartilhadas e reutilizadas em todo o projeto.

`style.scss`

```
@use "boot" as bt;

#variaveis {
  margin: 0;
  height: 150px;
  font-family: sans-serif;
  background-color: bt.$color-header-transparent;

  header {
    display: flex;
    align-items: center;
    justify-content: center;
    height: 50px;
    background-color: bt.$color-header;

    h1 {
      margin: 0;
      color: bt.$color-title;
    }
  }

  section {
    display: flex;
    align-items: center;
    justify-content: center;
    color: bt.$color-title;
    background-color: bt.$color-section;
    height: 70px;

    p {
      margin: 0;
    }
  }
}
footer {
  height: 50px;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: bt.$footer;
}
```

`_boot.scss`

```
@forward "color";
@forward "radius";
```

`_color.scss`

```
$bg-color: rgb(255, 255, 255) !default;
$primari-color: red !default;
$primari-text-color: #fff !default;
$hiperlink: red !default;
$footer: orange !default;
$color-header: #ff760c;
$color-header-transparent: rgba($color-header, 0.5);
$color-title: white;
$color-section: rgb(0, 82, 182);
```

`_radius.scss`

```
$border-radius: 0 !default;
```

#

### MIXIN AND INCLUDE

No Sass, @mixin e @include são usadas para reutilização de estilos. Elas ajudam a escrever CSS de forma mais eficiente, modular e DRY (Don't Repeat Yourself).

- @mixin permite definir blocos de CSS reutilizáveis que podem ser incluídos em qualquer lugar do seu arquivo Sass. Mixins podem aceitar argumentos.

- @include é usada para incluir um @mixin definido anteriormente. Isso insere o conteúdo do @mixin no local onde @include é utilizado.

`style.scss`

```
@use "theme" with (
  $bg-color: rgb(210, 210, 210),
  $border-radius: 10px
);

/* USO DIRETO DE VARIAVEIS */
$color-header: #ff760c;
$color-header-transparent: rgba($color-header, 0.5);
$color-title: white;
$color-section: rgb(0, 82, 182);

#variaveis {
  margin: 0;
  height: 150px;
  font-family: sans-serif;
  background-color: $color-header-transparent;

  header {
    @include theme.display-flex;
    height: 50px;
    background-color: $color-header;

    h1 {
      margin: 0;
      color: $color-title;
    }
  }

  section {
    @include theme.display-flex;
    color: $color-title;
    background-color: $color-section;
    height: 70px;

    p {
      margin: 0;
    }
  }
}
```

`_theme.scss`

```
$bg-color: rgb(255, 255, 255) !default;
$primari-color: red !default;
$primari-text-color: #fff !default;
$border-radius: 0 !default;
$hiperlink: red;
$footer: orange;

@mixin display-flex {
  display: flex;
  justify-content: center;
  align-items: center;
}
```

#

### @EXTEND

A diretiva @extend é usada para compartilhar um conjunto de regras CSS entre diferentes seletores. Isso permite que seletores reutilizem regras CSS comuns, promovendo a DRY (Don't Repeat Yourself).

Quando você usa @extend, o Sass copia todas as regras do seletor extendido para o seletor que está fazendo a extensão. Isso significa que os seletores compartilharão as mesmas regras para evitar a repetição de código.

`style.scss`

```
.error {
  width: 300px;
  text-align: center;
  padding: 10px;
  margin: 20px;
  background-color: aqua;
  &--serious {
    @extend .error;
    border: 2px solid red;
  }
}
```

#

### @AT-ROOT

A diretiva @at-root permite controlar a aninhamento de estilos no CSS resultante. Normalmente, quando se aninha seletores, os estilos são refletidos de maneira aninhada no CSS gerado. Com @at-root, pode-se "quebrar" esse aninhamento e mover os estilos para o nível mais alto (a raiz) do CSS, ignorando o contexto aninhado atual.

`style.scss`

```
@use "sass:selector";

@mixin field($child) {
  @at-root #{selector.unify(&, $child)} {
    padding: 10px;
    border: 2px solid red;
    margin: 5px;
    @content;
  }
}

.wrapper {
  display: flex;
  flex-direction: column;
  width: 300px;
  .field {
    @include field("input");
    @include field("select");
    background-color: antiquewhite;
  }
}
```

#

🚧 Projeto EM CONSTRUÇÃO 🚧
