<template>
        <header>
            <div class="bg-dark collapse" id="navbarHeader" >
                <div class="container">
                  <div class="row">
                    <div class="col-sm-8 col-md-7 py-4">
                      <h4 class="text-white">웨딩홀 목록</h4>
                      <p class="text-muted">사랑하는 저희 {{state.bride}} ♡ {{state.groom}} 커플을 축하해 주셔서 감사합니다.</p>
                    </div>
                    <div class="col-sm-4 offset-md-1 py-4">
                      <h4 class="text-white">연락처</h4>
                      <ul class="list-unstyled">
                        <li><a href="#" class="text-white">개인연락</a></li>
                      </ul>
                    </div>
                  </div>
                </div>
            </div>
            <div class="navbar navbar-dark bg-dark shadow-sm ">
                <div class="container">
                    <a href="#" class="navbar-brand d-flex align-items-center">
                        <strong>{{state.bride}} ♡ {{state.groom}} 웨딩홀</strong>
                    </a>
                    <button class="navbar-toggler collapsed" type="button" data-bs-toggle="collapse" data-bs-target="#navbarHeader" aria-controls="navbarHeader" aria-expanded="false" aria-label="Toggle navigation">
                        <span class="navbar-toggler-icon"></span>
                      </button>
                </div>
            </div>
        </header>
</template>

<script>
import { reactive } from 'vue'
export default {
    props: {
        bride: String,
        groom: String
    },
    setup (props, {emit}) {
        const state = reactive({
            bride: props.bride,
            groom: props.groom,
        })

        return {
            state
        }
    }
}
</script>

<!-- <style lang="scss" scoped></style> -->