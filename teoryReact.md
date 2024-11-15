
# Dom Virtual
- Basicamente es una manera de cargaar dom en el lado del servidor o el codigo para luego enviarlo al dom de html

# Componentes
- Fragmentos de html,css y js o ts que forman en un solo archivo un codigo que consume toda la logica de la aplicacion 
- Todos deben tener id


# Estados

***Hooks*** => variable que lanza cambios de estado

- useState siempre que un cambio no se genere mayormente es por el useState
- useEffect se usa con [] evita dependencia circular
- useCallback para evitar llamados constantes a renderizar componentes, memoriza funciones y la hace 1 vez, eficiencia (solo ejecuta 1 cosa, no cargarlo de mas funciones)
- useMemo para memorizar variables
- useRef dependizar valor de un componente a otro
- useContext crear un estado general y consumirlo

# Ciclo de vida
- Antes, mensaje antes de cargar una pantalla o un loading
    - Use Efect
- Durante cargar un tablero cuando se entre al home
    - 
- Despues cargar un mensaje de cerrar sesion 
    - 

# Hook
- propios de react, efectos, estados
- customizado 

# Special components
### Scroll y seccionamiento
- scrollview(high order component) "recive una lista"
- flatListScreen (es un scrow view pero con otras funciones)"La data debe tener arrays, el key es el id de la data, renderItem es un componente"
- ItemSeparatorComponent(genera un peradador global para secciones de un componente)
- SectionListScreen (es un scrow view pero con otras funciones)"La data debe tener arrays, el key es el id de la data, renderItem es un componente, renderSectionHeader agrupa en secciones, stickySectionHeaders, onEndReachedThreshold scroll infinito o renderiza todo mientras haces scroll"
- No uses el scrollview, use el flatlist o sectionlist juntos en mismo codigo
- SafeAreaProvider, SafeAreaView(usa un margin de seguridad para los noch de ios)
### Botones (high order components)
- TouchablesScreen (botones con hover al press)"activeOpacity es el porcentaje de opacidad"
- TouchableHighlight(hover ligero)
- TouchableWithoutFeedback(no tiene hover)

### Animaciones
- Animated(trae todos los tipos de animaciones de css)

# Async storage(litralmente local storage)