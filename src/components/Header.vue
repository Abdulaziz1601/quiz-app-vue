<template>
    <div>
        <b-nav tabs>
            <b-nav-item disabled><b>Fancy Quiz App</b></b-nav-item>
            <b-nav-item disabled><b>Counter: {{numCorrect}}/{{numTotal}}</b></b-nav-item>
        </b-nav>
        <div class="text-center hint">
            Choose the answer and submit:
        </div> 
    </div>
</template>

<script>
export default {
    props: [
        'numCorrect',   
        'numTotal'
    ]
}
</script>

<style scoped>
    .hint {
        font-size: 25px;
        color: red;
    }
</style>