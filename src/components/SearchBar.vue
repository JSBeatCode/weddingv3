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
            <!-- <div class="input-group-append">
              <button class="btn btn-info" type="button">검색</button>
            </div> -->
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
            // enterChange
        }
    },
    emits: [
        'funcSubmit'
    ]
}
</script>

<style lang="scss" scoped>

</style>