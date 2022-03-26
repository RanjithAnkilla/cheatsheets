<center> <h2>JavaScript [DOM]</h2></center>

- [DOM loading events](#dom-loading-events)
- [JavaScript Loading](#javascript-loading)
- [Nodes In-Depth](#nodes-in-depth)
  * [Creating DOM Nodes](#creating-dom-nodes)
  * [Changing contents of a DOM Element](#changing-contents-of-a-dom-element)
  * [innerHTML versus createElement](#innerhtml-versus-createelement)
  * [Using documentFragments](#using-documentfragments)
  * [Inserting DOM Elements](#inserting-dom-elements)
  * [insertAdjacentHTML, insertAdjacentText, insertAdjacentElement](#insertadjacenthtml-insertadjacenttext-insertadjacentelement)
  * [Replacing Element](#replacing-element)
  * [Cloning Node Tree](#cloning-node-tree)
  * [Removing Nodes](#removing-nodes)
- [Querying and Traversing the DOM](#querying-and-traversing-the-dom)
  * [Querying DOM Nodes [ HTMLCollections ]](#querying-dom-nodes--htmlcollections-)
  * [Querying DOM Nodes [ NodeList ]](#querying-dom-nodes--nodelist-)
  * [Itterating through DOM Elements.](#itterating-through-dom-elements)
  * [Finding Child Elements](#finding-child-elements)
  * [Finding Parent Element](#finding-parent-element)
  * [Finding Sibling Elements](#finding-sibling-elements)
- [Attributes, Styles and Classes.](#attributes-styles-and-classes)
  * [Element Properties versus HTML Attributes](#element-properties-versus-html-attributes)
  * [Setting and Getting HTML Attributes](#setting-and-getting-html-attributes)
  * [Setting and Getting Inline Styles](#setting-and-getting-inline-styles)
  * [Setting and Getting Classes](#setting-and-getting-classes)
- [Events And Event Listeners](#events-and-event-listeners)
  * [Adding Event Listeners](#adding-event-listeners)
  * [Remove Event Listeners](#remove-event-listeners)
  * [Capturing, Bubbling and Propagation.](#capturing-bubbling-and-propagation)
  * [Prevent Default Event Actions.](#prevent-default-event-actions)
  * [Event Delegation and Dynamic Events](#event-delegation-and-dynamic-events)
  * [Keyboard Events](#keyboard-events)
- [Forms and Events](#forms-and-events)
  * [Accessing Forms and Elements](#accessing-forms-and-elements)
  * [Form Submit Event and FormData](#form-submit-event-and-formdata)
  * [Transforming formData for the Server.](#transforming-formdata-for-the-server)
  * [Posting Form Data via Fetch Api](#posting-form-data-via-fetch-api)
  * [Handling Input Elements](#handling-input-elements)
  * [Handling Radio Input Elements](#handling-radio-input-elements)
  * [Handling Checkbox Input Elements](#handling-checkbox-input-elements)
  * [Handling Select Input Elements](#handling-select-input-elements)

<small><i><a href='http://ecotrust-canada.github.io/markdown-toc/'>Table of contents generated with markdown-toc</a></i></small>

---

### DOM loading events

- `document.addEventListiner('DOMContentLoaded', () => {});`
  - triggers after rendering engine parses html.
- `window.addEventListiner('load', () => {});`
  - triggers after a page load.

---

### JavaScript Loading

- defer attribute.
  - `<script defer src="/path.js"></script>`
  - fetches script and parses just before DOMContentLoaded.
- async attribute.
  - `<script async src="/path.js"></script>`
  - fetches script and parses asap.

---

### Nodes In-Depth

- document

```javascript
console.dir('document', document);
```

- html

```javascript
console.log('html', document.documentElement);
```

- head

```javascript
console.log('head', document.head);
```

- body

```javascript
console.log('body', document.body);
```

- retrieve constructor (returns the function that is used to create) name

```javascript
console.log(document.body.constructor.name); // HTMLBodyElement
```

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

// nodeName for any Node types
console.log(document.nodeName); // nodeName more genaric - '#document'

// tagName for any Element types
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
// innerText - elements, innerHTML, textContent - nodes | [Getters & Setters]

secoundItem.innerText = 'using innerText'; // only visible text.

thirdItem.innerHTML = '<span class="highlight">using innerHTML</span>'; // visible even when display property set to none.

fourthItem.textContent = 'using textContent'; // text of a node with white spaces.

console.log(thirdItem.innerText); // using innerHTML
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

document.body.innerHTML += createInputTemplate({
  label: 'Email',
  type: 'email',
});
```

#### Using documentFragments

```javascript
const data = ['Earth', 'Fire', 'Air', 'Water'];

const fragment = document.createDocumentFragment(); // or new DocumentFragment();

data.forEach((name) => {
  const li = document.createElement('li');
  li.innerText = name;

  fragment.append(li);
});

console.dir(fragment); // nodeList [li, li, li, li]

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

newDiv = div.cloneNode(); // cloneNode(false) only clones top element "<div></div>"

newDiv = div.cloneNode(true); // clones all elements and subtrees

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

### Querying and Traversing the DOM

#### Querying DOM Nodes [ HTMLCollections ]

1. getElementById
2. getElementsByName
3. getElementsByClassName - HTMLCollection
4. getElementsByTagName - HTMLCollection

```javascript
const data = ['Earth', 'Fire', 'Water'];

const fragment = document.createDocumentFragment();

const createEle = (e) => document.createElement(e);

data.forEach((e) => {
  const li = createEle('li');

  li.classList.add('list-item');
  li.innerText = e;
  fragment.append(li);
});

// getElementById - HTMLElement
const ulFromId = document.getElementById('list');
ulFromId.append(fragment);

// getElementsByClassName - HTML Collection
const listItemsFromClassName = ulFromId.getElementsByClassName('list-item');
console.log(listItemsFromClassName); // length: 3

// getElementsByTagName - HTML Collection
const listItemsFromTagName = ulFromId.getElementsByTagName('li');
console.log(listItemsFromTagName); // length: 3

// Demonstrate live collection
const newListItem = createEle('li');
newListItem.className = 'list-item';
newListItem.innerText = 'Air';
ulFromId.append(newListItem);

// live collection
console.log(listItemsFromTagName); // length: 4
console.log(listItemsFromClassName); // length: 4
```

#### Querying DOM Nodes [ NodeList ]

1. querySelector('css selector')
2. querySelectorAll('css selector') - NodeList

```javascript
const data = ['Earth', 'Fire', 'Water'];

const fragment = document.createDocumentFragment();

const createEle = (e) => document.createElement(e);

data.forEach((e) => {
  const li = createEle('li');

  li.classList.add('list-item');
  li.innerText = e;
  fragment.append(li);
});

// querySelector
const ulFromQuerySelector = document.querySelector('#list');
ulFromQuerySelector.append(fragment);

// querySelectorAll - NodeList (static)
const listItemFromQSA = ulFromQuerySelector.querySelectorAll('.list-item');
console.log(listItemFromQSA); // NodeList length: 3

const newListItem = createEle('li');
newListItem.className = 'list-item';
newListItem.innerText = 'Air';
ulFromQuerySelector.append(newListItem);

console.log(listItemFromQSA); // NodeList length: 3

console.log(ulFromQuerySelector.querySelectorAll('.list-item')); // NodeList length: 4
```

#### Itterating through DOM Elements.

```javascript
const data = ['Earth', 'Fire', 'Water', 'Air'];

const fragment = document.createDocumentFragment();

const createEle = (e) => document.createElement(e);

data.forEach((e) => {
  const li = createEle('li');

  li.classList.add('list-item');
  li.innerText = e;
  fragment.append(li);
});

const ulFromQuerySelector = document.querySelector('#list');
ulFromQuerySelector.append(fragment);

const listItemFromQSA = ulFromQuerySelector.querySelectorAll('.list-item');
console.log(listItemFromQSA);

for (let i = 0; i < listItemFromQSA.length; i++)
  console.log(listItemFromQSA[i].innerHTML);

for (let item of listItemFromQSA) console.log(item.innerHTML);

[...listItemFromQSA].forEach((item) => console.log(item.innerHTML));

Array.from(listItemFromQSA).forEach((item) => console.log(item.innerText));
```

#### Finding Child Elements

```javascript
const ulFromId = document.getElementById('list');

ulFromId.innerHTML = `
<li class='list-item'>Earth</li>
<li class='list-item'>Water</li>
<li class='list-item'>Fire</li>
<li class='list-item'>Air</li>
`;

// querySelector - NodeList
const queryChildren = ulFromId.querySelectorAll('li');
console.log(queryChildren);

// children - HTMLCollaction
const liChildren = ulFromId.children;
console.log(liChildren);

// childNodes - NodeList with text nodes (\n)
const liNodeChildren = ulFromId.childNodes;
console.log(liNodeChildren, liNodeChildren[0]); // [...], #text node(\n)

/*
 * node.firstChild (childNode), parentNode.firstElementChild (element)
 * lastChild, lastElementChild
 */
console.log(ulFromId.firstChild, ulFromId.firstElementChild); // #text '\n', <li>Earth<li>
console.log(ulFromId.lastChild, ulFromId.lastElementChild); // #text '\n', <li>Air<li>
```

#### Finding Parent Element

```javascript
console.log(document.body.parentNode); // Any DOM Nodes - html
console.log(document.body.parentElement); // Any Elements Nodes - html

console.log(document.querySelector('.list-item').closest('body')); // Closest Parent Node
```

#### Finding Sibling Elements

```javascript
const ulFromId = document.getElementById('list');

ulFromId.innerHTML = `
<li class='list-item'>1</li>
<li class='list-item'>2</li>
<li class='list-item'>3</li>
<li class='list-item'>4</li>
`;

const firstLi = ulFromId.querySelector('.list-item');

// Any DOM Nodes
console.log(firstLi.nextSibling); // #text '\n'(node)
console.log(firstLi.previousSibling); // #text '\n'(node)

// Any Element Nodes
console.log(firstLi.nextElementSibling); // <li>2</li>
console.log(firstLi.previousElementSibling); // null
```

### Attributes, Styles and Classes.

#### Element Properties versus HTML Attributes

```javascript
const newInput = document.createElement('input');

// element property
newInput.type = 'text';

newInput.value = 2;
// setAttribute()
// newInput.setAttribute('value', 2)

document.body.append(newInput);

console.log(parseInt(newInput.value, 10));
```

#### Setting and Getting HTML Attributes

```javascript
const exitBtn = document.querySelector('button');

// SET Attribute
exitBtn.setAttribute('aria-label', 'close this modal');
console.log(exitBtn);

console.log(exitBtn['aria-label']); // undefined

// GET Attribute
console.log(exitBtn.getAttribute('aria-label')); // "close this modal"

// GET Attribute with .attributes
console.log(exitBtn.attributes['aria-label']);
```

#### Setting and Getting Inline Styles

```javascript
const btn = document.querySelector('button');

console.log(btn.style);

// Setting Inline Styles using .cssText
btn.style.cssText = `
color: dodgerblue; padding: .2rem .3rem;
`;

// Setting Inline Styles using direct property
btn.style.color = 'coral';
btn.style.backgroundColor = '#333';

// Getting Styles
console.log(btn.style.color);
```

#### Setting and Getting Classes

```javascript
const app = document.querySelector('#app');

app.innerHTML = `
  <button class='one two'>EXIT</button>
`;

const btn = document.querySelector('button');

// Old way: Set
btn.className += ' three';

// Old way: Get
console.log(btn.className.split(' '));

// New way: Set
btn.classList.add('four');

// New way: Get
console.log(btn.classList);

// Toggle class
setTimeout(() => {
  btn.classList.toggle('four');
}, 2000);

// Remove class
btn.classList.remove('three');
```

### Events And Event Listeners

#### Adding Event Listeners

```javascript
const app = document.getElementById('app');

app.innerHTML = `
  <button type='button'>EXIT</button>
`;

const btn = document.querySelector('button');

// Avoid onevents (overrides)
btn.onpointerenter = function () {
  // destroyed
  this.style.color = 'coral';
};

btn.onpointerenter = function () {
  this.style.backgroundColor = 'coral';
};

function handleEvent(event) {
  console.log(this, event.target, event.target.style.backgroundColor); // this --> btn
}

const handlePointerEvent = (e) => {
  console.log(this, e.target); // this --> Window
  e.target.style.backgroundColor = '#333';
};

// addEventListener
btn.addEventListener('click', handleEvent);
btn.addEventListener('click', handleEvent); // does not allow dublicate callbacks

btn.addEventListener('pointerleave', handlePointerEvent);
```

#### Remove Event Listeners

```javascript
const app = document.getElementById('app');

app.innerHTML = `
  <button type='button'>EXIT</button>
`;

const btn = document.querySelector('button');

function handleClick(event) {
  console.log(event.target); // <button></button> (once)
  btn.removeEventListener('click', handleClick); // recursive
}

// btn.addEventListener('click', handleClick, { once: true });

// addEventListener
btn.addEventListener('click', handleClick); // function must be referenced to remove event

setTimeout(() => {
  btn.removeEventListener('click', handleClick);
}, 3000);
```

#### Capturing, Bubbling and Propagation.

```javascript
const app = document.getElementById('app');

app.innerHTML = `
  <div class='one'>
    <div class='two'>
      <button class='three' type='button'>EXIT</button>
    </div>
  </div>
`;

const one = document.querySelector('.one');
const two = document.querySelector('.two');
const three = document.querySelector('.three');

function handleClick(event) {
  console.log(event.target);
  // event.stopPropagation();
  event.stopImmediatePropagation();
}

one.addEventListener('click', handleClick, false); // Capturing(def - false) - Target  - Bubbling(def - true)
two.addEventListener('click', handleClick, true); // capturing(true)

three.addEventListener('click', handleClick);

three.addEventListener('click', () => console.log('clicked')); // stoped by stopImmediatePropagation()
```

#### Prevent Default Event Actions.

```javascript
const app = document.getElementById('app');

app.innerHTML = `
  <form>
    <label>Sign-up Email</label>
    <input type="email">
    <label>I agree to the terms</label>
    <input type="checkbox">
  </form>
`;

const form = document.querySelector('form');
const email = document.querySelector('input[type="email"]');
const checkbox = document.querySelector('input[type="checkbox"]');

function handleSubmit(event) {
  event.preventDefault();
  const emailValue = email.value;
  console.log(
    emailValue.match(/.+@gmail\.com$/) && checkbox.checked
      ? 'Submitted'
      : 'Submission Failed'
  );
  console.log(event.defaultPrevented); // true in this case
}

form.addEventListener('submit', handleSubmit);
```

#### Event Delegation and Dynamic Events

```javascript
const app = document.getElementById('app');

app.innerHTML = `
  <button type='button'>+</button>
  <ul class='list'>
    <li>Item 1</li>
    <li>Item 2</li>
    <li>Item 3</li>
    <li>Item 4</li>
  </ul>
`;

const btn = document.querySelector('button');
const list = document.querySelector('.list');
// const listItems = [...list.querySelectorAll('li')];

function handleClick(e) {
  if (e.target.nodeName.toLowerCase() === 'ul') return;
  console.log(e.target.innerText);
}

// listItems.forEach((item) => {
//   item.addEventListener('click', handleClick);
// });

list.addEventListener('click', handleClick); // Bubbling

btn.addEventListener('click', () => {
  const li = document.createElement('li');
  list.append(li);

  const items = list.querySelectorAll('li');

  li.innerHTML = `Item ${items.length}`;
});
```

#### Keyboard Events

```javascript
document.body.style.height = '300vh';

document.addEventListener('keydown', (e) => {
  switch (e.key) {
    case 'ArrowUp':
      e.preventDefault();
      console.log('UP');
      break;
    case 'ArrowDown':
      e.preventDefault();
      console.log('DOWN');
  }
});

window.addEventListener('keyup', (e) => {
  console.log(e.key, e.code);
});

document.addEventListener('keydown', (e) => {
  console.log(e.key, e.code);
});
```

### Forms and Events

#### Accessing Forms and Elements

```javascript
const app = document.getElementById('app');
app.innerHTML = `
  <form name='order'>
    <label>
      Full Name
      <input type='text' name='fullname'>
    </label>
  </form>
`;

const form = document.forms.order; // HTMLCollection
const fullname = form.elements.fullname; // HTMLFormControlsCollection

function handleInput(event) {
  // Access the value
  console.log(event.target.value);

  // Access the Form and Elements
  console.log(event.target.form);
  console.log(event.target.form.elements);
}

fullname.addEventListener('input', handleInput);
```

#### Form Submit Event and FormData

```javascript
const app = document.getElementById('app');

app.innerHTML = `
  <form name='order'>
    <label>
      Full Name
      <input type='text' name='fullname'>
    </label>
    <label>
      Fruits
      <select name='fruits'>
        <option name='apple'>Apple</option>
        <option name='banana'>Banana</option>
        <option name='orange'>Orange</option>
      </select>
    </label>
    <button type='submit'>SUBMIT</button>
  </form>
`;

const order = document.forms.order;

function handleSubmit(event) {
  event.preventDefault();
  // console.log([...new FormData(order)]); // [[...],[...]]
  console.log(new FormData(order)); // emits a formdata event
}

function handleFormData(event) {
  console.log([...event.formData]);
  const entries = event.formData.entries();
  for (let entry of entries) console.log(entry);
}

order.addEventListener('submit', handleSubmit);
order.addEventListener('formdata', handleFormData); // capture formdata event
```

#### Transforming formData for the Server.

```javascript
const app = document.getElementById('app');

app.innerHTML = `
  <form name='order'>
    <label>
      Full Name
      <input type='text' name='fullname'>
    </label>
    <label>
      Which pizza do you like?
      <select name='pizza'>
        <option name='meaty'>Meaty</option>
        <option name='pepperoni'>Pepperoni</option>
        <option name='cheesey'>Cheesey</option>
      </select>
    </label>
    <div>
      <p>What Size?</p>
      <label>
        Small
        <input type='radio' value='small' name='size' checked>
      </label>
      <label>
        Medium
        <input type='radio' value='medium' name='size'>
      </label>
      <label>
        Large
        <input type='radio' value='large' name='size'>
      </label>
    </div>
    <label>
      Quantity
      <input type='number' name='quantity'>
    </label>
    <button type='submit'>SUBMIT</button>
  </form>
`;

const order = document.forms.order;

function handleSubmit(event) {
  event.preventDefault();
  const formData = new FormData(event.target);

  // Query String
  // Header - Content-Type = application/x-www-form-urlencode

  // fullname=first%20last&pizza=meaty&size=large&quantity=2
  // const data = [...formData.entries()];
  // const asString = data
  //   .map(entry => `${encodeURIComponent(entry[0])}=${encodeURIComponent(entry[1])}`)
  //   .join('&');

  // fullname=first+last&pizza=meaty&size=large&quantity=2
  const asString = new URLSearchParams(formData).toString();
  console.log(asString);

  // JSON
  // Header - Content-Type = application/json
  const asJson = JSON.stringify(Object.fromEntries(formData));
  console.log(asJson);
}

order.addEventListener('submit', handleSubmit);
```

#### Posting Form Data via Fetch Api

```javascript
const order = document.forms.order;

function handleSubmit(event) {
  event.preventDefault();
  const formData = new FormData(event.target);

  // Query String
  const asString = new URLSearchParams(formData).toString();
  console.log(asString);

  // JSON
  const asJson = JSON.stringify(Object.fromEntries(formData));
  console.log(asJson);

  fetch('/api', {
    method: 'post',
    headers: {
      // 'Content-Type': 'application/x-www-from-urlencode',
      'Content-Type': 'application/json',
    },
    body: asJson,
  });
}

order.addEventListener('submit', handleSubmit);
```

#### Handling Input Elements

```javascript
const app = document.getElementById('app');

app.innerHTML = `
  <form name='example'>
    <input type='text' name='exinput'>
  </form>
`;

const form = document.forms.example;

const input = form.exinput;
// const input = form.elements.exinput;

// 1) Properties
console.log(input);

// set
input.value = 'hello';
// input.readOnly = true;
// input.disabled = true;

// get
console.log(input.value, input.textLength);

// 2) Events
input.addEventListener('focus', () => console.log('focus'));
input.addEventListener('blur', () => console.log('blur'));
input.addEventListener('input', () => console.log('input'));
input.addEventListener('change', () => console.log('change'));

// 3) Methods
input.focus();

setTimeout(() => {
  input.blur();
}, 2500);
```

#### Handling Radio Input Elements

```javascript
const app = document.getElementById('app');

app.innerHTML = `
  <form name='example'>
    <div class='ex'>
      <label>
        Red
        <input type='radio' name='color' value='red'>
      </label>
      <label>
        blue
        <input type='radio' name='color' value='blue' checked>
      </label>
      <label>
        green
        <input type='radio' name='color' value='green'>
      </label>
    </div>
  </form>
`;

const form = document.forms.example;

const radios = [...form.elements.color];

// 1) Properties
console.log(radios[0]);

// set
radios[0].checked = true;

//  get
console.log(radios[0].value, radios[0].checked);

// 2) Events
const div = document.querySelector('.ex');

div.addEventListener('change', () => {
  // const checked = radios.find(radio => radio.checked);
  // console.log(checked.value);

  console.log(form.elements.color.value);
});

// 3) Methods
radios[2].select();
```

#### Handling Checkbox Input Elements

```javascript
const app = document.getElementById('app');

app.innerHTML = `
  <form name='example'>
    <label>
      Accept Terms
      <input type='checkbox' name='terms' value='yes'>
    </label>
    <input type='text' name='test'>
  </form>
`;

const form = document.forms.example;

const checkbox = form.elements.terms;

// 1) Properties
console.log(checkbox);

// set
checkbox.checked = true;

// get
console.log(checkbox.checked, checkbox.value);

// 2) Events
checkbox.addEventListener('change', () => {
  console.log(checkbox.checked);
});

// 3) Methods
checkbox.select();

const formData = [...new FormData(form).entries()];
console.log(Object.fromEntries(formData));
```

#### Handling Select Input Elements

```javascript
const app = document.getElementById('app');

app.innerHTML = `
  <form name='example'>
    <select name='drinks'>
      <option value=''>--- Select Your Drink ---</option>
      <option value='lemonade' selected>Lemonade</option>
      <option value='cola'>Cola</option>
      <option value='water'>Water</option>
  </form>
`;

const form = document.forms.example;

const select = form.elements.drinks;

console.log(select);

// 1) Selected Value
console.log(select.value); // lemonade
select.value = 'water';
console.log(select.value); // water

// 2) Selected Index
console.log(select.selectedIndex);
const id = 2;
select.selectedIndex = id;
console.log(select.selectedIndex);

// 3) Selected Dom Element
console.log(select.options[select.selectedIndex]);
console.log(select.selectedOptions[0]); // HTMLCollection

// 4) Events
select.addEventListener('change', () => {
  console.log(select.value);
  console.log(select.selectedIndex);
  console.log(select.options[select.selectedIndex]);
});

// 5) Adding Options to Select
const newOption = document.createElement('option');
newOption.value = 'milk';
// newOption.innerText = 'Milk';
newOption.text = 'Milk';

// select.append(newOption);
select.add(newOption, 1);
```
