<p align="center">
  <a href="https://github.com/kettanaito/naming-cheatsheet">
    <img src="./naming-cheatsheet.png" alt="Naming cheatsheet" />
  </a>
</p>

# Cheatsheet de nomeclatura

- [Escreva em inglês](#escreva-em-inglês)
- [Padrão de nomenclatura](#padrão-de-nomenclatura)
- [C-I-D](#c-i-d)
- [Avoid contractions](#avoid-contractions)
- [Avoid context duplication](#avoid-context-duplication)
- [Reflect the expected result](#reflect-the-expected-result)
- [Naming functions](#naming-functions)
  - [A/HC/LC pattern](#ahclc-pattern)
    - [Actions](#actions)
    - [Context](#context)
    - [Prefixes](#prefixes)
- [Singular and Plurals](#singular-and-plurals)

---

Dar nome às coisas é uma tarefa difícil. Este documenta tenta tornar esta tarefa mais fácil.

Embora essas sugestões possam ser aplicadas a qualquer linguagem de programação, usarei JavaScript para ilustrá-las na prática.

## Escreva em inglês

Nomeie suas variáveis e funções em inglês.

```js
/* Ruim */
const primeiroNome = 'Gustavo'
const amigos = ['João', 'Raul']

/* Bom */
const firstName = 'Gustavo'
const friends = ['João', 'Raul']
```

> Goste ou não, o Inglês é a língua dominante na programação: a sintaxe de todas as linguagens de programação é escrita em Inglês, assim como inúmeras documentações e materiais educacionais. Ao escrever seu código em Inglês, você aumenta drasticamente sua coesão.

## Padrão de nomenclatura

Escolha **um** padrão de nomenclatura e siga o mesmo. Podendo ser `camelCase`, `PascalCase`, `snake_case`, ou qualquer outro que te agrade ou agrade a equipe, contando que seja consistente. Algumas linguaguens de programação tem seu próprio contrato sobre padrão de nomenclaturas; para isso, olhe a documentação da linguaguem e verifique repositórios populares no Github!

```js
/* Ruim */
const page_count = 5
const shouldUpdate = true

/* Bom */
const pageCount = 5
const shouldUpdate = true

/* Bom também */
const page_count = 5
const should_update = true
```

## C-I-D

Um nome deve ser _curto_, _intuitivo_ e _descritivo_:

- **Curto**. Um nome não deve demorar para ser digitado e, portanto, para ser lembrado;

- **Intuitive**. Um nome deve ser lido de forma natural, o mais próximo do que se é usado comumente no idioma;

- **Descriptive**. Um nome deve refletir o que faz/possui da maneira mais eficiente.

```js
/* Ruim */
const a = 5 // "a" pode ser significar qualquer coisa
const isPaginatable = a > 10 // "Paginatable" não tem um som legal
const shouldPaginatize = a > 10 // Verbos compostos são muito divertidos!

/* Bom */
const postCount = 5
const hasPagination = postCount > 10
const shouldPaginate = postCount > 10 // alternativa
```

## Avoid contractions

Do **not** use contractions. They contribute to nothing but decreased readability of the code. Finding a short, descriptive name may be hard, but contraction is not an excuse for not doing so.

```js
/* Bad */
const onItmClk = () => {}

/* Good */
const onItemClick = () => {}
```

## Avoid context duplication

A name should not duplicate the context in which it is defined. Always remove the context from a name if that doesn't decrease its readability.

```js
class MenuItem {
  /* Method name duplicates the context (which is "MenuItem") */
  handleMenuItemClick = (event) => { ... }

  /* Reads nicely as `MenuItem.handleClick()` */
  handleClick = (event) => { ... }
}
```

## Reflect the expected result

A name should reflect the expected result.

```jsx
/* Bad */
const isEnabled = itemCount > 3
return <Button disabled={!isEnabled} />

/* Good */
const isDisabled = itemCount <= 3
return <Button disabled={isDisabled} />
```

---

# Naming functions

## A/HC/LC Pattern

There is a useful pattern to follow when naming functions:

```
prefix? + action (A) + high context (HC) + low context? (LC)
```

Take a look at how this pattern may be applied in the table below.

| Name                   | Prefix   | Action (A) | High context (HC) | Low context (LC) |
| ---------------------- | -------- | ---------- | ----------------- | ---------------- |
| `getUser`              |          | `get`      | `User`            |                  |
| `getUserMessages`      |          | `get`      | `User`            | `Messages`       |
| `handleClickOutside`   |          | `handle`   | `Click`           | `Outside`        |
| `shouldDisplayMessage` | `should` | `Display`  | `Message`         |                  |

> **Note:** The order of context affects the meaning of a variable. For example, `shouldUpdateComponent` means _you_ are about to update a component, while `shouldComponentUpdate` tells you that _component_ will update on itself, and you are but controlling when it should be updated.
> In other words, **high context emphasizes the meaning of a variable**.

---

## Actions

The verb part of your function name. The most important part responsible for describing what the function _does_.

### `get`

Accesses data immediately (i.e. shorthand getter of internal data).

```js
function getFruitCount() {
  return this.fruits.length
}
```

> See also [compose](#compose).

You can use `get` when performing asynchronous operations as well:

```js
async function getUser(id) {
  const user = await fetch(`/api/user/${id}`)
  return user
}
```

### `set`

Sets a variable in a declarative way, with value `A` to value `B`.

```js
let fruits = 0

function setFruits(nextFruits) {
  fruits = nextFruits
}

setFruits(5)
console.log(fruits) // 5
```

### `reset`

Sets a variable back to its initial value or state.

```js
const initialFruits = 5
let fruits = initialFruits
setFruits(10)
console.log(fruits) // 10

function resetFruits() {
  fruits = initialFruits
}

resetFruits()
console.log(fruits) // 5
```

### `remove`

Removes something _from_ somewhere.

For example, if you have a collection of selected filters on a search page, removing one of them from the collection is `removeFilter`, **not** `deleteFilter` (and this is how you would naturally say it in English as well):

```js
function removeFilter(filterName, filters) {
  return filters.filter((name) => name !== filterName)
}

const selectedFilters = ['price', 'availability', 'size']
removeFilter('price', selectedFilters)
```

> See also [delete](#delete).

### `delete`

Completely erases something from the realms of existence.

Imagine you are a content editor, and there is that notorious post you wish to get rid of. Once you clicked a shiny "Delete post" button, the CMS performed a `deletePost` action, **not** `removePost`.

```js
function deletePost(id) {
  return database.find({ id }).delete()
}
```

> See also [remove](#remove).

> **`remove` or `delete`?**
>
> When the difference between `remove` and `delete` is not so obvious to you, I'd sugguest looking at their opposite actions - `add` and `create`.
> The key difference between `add` and `create` is that `add` needs a destination while `create` **requires no destination**. You `add` an item _to somewhere_, but you don't "`create` it _to somewhere_".
> Simply pair `remove` with `add` and `delete` with `create`.
>
> Explained in detail [here](https://github.com/kettanaito/naming-cheatsheet/issues/74#issue-1174942962).

### `compose`

Creates new data from the existing one. Mostly applicable to strings, objects, or functions.

```js
function composePageUrl(pageName, pageId) {
  return pageName.toLowerCase() + '-' + pageId
}
```

> See also [get](#get).

### `handle`

Handles an action. Often used when naming a callback method.

```js
function handleLinkClick() {
  console.log('Clicked a link!')
}

link.addEventListener('click', handleLinkClick)
```

---

## Context

A domain that a function operates on.

A function is often an action on _something_. It is important to state what its operable domain is, or at least an expected data type.

```js
/* A pure function operating with primitives */
function filter(list, predicate) {
  return list.filter(predicate)
}

/* Function operating exactly on posts */
function getRecentPosts(posts) {
  return filter(posts, (post) => post.date === Date.now())
}
```

> Some language-specific assumptions may allow omitting the context. For example, in JavaScript, it's common that `filter` operates on Array. Adding explicit `filterArray` would be unnecessary.

---

## Prefixes

Prefix enhances the meaning of a variable. It is rarely used in function names.

### `is`

Describes a characteristic or state of the current context (usually `boolean`).

```js
const color = 'blue'
const isBlue = color === 'blue' // characteristic
const isPresent = true // state

if (isBlue && isPresent) {
  console.log('Blue is present!')
}
```

### `has`

Describes whether the current context possesses a certain value or state (usually `boolean`).

```js
/* Bad */
const isProductsExist = productsCount > 0
const areProductsPresent = productsCount > 0

/* Good */
const hasProducts = productsCount > 0
```

### `should`

Reflects a positive conditional statement (usually `boolean`) coupled with a certain action.

```js
function shouldUpdateUrl(url, expectedUrl) {
  return url !== expectedUrl
}
```

### `min`/`max`

Represents a minimum or maximum value. Used when describing boundaries or limits.

```js
/**
 * Renders a random amount of posts within
 * the given min/max boundaries.
 */
function renderPosts(posts, minPosts, maxPosts) {
  return posts.slice(0, randomBetween(minPosts, maxPosts))
}
```

### `prev`/`next`

Indicate the previous or the next state of a variable in the current context. Used when describing state transitions.

```jsx
async function getPosts() {
  const prevPosts = this.state.posts

  const latestPosts = await fetch('...')
  const nextPosts = concat(prevPosts, latestPosts)

  this.setState({ posts: nextPosts })
}
```

## Singular and Plurals

Like a prefix, variable names can be made singular or plural depending on whether they hold a single value or multiple values.

```js
/* Bad */
const friends = 'Bob'
const friend = ['Bob', 'Tony', 'Tanya']

/* Good */
const friend = 'Bob'
const friends = ['Bob', 'Tony', 'Tanya']
```
