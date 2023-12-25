<template>
  <div id="app" width="100%" height="100vh" @dragstart="onDragStart" @dragend="onDragEnd">
    <GameRoom ref="gameRoom" :set-shape="setShape" />
    <ShapeShowcase :set-shape="setShape" :clear-shapes="clearShapes" />
    <div v-if="drag" :style="dragStyle"></div>
  </div>
</template>

<script>
import GameRoom from './components/GameRoom.vue';
import ShapeShowcase from './components/ShapeShowcase.vue';

export default {
  components: {
    GameRoom,
    ShapeShowcase,
  },
  data() {
    return {
      drag: false,
      dragStyle: {
        position: 'absolute',
        zIndex: 1000,
        top: 0,
        left: 0,
        transform: 'translate(-50%, -50%)',
      },
    };
  },
  methods: {
    setShape(shape) {
      if (!this.drag) {
        this.draggedShape = shape;
      }
    },
    onDragStart() {
      // Check if the animation is still running in the game room
      if (!this.drag) {
        this.drag = true;
      }
    },
    onDragEnd() {
      if (this.drag) {
        this.drag = false;
        this.draggedShape = '';
      }
    },
    clearShapes() {
      this.$refs.gameRoom.clearShapes();
    },
  },
};
</script>

<style>
body {
  margin: 0;
  overflow: hidden;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  color: #ecf0f1;
  background-color: #422680;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}

.shape {
  margin-bottom: 30px;
  cursor: grab;
}

.square {
  background-color: #660F56;
  width: 30px;
  height: 30px;
}

.rectangle {
  width: 30px;
  height: 60px;
  background-color: #AE2D68;
}

.triangle {
  width: 0;
  height: 0;
  border-right: 30px solid transparent;
  border-bottom: 30px solid #F54952;
}
</style>
