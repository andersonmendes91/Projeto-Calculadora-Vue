<template>
    <button class="botao" 
    @click="$emit('oncalc', label)"
    :class="{double, triple, operacao}">
        {{label}}
    </button>
</template>

<script>
export default {
    props: {
        label: {},
        operacao: {type: Boolean},
        double: {type: Boolean},
        triple: {type: Boolean}
    }

}
</script>

<style>

    :root {
        --bg-botao: #f0f0f0;
        --border-botao: solid 1px #888;
    }

    .botao {
        font-size: 1.4em;
        background-color: var(--bg-botao);
        border:none;
        border-right: var(--border-botao);
        border-bottom: var(--border-botao);
        outline: none;
    }

    .botao:active {
        background-color: #ccc;
    }

    .botao.double {
        grid-column: span 2;
    }

    .botao.triple {
        grid-column: span 3;
    }

    .botao.operacao {
        background-color:  #5882FA ;
        color: #fff;
        }

     .botao.operacao:active {
         background-color: #5882FAcc;
     }


</style>
