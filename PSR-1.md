Padrão de Codificação Básico
=====================

Esta seção do padrão compreende o que deve ser considerado dos elementos codificação padrão que são necessários para garantir um alto nível de interoperabilidade técnica entre código PHP compartilhado.

As palavras-chave "DEVE", "NÃO DEVE", "REQUERIDO", "DEVERIA", "NÃO DEVERIA", "RECOMENDADO", "PODE" e "OPCIONAL" nesse documento devem ser interpretadas como descrito na [RFC 2119](http://www.ietf.org/rfc/rfc2119.txt).

[PSR-0]: https://github.com/enricopereira/PSR_PT-BR/blob/master/PSR-0.md


1. Visão geral
-----------

- Arquivos DEVEM usar apenas tags `<?php` e `<?=`.

- Arquivos DEVEM usar apenas UTF-8 sem BOM para código PHP.

- Arquivos DEVERIAM _ou_ declarar símbolos (classes, funções, contantes, etc.) _ou_ causar outros efeitos (ex: gerar output, alterar configurações .ini, etc.), mas NÃO DEVERIAM fazer as duas coisas.

- Namespaces e classes DEVEM seguir a [PSR-0][].

- Nomes de classe DEVEM ser declarados em `StudlyCaps`.

- Constantes de classes DEVEM ser declaradas todas em letra maiúscula ("upper case") com separadores underline.

- Nomes de métodos DEVEM ser declarados em `camelCase`.


2. Arquivos
--------

### 2.1. Tags PHP

Código PHP DEVE usar as tags longas `<?php ?>` ou as short-echo tags `<?= ?>`; NÃO DEVE se utilizar outras variantes de tag.

### 2.2. Codificação de Caracteres

Código PHP DEVE usar apenas UTF-8 sem BOM.

### 2.3. Efeitos secundários

Um arquivo DEVERIA declarar novos símbolos (classes, funções, contantes, etc.) e não causar outros efeitos colaterais ou ele DEVERIA executar lógica com efeitos secundários, mas NÃO DEVERIA fazer ambos.

A expressão "efeitos secundários" significa a execução da lógica não diretamente ligada com declaração de classes, funções, constantes, etc, _meramente pela inclusão do arquivo_.

"Efeitos secundários" incluem, mas não estão limitados a: geração de output, uso explícito de `require` ou `include`, conexão a serviços externos, modificação de configurações ini, emissão de erros ou exceções, modificação de variáveis ​​globais ou estáticas,
ler ou escrever em um arquivo e assim por diante.

A seguir está um exemplo de um arquivo com ambos, declarações e efeitos secundários; um exemplo de que deve ser evitado:

```php
<?php
// efeito secundário: mudança nas configurações ini
ini_set('error_reporting', E_ALL);

// efeito secundário: carregamento de arquivo
include "file.php";

// efeito secundário: geração de output
echo "<html>\n";

// declaração
function foo()
{
    // corpo da função
}
```

O exemplo a seguir é de um arquivo que contém declarações sem efeitos secundários; um exemplo do que deve ser feito:

```php
<?php
// declaração
function foo()
{
    // corpo da função
}

// declaração condicional *não é* um efeito secundário
if (! function_exists('bar')) {
    function bar()
    {
        // corpo da função
    }
}
```


3. Namespace e Nomes de Classe
----------------------------

Namespaces e classes DEVEM seguir a [PSR-0][].

Isso significa que cada classe está em um arquivo, por si mesmo, e é em uma namespace de, pelo menos, um nível: um nome de fornecedor de nível superior.

Nomes de classe DEVEM ser declaradas em `StudlyCaps`.

Código escrito para PHP 5.3 e superior DEVE utilizar namespaces formal.

Por exemplo:

```php
<?php
// PHP 5.3 e superior:
namespace Vendor\Model;

class Foo
{
}
```

Código escrito para 5.2.x e inferior DEVERIA utilizar a convenção de pseudo-namespace de prefixos `Vendor_` em nomes de classe.

```php
<?php
// PHP 5.2.x e inferior:
class Vendor_Model_Foo
{
}
```

4. Constantes de Classe, Propriedades e Métodos
-------------------------------------------

O termo "classe" se refere a todas as classes, interfaces e traits.

### 4.1. Constantes

Constantes de classe DEVEM ser todas declaradas em letra maiúscula ("upper case") com separados underline.
Por exemplo:

```php
<?php
namespace Vendor\Model;

class Foo
{
    const VERSION = '1.0';
    const DATE_APPROVED = '2012-06-01';
}
```

### 4.2. Propriedades

Este guia intencionalmente evita qualquer recomendação sobre o uso de `$StudlyCaps`, `$camelCase` ou `$under_score` em nomes de propriedades.

Seja qual for a convenção de nomenclatura usada, ela DEVERIA ser aplicada consistentemente dentro de um escopo razoável. Esse escopo pode ser a nível de fornecedor, nível de pacote, nível de classe ou nível de método.

### 4.3. Métodos

Nomes de método DEVEM ser declarados em `camelCase()`.
