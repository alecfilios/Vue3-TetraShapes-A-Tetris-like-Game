<template>
    <!-- Component template for the shape showcase -->
    <div class="shape-showcase">
        <!-- Button to clear shapes -->
        <button @click="clearShapes">Clear</button>

        <!-- Triangle shape for dragging -->
        <div class="shape triangle" @dragstart="startDrag('triangle')" draggable="true"></div>

        <!-- Rectangle shape for dragging -->
        <div class="shape rectangle" @dragstart="startDrag('rectangle')" draggable="true"></div>

        <!-- Square shape for dragging -->
        <div class="shape square" @dragstart="startDrag('square')" draggable="true"></div>
    </div>
</template>

<script>
export default {
    props: ['setShape', 'clearShapes'],
    methods: {
        // Method to initiate a drag operation for a specific shape type
        startDrag(shapeType) {
            this.setShape(shapeType);
        },
    },
};
</script>

<style scoped>
/* Styling for the shape showcase container */
.shape-showcase {
    display: flex;
    flex-direction: column;
    align-items: center;
    width: 60px;
    height: 400px;
    border: 2px solid #280659;
    background-color: #341671;
    position: absolute;
    top: 50%;
    left: 25%;
    transform: translate(-50%, -50%);
    justify-content: center;
}

/* Styling for individual shapes */
.shape {
    margin-bottom: 30px;
    cursor: grab;
}

/* Styling for the square shape */
.square {
    background-color: #660F56;
    width: 30px;
    height: 30px;
}

/* Styling for the rectangle shape */
.rectangle {
    width: 30px;
    height: 60px;
    background-color: #AE2D68;
}

/* Styling for the triangle shape */
.triangle {
    width: 0;
    height: 0;
    border-right: 30px solid transparent;
    border-bottom: 30px solid #F54952;
}

/* Styling for the clear button */
button {
    margin-bottom: 50px;
}
</style>
