<script lang="ts" setup>
import { ref } from 'vue';
import type { Ref } from 'vue';
function generateRandomLightColor() {
    const hue = Math.floor(Math.random() * 360); // Hue is a degree on the color wheel (from 0 to 360)
    const saturation = Math.floor(Math.random() * 50) + 50; // Saturation is a percentage (50% - 100% for lighter colors)
    const lightness = 70; // Lightness can be set to 70% for light colors

    return `hsl(${hue}, ${saturation}%, ${lightness}%)`;
}

const pop: Ref<boolean> = ref(false);
const newNote: Ref<string> = ref('');
interface Note {
    id: number;
    text: string;
    date: Date;
    backgroundColor: string;
}
const notes: Ref<Note[]> = ref([]); // Assuming list should hold strings

const addNote = (): void => {
    if (!newNote.value.length) return;
    !newNote.value.length;
    notes.value.push({
        id: Math.floor(Math.random() * 100),
        text: newNote.value,
        date: new Date(),
        backgroundColor: generateRandomLightColor(),
    });
    newNote.value = '';
    pop.value = !pop.value;
};
</script>

<style lang="scss" scoped></style>
<template>
    <main>
        <div v-if="pop" class="overlay">
            <div class="modal">
                <p @click.prevent="pop = !pop">x</p>
                <textarea v-model="newNote" />
                <button @click.prevent="addNote()">Add Note</button>
            </div>
        </div>
        <div class="container">
            <header>
                <h1>Notes</h1>
                <button @click.prevent="pop = !pop">+</button>
            </header>
            <div class="cards-container">
                <div
                    v-for="note in notes"
                    class="card"
                    :style="{ backgroundColor: note.backgroundColor }"
                    :key="note.id"
                >
                    <p class="main-text">{{ note.text }}</p>
                    <p class="date">{{ note.date.toLocaleDateString('en-US') }}</p>
                </div>
            </div>
        </div>
    </main>
</template>

<style scoped>
.container {
    max-width: 1000px;
    padding: 10px;
    margin: 0 auto;
}

h1 {
    font-weight: bold;
    margin-bottom: 25px;
    font-size: 75px;
}

.card {
    width: 225px;
    height: 225px;
    background-color: rgb(237, 182, 44);
    padding: 10px;
    border-radius: 15px;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    margin-right: 20px;
    margin-bottom: 20px;
}

.main-text {
    line-height: 1.25;
    font-size: 17.5px;
    font-weight: bold;
}

.date {
    font-size: 12.5px;
    margin-top: auto;
}

header {
    display: flex;
    justify-content: space-between;
    align-items: center;
}

header button {
    border: none;
    padding: 10px;
    width: 50px;
    height: 50px;
    cursor: pointer;
    background-color: rgb(21, 20, 20);
    border-radius: 1000px;
    color: white;
    font-size: 20px;
}
.cards-container {
    display: flex;
    flex-wrap: wrap;
}

.overlay {
    position: absolute;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.77);
    transform: translate(-50%, -50%);
    top: 50%;
    left: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    z-index: 10;
}

main {
    height: 100vh;
    width: 100vw;
}

.modal {
    width: 750px;
    background-color: white;
    border-radius: 10px;
    padding: 30px;
    position: relative;
    display: flex;
    flex-direction: column;
}

.modal button {
    padding: 10px 20px;
    font-size: 20px;
    width: 100%;
    background-color: blueviolet;
    border: none;
    color: white;
    cursor: pointer;
    margin-top: 15px;
}

.modal p {
    margin-left: auto;
    font-size: 20px;
    z-index: 100000;
    cursor: pointer;
}

textarea {
    width: 100%;
    height: 200px;
    padding: 5px;
    font-size: 20px;
    border: 1px solid;
    border-radius: 4px;
}
</style>
