# A função `get_template_part( $slug, $name )`

Serve basicamente como mecanismo de modularização/reaproveitamento do código do seu tema. Evitando assim que você fique repetindo código em vários lugares. 

## Argumentos

### $slug `string` (obrigatório)
Um nome (e localização base) do arquivo.

### $name `string` (opcional)
Um complemento ao nome do arquivo.

Valor padrão: `null`.

## Valor retornado

Nenhum.

## Exemplo

Imagine a seguinte situação: 

Você tem um *banner* promocional que aparece em várias partes do seu site. 

```html
<div id="banner">
  <h3>Titulo do banner</h3>
  <img src="image.png" alt="">
</div>
```

> Este é o código que você **repete** manualmente em várias páginas para exibir o banner.

E toda vez que o banner muda você tem que ir alterar o banner manualmente em cada uma das páginas ou posts onde ele aparece, correndo o risco de esquecer de alterar alguma delas.

Para contornar esse problema você pode criar um arquivo `banner.php` (nome representativo apenas) e adicionar ele nas páginas usando a função `get_template_part`.

```php
<?php
// não precisa incluir `.php` no final
// carrega o arquivo banner.php
get_template_part('banner');
```

Você também pode usar com um segundo argumento

```php
// dessa forma, o arquivo que será carregado será o banner-special.php
get_template_part('banner', 'special'); 
?>
```

Assim você só precisa alterar o arquivo `banner.php` que automaticamente já irá alterar em todos os lugares que ele foi carregado usando `get_template_part`.

## Possíveis dúvidas

1. **Mas qual a diferença entre `get_template_part` e o `include`/`require`?**
Quase nenhuma. Porém se no seu tema tiver um arquivo `banner.php` e no tema filho (child theme) você também criar um `banner.php` então o arquivo que será carregado será o do tema filho. A vantagem disso é que você pode substituir partes do tema "pai" sem ter que alterar os arquivos principais como `index.php`, `page.php`, `search.php`, etc. Claro que o autor do tema tem que estar usando a função `get_template_part`, o que é bem comum.

## Curiosidades

1. A função `get_template_part` sempre emite (antes de carregar o arquivo) uma action chamada `get_template_part_{$slug}` com os 2 argumentos usados na função. Ex: se você usou `get_template_part('banner', 'special');` então uma action chamada `get_template_part_banner` vai ser emitida.
2. A função sempre busca os arquivos dentro do tema ativo. Se o tema ativo for um tema filho, então primeiro a função procura dentro do tema filho e, caso não encontrar, então vai tentar procurar dentro do tema pai. Ex: `get_template_part('banner')` vai sempre buscar um arquivo `banner.php` na raiz do tema. Enquanto `get_template_part('templates/banner')` vai buscar por um arquivo `banner.php` dentro de uma pasta chamada `templates`.
3. Se você chamar a função como `get_template_part('banner', 'special')` um arquivo chamado `banner-special.php` será carregado (como já foi dito). Porém se aquele arquivo não existir, então será carregado o arquivo `banner.php` (caso este exista). De qualquer maneira, caso nenhum dos arquivos exista, não irá ocorrer nenhum erro (mas aquela action ainda será emitida).

## Exercícios

- Instalar os dois temas da pasta [demo](https://github.com/luizbills/mentoria-do-ct/tree/master/wordpress/get-template-part/chapter-1/demo) (um é tema pai e outro é tema filho).
- Ativar o tema pai (Demo) e ir na página inicial.
- Ativar o tema filho (Demo child) e ir na página inicial. Verificando que o conteúdo mudou.
- Analisar códigos dos arquivos dos temas (ignorando os arquivos `style.css` e `functions.php`).

## Referências
https://developer.wordpress.org/reference/functions/get_template_part/
