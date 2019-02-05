<template>
    <div class="calculadora">
        <Display :value="valorDisplay"/>

        <Botao label="AC" triple @oncalc="limparMemoria"/>
        <Botao label="/" operacao @oncalc="setOperacao"/>
        <Botao label="7" @oncalc="addDigito"/>
        <Botao label="8" @oncalc="addDigito"/>
        <Botao label="9" @oncalc="addDigito"/>
        <Botao label="*" operacao @oncalc="setOperacao"/>
        <Botao label="4" @oncalc="addDigito"/>
        <Botao label="5" @oncalc="addDigito"/>
        <Botao label="6" @oncalc="addDigito"/>
        <Botao label="-" operacao @oncalc="setOperacao"/>
        <Botao label="1" @oncalc="addDigito"/>
        <Botao label="2" @oncalc="addDigito"/>
        <Botao label="3" @oncalc="addDigito"/>
        <Botao label="+" operacao @oncalc="setOperacao"/>
        <Botao label="0" double @oncalc="addDigito"/>
        <Botao label="." @oncalc="addDigito"/>
        <Botao label="=" operacao @oncalc="setOperacao"/>
        

         

    </div>
</template>

<script>

import Display from '../componentes/display';
import Botao from '../componentes/botao';

export default {

    data: function () {
        return {
            valorDisplay: "0",
            limparDisplay: false,
            operacao: null,
            valores: [0, 0],
            estado: 0
        }
    },
    components: {Botao, Display},
    methods: {
        limparMemoria(){
            Object.assign(this.$data, this.$options.data())
        },
        setOperacao(operacao){
            if (this.estado == 0){
                this.operacao = operacao
                this.estado = 1
                this.limparDisplay = true
            } else {
                let igual = operacao == '='
                let estadoOperacao = this.operacao;

                 try {
                this.valores[0] = eval (
                    `${this.valores[0]}  ${estadoOperacao} ${this.valores[1]}`
                )
            }catch(e){
                    this.$emit('onError', e)
            }

            this.valores[1] = 0
            this.valorDisplay = this.valores[0]
            this.operacao = igual ? null : operacao
            this.estado = igual ? 0 : 1
            this.limparDisplay = !igual
            
            }

           

            
        },

        addDigito(n){
            if (n == '.' && this.valorDisplay.includes('.')){
                return
            }

            let limparDisplay = this.valorDisplay == '0'
                || this.limparDisplay
            
            let estadoValor = limparDisplay ? "" : this.valorDisplay
            let valorDisplay = estadoValor + n
            this.valorDisplay = valorDisplay;
            this.limparDisplay = false

            if (n!== '.'){
                let i = this.estado
                let novoValor = parseFloat(valorDisplay)
                this.valores[i] = novoValor;
            }
        }
    }
}
</script>

<style>

.calculadora {
    height: 320px;
    width: 235px;
    border-radius: 5px;
    overflow: hidden;

    display: grid;
    grid-template-columns: repeat(4, 25%);
    grid-template-rows: 1fr 48px 48px 48px 48px 48px;

}

</style>
