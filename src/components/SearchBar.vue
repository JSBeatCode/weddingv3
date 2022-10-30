<template>
    <div class="col">
        <div class="input-group mb-3">
            <input type="text" 
                    class="form-control" 
                    placeholder="예식장 검색" 
                    aria-label="예식장 검색" 
                    aria-describedby="basic-addon2"
                    v-model="state.txt"
                    @input="onChange"
                    @keyup.enter="enterChange"
                >
                <div id="input-x" @click="delAllChange">
                    X
                </div>
          </div>
    </div>
</template>

<script>
import { reactive, ref } from 'vue'
export default {
    props:{

    },
    setup (props, {emit}) {
        // const searchTxt = ref('')
        const state = reactive({
            txt: ''
        })

        const onChange = (e) => {
            state.txt = e.target.value;
            emit('funcSubmit', state.txt);
        }

        const delAllChange = () => {
            state.txt = '';
            emit('funcSubmit', state.txt);
        }
        // const enterChange = (e) => {
        //     let val = state.txt;
        //     emit('funcSubmit', val);
        //     if(e.keyCode === 13) {
        //         state.txt = '';
        //     }
        // }

        return {
            state,
            onChange,
            delAllChange,
            // enterChange
        }
    },
    emits: [
        'funcSubmit'
    ]
}
</script>

<style lang="scss" scoped>
#input-x {
    cursor: pointer;
    position:absolute;
    right:2%;
    top:15%;
    width:20px;
    height:20px;
    border-radius:50%;
    text-align: center;
    z-index: 100;
}
#input-x:hover {
    font-family: bold;
}
</style>