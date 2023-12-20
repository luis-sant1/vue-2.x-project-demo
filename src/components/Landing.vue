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
    <main>
        <h1>
            Demo AMII
        </h1>

        <div id="app">
            <VueEditor v.model = "content"/>
        </div>
    </main>
</template>



<style scoped></style>