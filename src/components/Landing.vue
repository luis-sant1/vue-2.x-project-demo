<!-- Imports -->
<script>
import { VueEditor } from "vue2-editor";
export default {
    name: 'LandingPage',
    components: {
        VueEditor
    },
    data() {
        return {
            showModal : false,
            content: `
            <h1> this header </h1>
            <p>Amet irure excepteur cillum ipsum sit elit dolor et nisi eu consectetur aliqua laborum laboris.</p>
            <h1> This Footer</h1>
            `
        }
    }
}
</script>

<template>
    <main id="container">
        <h1>
            Demo AMII
        </h1>

        <div id="app">
            <VueEditor v-model="content" />
        </div>
        <button id="container-sendBtn" @click="showModal = true">Send</button>

        <div v-if="showModal" id="container-modal" >
            <div id="container-modal-options">
                <span>Cómo quiere proceder?</span>
                <button>Imprimir documento</button>
                <button>Editar documento en formato DOC</button>
                <button @click="showModal = false">Close</button>
            </div>
        </div>
    </main>
</template>



<style scoped>
#container-sendBtn {
    width: 25%;
}

#container {
    position: relative;
    display: flex;
    flex-direction: column;
    justify-content: center;
}

#container-modal {
    /* display: none; */
    height: 100%;
    width: 100%;
    position: absolute;
    background-color: rgba(0, 0, 0, 0.438);
    overflow: hidden;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
}
#container-modal-options{
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    background-color: white;
    width: 50rem;
    height: 18rem;
}
</style>