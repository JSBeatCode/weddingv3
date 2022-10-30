<template>
    <div class="modal fade" id="myModal">
        <div class="modal-dialog modal-xl">
            <!-- The Modal -->
            <div class="modal-content">

                <!-- Modal Header -->
                <div class="modal-header">
                    <h4 class="modal-title">{{state.mdName}}</h4>
                    <button type="button" class="btn-close" data-bs-dismiss="modal"></button>
                </div>

                <!-- Modal body -->
                <div class="modal-body">
                    <!-- Carousel -->
                    <div id="demo" class="carousel slide" data-bs-ride="carousel">

                        <!-- The slideshow/carousel -->
                        <div class="carousel-inner">
                            <div v-for="(one, index) in state.mdImageList" v-bind:key="one+'-'+state.mdName+'-'+state.mdId">
                                <div :class="'carousel-item' + ' ' + (index === 0 ? 'active' : '')">
                                    <span @click="$emit('goToSite', one)">
                                        <img class="d-block w-100" :src="one" :alt="'wedding hall' + (index+1)" style="width:100%">
                                    </span>
                                </div>
                            </div>
                            <!-- <div class="carousel-item active">
                                <img class="d-block w-100" src="src/assets/img/halls/01/01.jpg" alt="" style="width:100%">
                            </div>
                            <div class="carousel-item">
                                <img class="d-block w-100" src="src/assets/img/halls/01/02.jpg" alt="" style="width:100%">
                            </div>
                            <div class="carousel-item">
                                <img class="d-block w-100" src="src/assets/img/halls/01/03.jpg" alt="" style="width:100%">
                            </div>
                            <div class="carousel-item">
                                <img class="d-block w-100" src="src/assets/img/halls/01/04.jpg" alt="" style="width:100%">
                            </div> -->
                        </div>
                        
                        <!-- Left and right controls/icons -->
                        <button id="carousel-control-prev" class="carousel-control-prev" type="button" data-bs-target="#demo" data-bs-slide="prev">
                            <span class="carousel-control-prev-icon"></span>
                        </button>
                        <button id="carousel-control-next" class="carousel-control-next" type="button" data-bs-target="#demo" data-bs-slide="next">
                            <span class="carousel-control-next-icon"></span>
                        </button>
                    </div>
                </div>

                <!-- Modal footer -->
                <div class="modal-footer">
                    <button type="button" class="btn btn-danger" data-bs-dismiss="modal" >Close</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import { ref, onMounted, onUpdated, reactive } from 'vue'
export default {
    props: {
        mdImageList: Array,
        mdName: String,
        mdId: String,
    },
    setup(props, {emit}) {
        const state = reactive({
            mdImageList: props.mdImageList,
            mdName: props.mdName,
            mdId: props.mdId
        });
        

        // onMounted(() => {

        // })

        onUpdated(() => {
            state.mdImageList = props.mdImageList;
            state.mdName = props.mdName;
            state.mdId = props.mdId;
        })

        return {
            state,                    
        }
    },
    emits: [
        'goToSite'
    ]
}
</script>

<style lang="scss" scoped>
#carousel-control-next, #carousel-control-prev {
    width: 30px;
    height: 30px;
    position: absolute;
    top:45%
}
</style>