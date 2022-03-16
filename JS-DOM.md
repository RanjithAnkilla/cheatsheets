<center> <h2>JavaScript [DOM]</h2></center>

---

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
  - `<script async src="/path.js"></script>`
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

```javascript
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

- nodeType, nodeName, tagName

```javascript
console.log(document.body.nodeType); // 1 (1 - 12)
console.log(document.nodeType); // 9 (1 - 12)

console.log(document.nodeName); // nodeName more genaric - '#document'
console.log(document.tagName); // no tag for document - undefined

console.log(document.body.nodeName); // BODY
console.log(document.body.tagName); // BODY
```

#### Creating DOM Nodes

```javascript
const ul = document.createElement('ul');

const li = document.createElement('li');

const listText = document.createTextNode('injected by js');

const comment = document.createComment('testing');

document.body.append(ul);

ul.append(comment);
ul.append(li);

li.append(listText);
```

Result:

```html
<body>
  <ul>
    <!--testing-->
    <li>injected by js</li>
  </ul>
</body>
```

#### Changing contents of a DOM Element

```javascript
// innerText(element), innerHTML, textContent(node)

secoundItem.innerText = 'using innerText'; // only visible text.

thirdItem.innerHTML = '<span class="highlight">using innerHTML</span>'; // visible even when display prop set to none (markup).

fourthItem.textContent = 'using textContent'; // text of a node with white spaces.
```

#### innerHTML versus createElement

- Creating nodes using createElement

```javascript
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

```javascript
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

```javascript
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

#### insertAdjacentHTML, insertAdjacentText, insertAdjacentElement

```javascript
const ul = document.querySelector('.list');

ul.insertAdjacentHTML('beforebegin', '<p>Before</p>');
ul.insertAdjacentHTML('afterbegin', '<li>First</li>');
ul.insertAdjacentHTML('beforeend', '<li>Last</li>');
ul.insertAdjacentHTML('afterend', '<p>After</p>');
```

Result:

```html
<p>Before</p>
<ul class="list">
  <li>First</li>
  <li>Last</li>
</ul>
<p>After</p>
```

#### Replacing Element

```html
<div id="ele"></div>
```

```javascript
const div = document.querySelector('#ele');
div.innerHTML = `<p>Replace Me!</p>`;

const newDiv = document.createElement('div');
newDiv.innerText = 'Replaced Element';

const anotherDiv = document.createElement('div');
anotherDiv.innerText = 'Another Replaced Element';

div.replaceWith(newDiv, anotherDiv);
```

Result:

```html
<div>Replaced Element</div>
<div>Another Replaced Element</div>
```

- Old Way

```javascript
div.parentNode.replaceChild(newDiv, div); // <div>Replaced Element</div>
```

#### Cloning Node Tree

```javascript
const div = document.querySelector('#ele');
div.innerHTML = `<p>Clone Me!</p>`;

let newDiv = document.createElement('div');

// cloneNode(deep? : boolean)

newDiv = div.cloneNode(); // clones top-level element "<div></div>"

newDiv = div.cloneNode(true); // clones complete node tree

document.body.append(newDiv);
```

#### Removing Nodes

```javascript
const div = document.querySelector('#ele');
div.innerHTML = `<p>12! New Message</p>`;

div.classList.add('notify');

setTimeout(() => {
  div.remove();
}, 2 * 1000);
```

- Old Way

```javascript
setTimeout(() => {
  div.parentNode.removeChild(div);
}, 2 * 1000);
```
