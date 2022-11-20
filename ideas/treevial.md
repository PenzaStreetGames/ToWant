# Treevial

## Операторы претенденты

`[]` - дочерние элементы

`^[]` - элементы-предки

`>:[]` `<:[]` `:[]` - соседи (справа, слева, все)

`->` `~>` `=>` 

`*[]` - потомки n-го уровня

## Операции над элементом DOM

[Ссылка на MDN](https://developer.mozilla.org/en-US/docs/Web/API/Node) 

* Node.baseURI ?
* Дочерние узлы `node[]`
* Первый ребёнок `node[1]`
* Последний ребёнок `node[-1]`
* Node.isConnected ?
* Следующий сосед `node>:[1]`
  * Следующие соседи `node>[]`
  * Предыдущие соседи `node<[]`
  * Всё соседи `node:[]`
* Node.nodeName `node.name`
* Node.nodeValue `node.value`
* Родительский узел `node^[1]`
* 