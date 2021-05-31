# 操作 XML 文件

## xml.dom

利用 Python 的 `xml.dom.minidom` 包来操作 xml 文件

注意区分`Node`、`Element`、`Attribute`、`Text`：

- `Node `节点：构成 DOM 的基础，是 XML 文档各组件的父类，包含元素节点、属性节点、文本节点

- `Element `元素节点：类似 HTML 里的标签，如  `<html>`、`<body>`、`<p>`、`<span>`等

- `Attribute` 属性节点：类似标签属性，例如 `<p id="desc">`，其中 `id="desc"` 即为属性节点

- `Text` 文本节点：元素节点中包含的文本，例如 `<p id="desc">这是一个描述</p>`，其中 `这是一个描述` 即为一个文本节点

### Node 节点对象

XML 文档的所有组件都是 Node 的子类。

- `Node.nodeType` ：表示节点类型的整数

扩展：`Node` 类型对应常量

![image-20191122153335799](C:\Users\dbsours\AppData\Roaming\Typora\typora-user-images\image-20191122153335799.png)

- `Node.parentNode`：当前节点的父节点，文档节点的父节点为 `None`，根节点的父节点是文档对象，属性节点的父节点总为 `None`

- `Node.attributes`：属性对应的命名节点图，只有元素才有实际的值，其他均为 `None`

- `Node.previousSibling`：当前节点前面的兄弟节点，若当前节点为父节点的第一个子节点，则返回 `None`

- `Node.nextSibling`：当前节点后面的兄弟节点，若当前节点为父节点的最后一个子节点，返回 `None`

- `Node.childNodes`：当前节点包含的节点列表

- `Node.firstChild`：当前节点的第一个子节点，否则为`None`

- `Node.lastChild`：当前节点的最后一个子节点，否则为`None`

- `Node.localName`： 返回被选元素的本地名称(元素名称)。 

- `Node.prefix`：返回选定的元素的命名空间前缀。 

- `Node.nodeName`：对于元素节点，返回元素标签名；对于文本节点，返回`#text`；对于属性节点，返回属性名

- `Node.nodeValue`：对于元素节点，返回`None`；对于文本节点，返回文本值，对于属性节点，返回属性值

- `Node.hasAttributes()`：如果节点有属性则返回 `True`

- `Node.hasChildNodes()`：如果节点有子节点则返回 `True`

- `Node.isSameNode(other)`：如果当前节点和指定节点是同一个节点返回 `True`

- `Node.appendChild(newChild)`：在子节点列表末尾添加一个`newChild`，返回 `newChild` 如果子节点已在树中，则先删除它

- `Node.insertBefore(newChild,refChild)`：在子节点 `refChild` 前插入一个`newChild`，并返回`newChild`。如果 `refChild` 不存在，则报 `ValueError` 异常。如果 `refChild` 为 `None`，则在子列表的末尾插入 `newChild`

- `Node.removeChild(oldChild)`：删除子节点，如果 `oldChild`不是该节点的子节点，则报`ValueError` 异常，删除成功返回 `oldChild`，如果 `oldChild` 不再被使用，那么应该调用它的 `unlink()` 方法。

- `Node.replaceChild(newChild,oldChild)`：用 `newChild` 替换 `oldChild`。如果 `oldChild`不是该节点的子节点，则报`ValueError` 异常

- `Node.normalize()`：连接相邻的文本节点，以便将所有文本扩展存储为单个文本实例。这简化了许多应用程序对来自DOM树的文本的处理。

- `Node.cloneNode(deep)`：克隆当前节点，并返回克隆节点。设置 `deep` 意味着克隆所有子节点。

### NodeList 节点列表对象

节点列表表示节点序列，该对象在 DOM 核心推荐中有两种使用方式：

- 元素对象提供的子节点列表
- `getElementsByTagName()` 和 `getElementsByTagNameNS() `方法获取的结果列表

- `NodeList.item(i)`：返回节点序列的第 i 项，i 不能超过序列长度范围（0-len）

- `NodeList.length`：返回节点序列的长度

### Document 文档对象

利用 DOM 解析器解析 xml 文档，获取包含文档所有元素的树结构（`Document` 对象），后续操作主要在这个树结构的基础上进行

```python
from xml.dom.minidom import parse

#获取文档树结构
DOMTree = parse(r'文件名.xml')
```

- `Document.documentElement`：获取文档根节点

- `Document.createElement(tagName)`：创建并返回一个新的元素节点，元素在创建时没有插入到文档中，需要使用`insertBefore()` 或 `appendChild` 显示插入

- `Document.createElementNS(namespaceURI, tagName)`：创建带有指定命名空间的元素节点，标签名可能会有一个前缀。该元素不会插入到文档中，需使用`insertBefore()` 或 `appendChild` 显示插入

