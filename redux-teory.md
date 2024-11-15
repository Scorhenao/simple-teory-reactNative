# Redux Fundamentals

## Ejemplo de tienda

1. **La tienda (store) es el lugar donde se guarda el estado de la aplicación**, como un "almacén central" que contiene toda la información que los componentes pueden utilizar. 

2. **Los reducers son como personas (o entidades) dentro de la tienda**, y cada uno tiene su tarea específica de manejar una parte del estado. Por ejemplo, uno puede ser responsable del estado de un contador, otro del estado de un usuario, etc.

3. **Acción (action)** es como una orden que le das a un reducer, diciendo "haz algo". La acción tiene un `type` que indica qué tipo de acción se va a realizar (en tu ejemplo, la acción se llama `ordenar`).

4. **`dispatch` es como enviar la orden a la tienda** para que los reducers hagan su trabajo y cambien el estado. Cuando el componente llama a `dispatch`, le está diciendo al reducer que realice una acción, como cambiar la variable `ordenar` de `false` a `true`.

5. **Promesa como respuesta**: Aunque Redux en su forma básica no trabaja con promesas directamente, puedes manejar operaciones asincrónicas (como peticiones a una API) usando middleware como `redux-thunk`. Si necesitas que la acción devuelva una promesa, puedes usar `redux-thunk` para manejarlo.

### Ejemplo práctico para que puedas verificar tu comprensión:

Imaginemos que tienes una tienda (store) que maneja el estado de un proceso de orden (con un estado por defecto de `false`):

#### Definición del Reducer

```typescript
const initialState = {
  ordenando: false, // Variable por defecto
};

function ordenarReducer(state = initialState, action: any) {
  switch (action.type) {
    case "ORDENAR":
      return { ...state, ordenando: true }; // Cambiar estado de 'false' a 'true'
    default:
      return state;
  }
}
```

#### Crear la tienda (store)

```typescript
import { createStore } from "redux";

// Crear el store con el reducer
const store = createStore(ordenarReducer);
```

#### Acción (Action)

```typescript
const ordenarAction = { type: "ORDENAR" };
```

#### Componente donde se hace el `dispatch` y se obtiene una promesa

```typescript
import React, { useState } from "react";
import { View, Button, Text } from "react-native";
import { useDispatch, useSelector } from "react-redux";

const OrdenarComponent = () => {
  const dispatch = useDispatch();
  const ordenando = useSelector((state: any) => state.ordenando);

  const [message, setMessage] = useState("");

  const ordenar = () => {
    // Aquí despachamos la acción 'ORDENAR'
    dispatch(ordenarAction);

    // Simulamos la respuesta de una promesa (esto es opcional)
    new Promise((resolve) => {
      setTimeout(() => {
        resolve("Orden realizada");
      }, 1000);
    })
      .then((res) => setMessage(res))
      .catch((error) => setMessage("Error al ordenar"));
  };

  return (
    <View>
      <Text>Estado de ordenar: {ordenando ? "Sí" : "No"}</Text>
      <Button title="Ordenar" onPress={ordenar} />
      <Text>{message}</Text>
    </View>
  );
};

export default OrdenarComponent;
```

### Explicación:
1. **`dispatch(ordenarAction)`**: En el componente, se despacha la acción que indica que la orden debe cambiar de `false` a `true` (en el reducer).
2. **Promesa**: Después de hacer el `dispatch`, la promesa se resuelve con un mensaje como respuesta (esto es solo un ejemplo de cómo manejar asincronía, pero Redux en sí mismo no requiere promesas para sus operaciones).
3. **Estado**: El estado `ordenando` se actualiza en el reducer, y en el componente, se muestra un mensaje dependiendo de si la orden se procesó correctamente.

### Resumen:
- La **tienda** (store) es donde guardas el estado.
- **Los reducers** son las personas encargadas de manejar el estado, y cada uno tiene una **acción** asociada que le dice qué hacer.
- **Dispatch** se usa para enviar esas acciones a los reducers.
- Aunque Redux por sí solo no devuelve promesas, puedes manejar acciones asincrónicas usando middleware como `redux-thunk`.

### Comandos de instalación

```bash
npm install redux react-redux
```

### Store

- Este es el único lugar donde se almacena el estado de la aplicación. Es el contenedor central donde se guarda toda la información compartida entre los componentes.

Ejemplo de configuración de un Store con múltiples reducers:

