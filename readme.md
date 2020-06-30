## Extends Vue Formulate

### Features for FormulateForm (Generated Forms)

1. Events declaration within the `schema` using `on` listeners object

```js
// schema
[{
  component: 'div',
  children: 'Click me',
  on: {
    click(el) {
      console.log(You clicked the following element, el)
    },
  },
}]
```

2. Events propagation with `@events`

```js
// schema
;[
  {
    component: 'div',
    class: 'form-buttons',
    children: 'Click me',
    events: ['click'],
  },
]
```

```js
// vue - js
const eventsHandler = (event) => {
  if(event.name === "click" &&  event.element.classList.includes('form-buttons')){
    console.log(You clicked the following element, event.element)
  }
}
```

```html
<!-- vue - template -->
<FormulateForm :schema="schema" @events="eventsHandler"></FormulateForm>
```

3. Hooks on Node (`nodeHook`) and Component (`componentHook`) creation
```js
const nodeHook = (el) => {
  if(el.component === 'FormulateInput') {
    // This example replaces the outer-class definition
    el.definition.attrs = {...el.definition.attrs, 'outer-class':'px-6 py-3'}
  }
  return el
}
```

```js
// Dumb example which let's you dynamically wrap any div node
const componentHook = (node) => {
  if(node.component === "div") {
    return h('div', { attrs: { class: 'wrapper'}}, [
      h('div', "Before"),
      h(node.component, node.definition, node.children),
      h('div', "After"),
    ])
  } else {
    return h(node.component, node.definition, node.children)
  }
}
```

```html
<FormulateForm
  :formulateValue="value"
  @input="payload => $emit('input',  payload)"
  :nodeHook="nodeHook"
  :componentHook="componentHook"
  :schema="schema"
/>
```