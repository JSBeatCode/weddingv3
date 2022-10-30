<template>
    <MapNv :webList="state.originList" />
    <br>
    <SearchBar @funcSubmit="funcSubmit" />
    <!-- <div class="row row-cols-1 row-cols-sm-2 row-cols-md-3 g-3"> -->
        <div class="row row-cols-1 row-cols-sm-2 row-cols-md-3 g-2 g-lg-3">
        <div class="col" v-for="one in state.list" :key="state.list.id">
            <div class="card shadow-sm">
                <span data-bs-toggle="modal" data-bs-target="#myModal" @click="changeModalData(one.id, one.imageList, one.name)">
                    <img width="100%" height="225" :src="one.image" :alt="one.imageAlt" class="card-img-top" >
                </span>
                <div class="card-body">
                    <p class="card-text"><span style="font-weight:bold; color:blue; cursor:pointer;" @click="goToSite(one.picSrc)">{{one.name}}</span> | {{one.hall}}</p>
                    <p class="card-text">{{one.address}}</p>
                    <p class="card-text text-muted">￦{{(one.price).toString().replace(/\B(?<!\.\d*)(?=(\d{3})+(?!\d))/g, ",")}}</p>
                    <!-- <div class="text-center"> -->
                    <div class="d-flex justify-content-between align-items-center">
                        <div class="btn-group">
                            <button type="button" class="btn btn-md btn-outline-secondary" data-bs-toggle="modal" data-bs-target="#myModal" @click="changeModalData(one.id, one.imageList, one.name)">사진</button>
                            <button type="button" class="btn btn-md btn-outline-secondary" @click="goToSite(one.naverSrc)">네이버</button>
                            <button type="button" class="btn btn-md btn-outline-secondary" @click="goToSite(one.kakaoSrc)">다음</button>
                        </div>
                        <small class="text-muted">{{((one.naverScore + one.kakaoScore) / 2).toFixed(2)}}</small>
                    </div>
                </div>
            </div>
        </div>
    </div>
    <Modal :mdImageList="state.mdImageList" :mdName="state.mdName" :mdId="state.mdId" @goToSite="goToSite" />
</template>

<script>
import { ref, onMounted, reactive, onUpdated } from 'vue'
import { webList } from '../../data'
import Modal from './Modal.vue'
import SearchBar from './SearchBar.vue'
import MapNv from './MapNv.vue'

export default {
    components: {
        Modal,
        SearchBar,
        MapNv
    },
    setup() {
        const state = reactive({
            mdId: '',
            mdImageList: [],
            mdName: 'Test',
            list: webList,
            originList: webList,
        })


        const goToSite = (address) => {
            window.open(address)
        }

        const changeModalData = (clickId, clickImageList, clickName) => {
            state.mdId = clickId;
            state.mdImageList = clickImageList;
            state.mdName = clickName;
        }

        const funcSubmit = (val) => {
            val = val.toUpperCase();
            if (val === null || val === undefined || val === '') {
                state.list = state.originList;
            } else {
                const result = state.originList.filter((a,i)=>{
                    if (a.name.includes(val) === true 
                    || a.address.includes(val) === true
                    || a.hall.includes(val) === true
                    ){
                        return a;
                    }
                });
                state.list = result;
            }
            
        }

        onMounted(() => {
            // console.log('d1', webList)
        })

        return {
            state,
            goToSite,
            changeModalData,
            funcSubmit,
            // modeValidation
        }
    }
}
</script>

<style lang="scss" scoped>

</style>