- `Document.createTextNode(data)` ：创建并返回一个文本节点（包含参数传递的数据），同其他创建方法一样，该节点不插入到文档中

- `Document.createComment(data)`：创建并返回一个注释节点，该节点包含作为参数传递的数据，并且不插入到文档中

- `Document.createAttribute(name)`：创建并返回一个属性节点，该属性节点不与任何特定元素关联，必须使用 `setAttributeNode()` 与元素节点关联

- `Document.createAttributeNS(namespaceURI,qualifiedName)`：创建并返回带有命名空间的属性节点。标签名可能有一个前缀。此方法不将属性节点与任何特定元素关联。必须在适当的元素对象上使用 `setAttributeNode()`来使用新创建的属性实例。

- `Document.createProcessingInstruction(target,data)`：创建并返回包含作为参数传递的目标和数据的处理指令节点。与其他创建方法一样，这个方法不将节点插入树中。

- `Document.getElementsByTagName(tagName)`：获取文档中所有名称的元素节点

- `Document.getElementsByTagNameNS(namespaceURI,localName)`：获取文档中所有命名空间为`localName` 前缀的元素节点。`localName` 是前缀之前的命名空间的一部分

### Element 元素节点

`Element` 是 `Node` 的子类，因此继承了该类的所有属性

- `Element.tagName`：元素的标签名。在使用命名空间的文档中可能会包含`:`

- `Element.getElementsByTagName(tagName)`：获取标签名为 `tagName` 的所有元素

- `Element.getElementsByTagNameNs(namespaceURI, localName)`：获取节点中命名空间为`localName` 前缀的元素节点。

- `Element.hasAttribute(name)`：如果元素具有属性名为 `name` 的属性，则返回 True

- `Element.haisAttributeNS(namespaceURI,localName)`：如果元素具有由 `namespaceURI` 和 `localName `命名的属性，则返回 True

- `Element.getAttribute(name)`：返回元素属性名为 `name` 的属性值，如果 `name` 属性不存在则返回空

- `Element.getAttributeNode(attrname)`：返回元素属性名为 `attrname` 的属性节点

- `Element.getAttributeNs(namespaceURI,localName)`：返回具有由 `namespaceURI` 和 `localName `命名的属性的值

- `Element.getAttributeNodeNS(namespaceURI,localName)`：返回元素具有由 `namespaceURI` 和 `localName` 命名的属性的属性节点

- `Element.removeAttribute(name)`：移除元素节点属性名为`name` 的属性，如果没有该属性则报 `NotFoundErr` 异常

- `Element.removeAttributeNode(oldAttr)`：从属性列表中移除并返回属性名为`oldAttr` 的属性，如果没有该属性则报 `NotFoundErr` 异常

- `Element.removeAttributeNS(namespaceURI,localName)`：按 `localName` 删除属性。注意，它使用的是`localName`，而不是`qname`。如果没有匹配属性，则不会引发异常。

- `Element.setAttribute(name,value)`：设置属性，有就更改，没有就添加

- `Element.setAttributeNS(namespaceURI, qname, value)`：创建或设置由命名空间 URI `namespaceURI` 和 限定名共同指定的属性。 除了可以改变一个属性的值以外，使用该方法还可以改变属性的名字空间前缀。 

- `Element.setAttributeNode(newAttr)`：向元素中添加指定的属性节点，如果属性名已存在则替换它并返回旧的属性节点，如果 `newAttr` 已经在使用，则报 `InuseAttributeErr` 异常。

- `Element.setAttributeNodeNS(newAttr)`：向元素中添加指定的属性节点，如果命名空间`namespace`和`localname` 指定的属性存在，则替换它并返回旧的属性节点，如果 `newAttr` 已经在使用，则报 `InuseAttributeErr` 异常。

### Attr 对象

`Attr` 是`Node` 的子类，因此继承了该类的所有属性

- `Attr.name` ：属性名称，在使用命名空间的文档中，可能会包含一个 `:`

- `Attr.localName`：属性限定名称，即`:` 之后的部分，否则就是整个名称

- `Attr.prefix`：属性前缀名，即`:`之前的部分，否则就是整个名称

- `Attr.value`：属性的文本值，与 `nodeValue` 具有相同的意义

### NamedNodeMap 对象

`NamedNodeMap` 不是 `Node` 的子类

- `NamedNodeMap.length`：属性节点序列长度
- `NamedNodeMap.item(index)`：返回指定索引位置的属性，每一项都是一个属性节点，可通过 `value` 属性来获取属性值

### Comment 对象

`Comment` 表示XML文档中的注释。它是Node的子类，但是不能有子节点。

- `Comment.data`：注释的内容作为字符串。属性包含在 `<!--` 和 `-->` 字符中，但不包括它们。