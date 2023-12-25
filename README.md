This Vue.js project implements a simple shape dropper game where users can drag and drop various shapes into a game room. The game includes features such as gravity, shape rotation, and clearing the shapes from the game room.



https://github.com/alecfilios/Vue3-TetraShapes-A-Tetris-like-Game/assets/43823795/097bc3f9-75e8-46ed-b576-432d56deb87b




Prerequisites
Make sure you have Node.js installed on your machine.


## Project setup
Follow these steps to run the Vue.js Shape Dropper Game locally:

```
npm install
```
### Compiles and hot-reloads for development
```
npm run serve
```

## How the Game Works
The Shape Dropper Game allows users to drag and drop three types of shapes - square, rectangle, and triangle - into a game room. The game features include:

- Gravity: Shapes fall under the influence of gravity when dropped into the game room.
- Shape Rotation: Users can rotate shapes using the arrow keys.
- Clear Shapes: The "Clear" button in the shape showcase allows users to remove all shapes from the game room.

## Code Overview
The project consists of the following main components:

- App.vue: The main component that serves as the entry point for the application. It includes the game room, shape showcase, and drag-and-drop functionality.

- GameRoom.vue: This component represents the game room where shapes are dropped. It handles the dropping, falling animation, and clearing of shapes.

- ShapeShowcase.vue: The component showcasing shapes available for dragging. It includes a "Clear" button to remove shapes from the game room.


The styles are organized to provide a visually appealing dark-themed interface.
