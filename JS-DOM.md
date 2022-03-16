# JavaScript [DOM]

### DOM loading events

- `document.addEventListiner('DOMContentLoaded', () => {});`
  - triggers after rendaring engine parses html.
- `window.addEventListiner('load', () => {});`
  - triggers after a page load.

---

### JavaScript Loading

- defer atteribute.
  - `<script defer src="/path.js"></script>`
  - fetches script and parses just before DOMContentLoaded.
- async atteribute.
  - `<script defer src="/path.js"></script>`
  - fetches script and parses asap.

---

### Nodes In-Depth

- document

  - `console.dir('document', document);`

- html

  - `console.log('html', document.documentElement);`

- head

  - `console.log('head', document.head);`

- body

  - `console.log('body', document.body);`

- retrieve constructor name

  - `console.log('body constructor name: ', document.body.constructor.name); // HTMLBodyElement`

- looking at the prototype chain

```
  console.log(document.body instanceof HTMLBodyElement); // true
  console.log(document.body instanceof HTMLElement); // true
  console.log(document.body instanceof Element); // true
  console.log(document.body instanceof Node); // true
  console.log(document.body instanceof EventTarget); //true
```

- Node Types (12)

  1. Element
  2. Attribute
  3. Text
  4. CDATASection
  5. EntityReference
  6. Entity
  7. ProcessingInstructions
  8. Comment
  9. Document
  10. DocumentType
  11. DocumentFragment
  12. Notation

- nodeName, tagName

```
  console.log(document.body.nodeType); // 1 (1 - 12)
  console.log(document.nodeType); // 9 (1 - 12)

  console.log(document.nodeName); // nodeName more genaric
  console.log(document.tagName); // no tag for document

  console.log(document.body.nodeName); // nodeName BODY
  console.log(document.body.tagName); // tagName BODY
```

#### Creating DOM Nodes

```
const ul = document.createElement('ul');

const li = document.createElement('li');

const listItem = document.createTextNode('injected by js');

const comment = document.createComment('testing');

document.body.append(ul);

ul.append(comment);
ul.append(li);

li.append(listItem);
```

Result:

```
<body>
  <ul>
    <!--testing-->
    <li>injected by js</li>
  </ul>
</body>
```

#### Changing contents of a DOM Element

```
// innerText(element), innerHTML, textContent(node)

secoundItem.innerText = 'using innerText'; // only visible text.

thirdItem.innerHTML = '<span class="highlight">using innerHTML</span>' // visible even when display prop set to none (markup).

fourthItem.textContent = 'using textContent' // text of a node with white spaces.
```

#### innerHTML versus createElement

- Creating nodes using createElement

```
function createInputDom({ label, type = 'text' }) {
  const labelEl = document.createElement('label');
  const inputEl = document.createElement('input');

  labelEl.innerText += label;
  inputEl.type = type;

  labelEl.append(inputEl);

  return labelEl;
}

const injectInputDom = createInputDom({ label: 'Full Name ' });

document.body.append(injectInputDom);
```

- Creating nodes using template string **(Recommended)**.

```
function createInputTemplate({ label, type = 'text' }) {
  return `
  <label>
    ${label}
    <input type=${type}>
  </label>
  `;
}

document.body.innerHTML += createInputTemplate({label: 'Email', type: 'email'})
```

#### Using documentFragments

```
const data = ['Earth', 'Fire', 'Air', 'Water'];

const fragment = document.createDocumentFragment();

data.forEach((name) => {
  const li = document.createElement('li');
  li.innerText = name;

  fragment.append(li);
});

console.dir(fragment); // nodeList [li, li, li,li]

document.body.append(fragment); // all at once
```

#### Inserting DOM Elements

1. node.append(srt/node)
2. node.prepend(srt/node)

3. node.before(srt/node)
4. node.after(srt/node)

- old way using insertBefore

1. node.parentNode.insertBefore(newNode, node)
2. node.parentNode.insertBefore(newNode, node.nextSibling)