```typescript
import { createStore, combineReducers } from "redux";

// Reducer de contador
const initialStateCounter = { count: 0 };

function counterReducer(state = initialStateCounter, action: any) {
  switch (action.type) {
    case "INCREMENT":
      return { ...state, count: state.count + 1 };
    case "DECREMENT":
      return { ...state, count: state.count - 1 };
    default:
      return state;
  }
}

// Reducer de usuario (ejemplo adicional)
const initialStateUser = { name: "", age: 0 };

function userReducer(state = initialStateUser, action: any) {
  switch (action.type) {
    case "SET_USER":
      return { ...state, name: action.payload.name, age: action.payload.age };
    default:
      return state;
  }
}

// Combinar reducers
const rootReducer = combineReducers({
  counter: counterReducer,
  user: userReducer,
});

// Crear el store con el rootReducer
const store = createStore(rootReducer);

// Ver el estado inicial
console.log(store.getState());
```

En este ejemplo:
- **counterReducer** maneja las acciones relacionadas con un contador (`INCREMENT`, `DECREMENT`).
- **userReducer** maneja las acciones relacionadas con el usuario, como establecer su nombre y edad.

##### Que retorna el store
```typescript
{
  counter: { count: 0 },
  user: { name: "", age: 0 }
}
```

### Reducers

- Los reducers son funciones puras que toman el estado actual y una acción, y devuelven un nuevo estado.

Ejemplo de un reducer para el contador:

```typescript
const initialStateCounter = { count: 0 };

function counterReducer(state = initialStateCounter, action: any) {
  switch (action.type) {
    case "INCREMENT":
      return { ...state, count: state.count + 1 };
    case "DECREMENT":
      return { ...state, count: state.count - 1 };
    default:
      return state;
  }
}
```

### Actions

- Las acciones son objetos que describen lo que debe cambiar en el estado. Siempre deben tener un tipo (`type`).

Ejemplo de una acción para el contador y el usuario:

```typescript
const incrementAction = { type: "INCREMENT" };
const decrementAction = { type: "DECREMENT" };

// Acción para establecer un usuario
const setUserAction = {
  type: "SET_USER",
  payload: { name: "John Doe", age: 30 }
};
```

### Dispatch

- `dispatch` se usa para enviar acciones al store y actualizar el estado.

Ejemplo de uso de `dispatch`:

```typescript
store.dispatch(incrementAction); // Incrementa el contador
store.dispatch(decrementAction); // Decrementa el contador
store.dispatch(setUserAction);   // Establece el nombre y la edad del usuario
```

### Conectando con React

- Usamos `react-redux` para conectar Redux con los componentes de React.

Ejemplo de un componente conectado para mostrar el contador y el nombre del usuario:

```typescript
import React from "react";
import { View, Text, Button } from "react-native";
import { connect } from "react-redux";

const CounterAndUser = ({ count, user, dispatch }) => {
  return (
    <View>
      <Text>Contador: {count}</Text>
      <Button
        title="Incrementar"
        onPress={() => dispatch({ type: "INCREMENT" })}
      />
      <Button
        title="Decrementar"
        onPress={() => dispatch({ type: "DECREMENT" })}
      />
      <Text>Nombre de usuario: {user.name}</Text>
      <Button
        title="Establecer usuario"
        onPress={() =>
          dispatch({
            type: "SET_USER",
            payload: { name: "Jane Doe", age: 25 },
          })
        }
      />
    </View>
  );
};

const mapStateToProps = (state: any) => ({
  count: state.counter.count,
  user: state.user,
});

export default connect(mapStateToProps)(CounterAndUser);
```

### Middleware (opcional)

- Redux middleware te permite interceptar las acciones antes de que lleguen al reducer. Un middleware común es `redux-thunk` para manejar acciones asíncronas.

Ejemplo de uso con `redux-thunk`:

```bash
npm install redux-thunk
```

```typescript
import { createStore, applyMiddleware } from "redux";
import thunk from "redux-thunk";

const store = createStore(rootReducer, applyMiddleware(thunk));
```

### Flujo de Redux:

![Flujo de Redux UML](redux%20flow.png)

En este flujo:
- **Actions** son disparadas por los componentes.
- **Dispatch** envía las acciones al **Store**.
- El **Store** actualiza el **State** según los reducers, y los componentes conectados reflejan el estado actualizado.

Este flujo básico de Redux es común tanto para aplicaciones web como para aplicaciones móviles en **React Native**, solo adaptando la manera de renderizar los componentes a los elementos de React Native como `View`, `Text`, y `Button`.

---
