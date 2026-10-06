# Geometry Dash OO — Proyecto de clase

Juego tipo *Geometry Dash* hecho en **C#** con **Raylib-cs**, construido a partir
de una jerarquía de clases (`Objeto` → `Caja` → `Jugador` / `Obstaculo`) siguiendo
el paradigma **Orientado a Objetos**.

## Integrantes

- Orduño Moreno Jose Eduardo
- Franyutti Ricardez Jose Jesús

## Descripción del juego

Un cubo azul salta obstáculos que avanzan hacia él de derecha a izquierda. Si
toca uno, la partida termina y aparece un cartel con el puntaje obtenido;
presionando **ESPACIO** se reinicia.

### Controles

| Tecla     | Acción |
|-----------|--------|
| `ESPACIO` | Saltar / reiniciar tras perder |

## Estructura de clases

```
Objeto (abstracta)
  └── Caja
        ├── Jugador     (gravedad y salto)
        └── Obstaculo   (se mueve hacia la izquierda y se recicla)
```

- **`Objeto`**: clase base abstracta. Define posición, velocidad y color, y
  obliga a sus clases hijas a implementar `Draw`, `Update`, `GetArea` y
  `CollisionWith`.
- **`Caja`**: implementa un rectángulo con ancho y largo. Sabe dibujarse,
  moverse según su velocidad y detectar colisión con otra `Caja`
  (`Raylib.CheckCollisionRecs`).
- **`Jugador`**: hereda de `Caja`. Sobreescribe `Update` para aplicar gravedad
  y responder al salto con `ESPACIO` mientras está en el suelo.
- **`Obstaculo`**: hereda de `Caja`. Sobreescribe `Update` para avanzar hacia
  la izquierda y reaparecer a la derecha cuando sale de la pantalla
  (reciclado, en vez de crear objetos nuevos todo el tiempo).

## Lógica del juego (`Main`)

1. **Inicializa** la ventana, el jugador y la lista de obstáculos.
2. **Game loop** (60 FPS):
   - Si el jugador sigue vivo: actualiza obstáculos y jugador, suma puntos y
     revisa colisiones (`jugador.CollisionWith(o)`). Si choca, pasa a estado
     "muerto".
   - Si está "muerto": espera a que se presione `ESPACIO` para reiniciar
     posición, velocidad, obstáculos y puntaje.
   - Dibuja el fondo, el suelo, el puntaje, el jugador, los obstáculos y, si
     corresponde, el cartel de "¡PERDISTE!" con el puntaje final.
3. **Cierra** la ventana al terminar.

## Estructura del repositorio

```
├── Program.cs     # Código del juego (Orientado a Objetos)
└── README.md
```

## Cómo ejecutarlo

Requiere el SDK de .NET y el paquete `Raylib-cs`.

```bash
dotnet new console -n GeometryDashOO
cd GeometryDashOO
dotnet add package Raylib-cs
# reemplazar el Program.cs generado por el de este repositorio
dotnet run
```

## Paradigma utilizado

Orientado a Objetos: se usa una clase abstracta (`Objeto`), herencia
(`Caja` → `Jugador`, `Caja` → `Obstaculo`), polimorfismo (`Update`, `Draw` y
`CollisionWith` se comportan distinto según la clase real de cada objeto) y
encapsulamiento (cada clase gestiona su propio estado y comportamiento).
