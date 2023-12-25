<template>
    <!-- Game room component template -->
    <div class="game-room" @drop="dropShape" @dragover.prevent>
        <!-- Loop through shapes and render them -->
        <div v-for="(shape, index) in shapes" :key="index" :class="`shape ${shape.type}`" :style="shape.style"
            @dragstart="startDrag(shape.type)" @dragend="endDrag(index)" draggable="true"></div>
    </div>
</template>

<script>
export default {
    props: ['setShape'],
    data() {
        return {
            // Array to store shapes in the game room
            shapes: [],
            // Gravity affecting the falling shapes
            gravity: 0.1,
            // Flag to track if the animation is running
            animationRunning: false,
            // Index of the currently falling shape
            fallingIndex: -1,
            // Rotation angle of the falling shape
            rotationAngle: 0,
            // Size of the grid
            gridSize: 30,
            // Array to store the capacities of each column in the grid
            columnCapacities: Array(20).fill(0),
        };
    },
    methods: {
        // Handle dropping a shape into the game room
        dropShape(event) {
            if (this.$parent.drag) {
                // Calculate grid position for the dropped shape
                const gridX = Math.floor((event.clientX - window.innerWidth / 2 + 600 / 2) / this.gridSize) * this.gridSize;
                const gridY = Math.floor((event.clientY - window.innerHeight / 2 + 400 / 2) / this.gridSize) * this.gridSize;

                // Create a new shape object
                const newShape = {
                    type: this.$parent.draggedShape,
                    style: {
                        position: 'absolute',
                        top: `${gridY}px`,
                        left: `${gridX}px`,
                    },
                    velocity: { x: 0, y: 0 },
                    falling: true,
                    rotationAngle: 0,
                    column: gridX / 30,
                    bottomLimit: this.columnCapacities[gridX / 30],
                    rotated: false,
                };

                // Update column capacities
                this.columnCapacities[gridX / 30] += 1;
                if (newShape.type === 'rectangle') {
                    this.columnCapacities[gridX / 30] += 1;
                }

                // Add the new shape to the shapes array
                this.shapes.push(newShape);
                this.fallingIndex = this.shapes.length - 1;
                this.applyGravity();
            }
        },

        // Get the height of a shape based on its type and rotation
        getShapeHeight(type, index) {
            switch (type) {
                case 'triangle':
                    return 30; // Adjust this value based on the actual height of your triangle shape
                case 'rectangle':
                    if (Math.abs(this.shapes[index].rotationAngle % 180) === 90) {
                        return 45; // Height for 90 or 270 degrees rotation
                    }
                    return 60;
                case 'square':
                    return 30;
                default:
                    return 0;
            }
        },

        // Get the width of a shape based on its type
        getShapeWidth(type) {
            switch (type) {
                case 'triangle':
                    return 30; // Adjust this value based on the actual width of your triangle shape
                case 'rectangle':
                    return 30; // Adjust this value based on the actual width of your rectangle shape
                case 'square':
                    return 30;
                default:
                    return 0;
            }
        },

        // Handle starting a drag operation for a shape
        startDrag(shapeType) {
            this.setShape(shapeType);
        },

        // Handle ending a drag operation for a shape
        endDrag(index) {
            // Update column capacities
            this.columnCapacities[this.shapes[index].column] -= 1;

            // Remove the shape from the array
            this.shapes.splice(index, 1);

            this.fallingIndex--

        },

        // Apply gravity to falling shapes
        applyGravity() {
            // Check if the animation loop is already running
            if (!this.animationRunning) {
                this.animationRunning = true; // Set flag to indicate the animation is running

                // Animation loop
                const animate = () => {
                    this.shapes.forEach((shape, index) => {
                        const shapeheight = this.getShapeHeight(shape.type, index);

                        // Check if the shape has reached the bottom of the game room
                        const bottomLimit = 400 - shapeheight - shape.bottomLimit * 30;
                        if (parseFloat(shape.style.top) >= bottomLimit) {
                            shape.style.top = `${bottomLimit}px`;
                            shape.velocity.y = 0; // Stop falling when it reaches the bottom
                            shape.falling = false;
                        } else {
                            // Update the y-position based on velocity
                            shape.style.top = `${parseFloat(shape.style.top) + shape.velocity.y}px`;
                            // Update velocity for the next frame by adding gravity
                            shape.velocity.y = this.gravity;
                            shape.falling = true;
                        }

                        // Rotate the falling shape when arrow keys are pressed
                        if (this.animationRunning && index === this.fallingIndex) {
                            shape.style.transform = `rotate(${shape.rotationAngle}deg)`;
                        }
                    });

                    // Check if there are still shapes in the array
                    if (this.checkForFallingShape()) {
                        // Continue the animation loop
                        window.requestAnimationFrame(animate);
                    } else {
                        // If no shapes are left, stop the animation
                        this.animationRunning = false;
                    }
                };

                // Start the animation loop
                window.requestAnimationFrame(animate);
            }
        },

        // Handle keydown event for arrow keys
        onKeyDown(e) {
            // Rotate the shape on arrow key presses
            switch (e.key) {
                case 'ArrowLeft':
                    this.shapes[this.fallingIndex].rotationAngle -= 90;
                    break;
                case 'ArrowRight':
                    this.shapes[this.fallingIndex].rotationAngle += 90;
                    break;
            }
            // Handle rotation for rectangles
            if (this.shapes[this.fallingIndex].type === 'rectangle') {
                this.shapes[this.fallingIndex].rotated = !this.shapes[this.fallingIndex].rotated;
                if (this.shapes[this.fallingIndex].rotated) {
                    this.columnCapacities[this.shapes[this.fallingIndex].column] -= 1;
                } else {
                    this.columnCapacities[this.shapes[this.fallingIndex].column] += 1;
                }
            }
        },

        // Check if there are still falling shapes in the array
        checkForFallingShape() {
            if (this.shapes.length <= 0) return false;
            return this.shapes.some((shape) => shape.falling);
        },

        // Method to clear shapes
        clearShapes() {
            this.shapes = [];
            this.columnCapacities = Array(20).fill(0);
            this.fallingIndex = -1;
            this.animationRunning = false;
        },
    },

    mounted() {
        // Add event listener for keydown when the component is mounted
        window.addEventListener('keydown', this.onKeyDown);
    },

    beforeUnmount() {
        // Remove event listener when the component is destroyed
        window.removeEventListener('keydown', this.onKeyDown);
    },
};
</script>

<style scoped>
/* Styling for the game room container */
.game-room {
    width: 600px;
    height: 400px;
    border: 2px solid #280659;
    background-color: #341671;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    overflow: hidden;
    /* Add overflow to hide shapes that go beyond the game room boundaries */
}
</style>
