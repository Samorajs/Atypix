# Atypix Framework

Atypix is a modern JavaScript framework that offers a **different syntax** compared to traditional JSX-based frameworks. It uses a **function-based, declarative approach** inspired by Jetpack Compose, making it easier to build dynamic and reactive user interfaces.

## Key Features

- **Function-Based Components**: Write components as functions with a clean, declarative syntax.
- **State Management**: Built-in `state` for managing component state (similar to React's `useState`).
- **Side Effects**: Use `useEffect` to handle side effects like data fetching or subscriptions.
- **Form Handling**: Pre-built form components (`Form`, `Field`, `Label`, etc.) for easy form creation.
- **Data Fetching**: A `Fetcher` utility for making API requests.
- **Component Library**: Includes reusable components like `View`, `Divider`, `Navs`, `ListItems`, and more.

---

## Installation

To use Atypix, install it via npm:

```bash
npm install atypix
```

---

## Usage

### Basic Example

Here’s a simple example of how to use Atypix to create a component:

```javascript
import { state, useEffect } from 'atypix';
import { View, Fetcher } from 'atypix/components';

function App() {
  const [data, setData] = state([]);
  const [loading, setLoading] = state(true);

  useEffect(() => {
    Fetcher('https://jsonplaceholder.typicode.com/posts', (response, error) => {
      if (error) {
        console.error('Error fetching data:', error);
      } else {
        setData(response.data);
        setLoading(false);
      }
    });
  }, []);

  return View({
    type: 'div',
    children: [
      loading
        ? View({ type: 'div', children: 'Loading...' })
        : data.map((item) =>
            View({
              type: 'div',
              key: item.id,
              children: [
                View({ type: 'h2', children: item.title }),
                View({ type: 'p', children: item.body }),
              ],
            })
          ),
    ],
  });
}

export default App;
```

---

### Fetching Data with `Fetcher`

The `Fetcher` utility is used to make API requests. Here's an example:

```javascript
import { Fetcher } from 'atypix/components';

function fetchData() {
  Fetcher('https://jsonplaceholder.typicode.com/posts', (response, error) => {
    if (error) {
      console.error('Error fetching data:', error);
    } else {
      console.log('Data fetched successfully:', response.data);
    }
  });
}
```

---

### Managing State

Atypix provides a `state` function for managing component state. Here's an example of a counter component:

```javascript
import { state } from 'atypix';
import { View } from 'atypix/components';

function Counter() {
  const [count, setCount] = state(0);

  return View({
    type: 'div',
    children: [
      View({ type: 'p', children: `Count: ${count}` }),
      View({
        type: 'button',
        children: 'Increment',
        onclick: () => setCount(count + 1),
      }),
    ],
  });
}
```

---

### Handling Forms

Atypix provides form components like `Form`, `Field`, and `Label` for easy form creation. Here's an example:

```javascript
import { state } from 'atypix';
import { Form, Field, Label } from 'atypix/components';

function MyForm() {
  const [formData, setFormData] = state({ name: '', email: '' });

  const handleSubmit = (event) => {
    event.preventDefault();
    console.log('Form Data:', formData);
  };

  return Form({
    onsubmit: handleSubmit,
    children: [
      Label({ anchorTo: 'name', children: 'Name' }),
      Field({
        id: 'name',
        name: 'name',
        value: formData.name,
        oninput: (e) => setFormData({ ...formData, name: e.target.value }),
      }),
      Label({ anchorTo: 'email', children: 'Email' }),
      Field({
        id: 'email',
        name: 'email',
        type: 'email',
        value: formData.email,
        oninput: (e) => setFormData({ ...formData, email: e.target.value }),
      }),
      View({ type: 'button', children: 'Submit', attrs: { type: 'submit' } }),
    ],
  });
}
```

---

### Using the `View` Component

The `View` component is a versatile function for creating any HTML element. Here's an example:

```javascript
import { View } from 'atypix/components';

function MyComponent() {
  return View({
    type: 'div',
    className: 'container',
    style: { backgroundColor: '#f0f0f0' },
    children: [
      View({ type: 'h1', children: 'Hello, Atypix!' }),
      View({ type: 'p', children: 'This is a simple example of using the View component.' }),
    ],
  });
}
```

---

### Using the `Divider` Component

The `Divider` component is used to create a line break or horizontal rule:

```javascript
import { View, Divider } from 'atypix/components';

function MyComponent() {
  return View({
    type: 'div',
    children: [
      View({ type: 'p', children: 'This is some text.' }),
      Divider({ type: 'hr' }),
      View({ type: 'p', children: 'This is some more text.' }),
    ],
  });
}
```

---

### Component Library Overview

Here’s a quick overview of the components available in Atypix:

- **`View`**: A function for creating any HTML element.
- **`Form`**: A function for creating forms.
- **`Field`**: A function for creating input fields.
- **`Label`**: A function for creating labels.
- **`Divider`**: A function for creating line breaks or horizontal rules.
- **`Fetcher`**: A utility for making API requests.
- **`Navs`**: A function for creating navigation menus.
- **`ListItems`**: A function for creating lists.

---

## Example: CRUD Application

Here’s an example of a simple CRUD application using Atypix:

```javascript
import { state, useEffect } from 'atypix';
import { View, Form, Field, Fetcher } from 'atypix/components';

function CrudApp() {
  const [items, setItems] = state([]);
  const [newItem, setNewItem] = state('');

  useEffect(() => {
    Fetcher('https://jsonplaceholder.typicode.com/todos', (response, error) => {
      if (error) {
        console.error('Error fetching data:', error);
      } else {
        setItems(response.data);
      }
    });
  }, []);

  const addItem = (event) => {
    event.preventDefault();
    setItems([...items, { id: items.length + 1, title: newItem }]);
    setNewItem('');
  };

  return View({
    type: 'div',
    children: [
      Form({
        onsubmit: addItem,
        children: [
          Field({
            value: newItem,
            oninput: (e) => setNewItem(e.target.value),
            placeholder: 'Add a new item',
          }),
          View({ type: 'button', children: 'Add', attrs: { type: 'submit' } }),
        ],
      }),
      View({
        type: 'ul',
        children: items.map((item) =>
          View({ type: 'li', key: item.id, children: item.title })
        ),
      }),
    ],
  });
}

export default CrudApp;
```

---

## Conclusion

Atypix is designed to make building modern web applications easier and more intuitive. With its function-based components, reactive state management, and comprehensive utility functions, Atypix provides a powerful yet simple way to build dynamic user interfaces.

For more detailed documentation and examples, check out the official Atypix repository.

---

This documentation is tailored to your function-based component structure. Let me know if you need further adjustments!
