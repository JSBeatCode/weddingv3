<template>
    <div class="col">
        <div id="map" style="width:100%;height:20rem;"></div>
    </div>
</template>

<script>
import { onMounted } from 'vue';
import { markerImage } from '../../data'
import { reactive } from 'vue';

export default {
    props: {
        webList: Array
    },
    setup(props, {emit}) {
        
        const state = reactive({
            webList: props.webList
        })
        const mapInit = () => {
            const position = new naver.maps.LatLng(37.557523, 126.8018856);

            const mapOptions = {
                center: position,
                zoom: 10
            };

            const map = new naver.maps.Map('map', mapOptions);

            let markerList = new Array();
            let infoList = new Array();

            for (let i = 0; i < state.webList.length; i++) {
                const a = state.webList[i]
                const lat = a.lat;
                const lang = a.lang;
                // let count = 0;
                // console.log('debug1', (lat !== null && lat !== undefined) , (lang === null && lang === undefined))
                if((lat !== null && lat !== undefined) && (lang !== null && lang !== undefined)) {
                    const marker = new naver.maps.Marker({
                        position: new naver.maps.LatLng(lat, lang),
                        title: a.name,
                        map: map,
                        icon: {
                            url: markerImage,
                            size: new naver.maps.Size(40, 40),
                            scaledSize: new naver.maps.Size(40, 40),
                            origin: new naver.maps.Point(0, 0),
                        }
                    });

                    const infoWindow = new naver.maps.InfoWindow({
                        // content: '<div style="width:150px;text-align:center;padding:10px;">The Letter is <b>"'+ 'test' +'"</b>.</div>'
                        content: '<div>'+a.name+'</div>',
                        maxWidth: 200,
                        backgroundColor: "#eee",
                        //borderColor: "#2db400",
                        //borderWidth: 5,
                        anchorSize: new naver.maps.Size(0, 0),
                        anchorSkew: true,
                        anchorColor: "#eee",
                    });

                    // infoWindow.open(map, marker);

                    infoList.push(infoWindow)
                    markerList.push(marker)
                }
                
            }

            for (var y=0; y<markerList.length; y++) {
                naver.maps.Event.addListener(markerList[y], 'click', getClickHandler(y))
            }

            function getClickHandler(seq) {
                return function(e){
                    var oneMarker = markerList[seq];
                    var oneInfo = infoList[seq];
                    if(oneInfo.getMap()) {
                        oneInfo.close()
                    } else {
                        oneInfo.open(map, oneMarker);
                    }
                }
            }

            for(let c = 0; c < markerList.length; c++){
                const marker = markerList[c];
                getOverlay(map, marker.position, marker.title)
            }
            
            
        }

        // ?????????????????? ???????????? ??????????????? ?????? ????????? ?????????.
        // function getLatLang() {
        //     naver.maps.Event.addListener(map, 'click', function (e) {
        //         // ????????? ???????????? ?????? ????????? ???????????????.
        //         alert(e.coord.lat() + ', ' + e.coord.lng());
        //         // e ??? ????????? ???????????? ????????? (???????????? ????????? ?????? ????????? ?????????)
        //         // e ?????? ????????? ?????? ????????? ?????? ?????????.
        //         // e.coord.lat() ??? ?????? (Latitude)  ?????? ????????? lat
        //         // e.coord.lng() ??? ?????? (Longitude) ?????? ????????? lng
        //     });
        // }

        function getOverlay(map, mapPosition, title) {
                /**
                 * ????????? ?????? ???????????? ????????????
                 */
                var CustomOverlay = function (options) {
                    
                    // this._element = $('<div style="position:absolute;left:0;top:0;width:124px;background-color:#F2F0EA;text-align:center;border:2px solid #6C483B;">?????????????????????</div>')
                    this._element = document.createElement('div')
                    this._element.innerHTML = title;
                    this._element.style.cssText = ("" + (
                        "position:absolute;"
                        +"left:0;"
                        +"top:0;"
                        +"width:124px;"
                        +"background-color:#F2F0EA;"
                        +"text-align:center;"
                        +"border:2px solid #6C483B;"
                        +"font-size:12px"
                    ));
                    this.setPosition(options.position);
                    this.setMap(options.map || null);
                };

                // CustomOverlay??? OverlayView??? ??????????????????.
                CustomOverlay.prototype = new naver.maps.OverlayView();

                CustomOverlay.prototype.constructor = CustomOverlay;

                CustomOverlay.prototype.setPosition = function (position) {
                    this._position = position;
                    this.draw();
                };

                CustomOverlay.prototype.getPosition = function () {
                    return this._position;
                };

                CustomOverlay.prototype.onAdd = function () {
                    var overlayLayer = this.getPanes().overlayLayer;
                    // console.log('overlayLayer ', overlayLayer)
                    // this._element.appendTo(overlayLayer);
                    overlayLayer.append(this._element);
                    // console.log('overlayLayer ', overlayLayer)
                };

                CustomOverlay.prototype.draw = function () {
                    // ?????? ????????? ???????????? ???????????? draw ????????? ?????? ????????????.
                    if (!this.getMap()) {
                        return;
                    }


                    // projection ????????? ?????? LatLng ????????? ?????? ????????? ???????????????.
                    var projection = this.getProjection(),
                        position = this.getPosition();

                    var pixelPosition = projection.fromCoordToOffset(position);

                    this._element.style.left = (pixelPosition.x - 60) + 'px';
                    this._element.style.top = (pixelPosition.y - 65) + 'px';
                };

                CustomOverlay.prototype.onRemove = function () {
                    var overlayLayer = this.getPanes().overlayLayer;

                    this._element.remove();
                    // // ????????? ???????????? ??????????????? ???????????????.
                    // this._element.off();
                    this._element.removeEventListener('click', this.onClick);
                };

                // ???????????? ??????
                var overlay = new CustomOverlay({
                    position: mapPosition,
                    map: map
                });
                // overlay.onAdd();
                
            }

        onMounted(() => {
            mapInit();
        })

        return {
            state,
            mapInit,
        }
    }
}
</script>

<style lang="scss" scoped>

</style>