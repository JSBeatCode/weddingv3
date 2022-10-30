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
            // console.log('debug1', state.webList);
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

                    infoWindow.open(map, marker);

                    infoList.push(infoWindow)
                    markerList.push(marker);
                    // infoList.push({
                    //     id: i,
                    //     data: infoWindow
                    // })
                    // markerList.push({
                    //     id: i,
                    //     data: marker
                    // });

                    // naver.maps.Event.addListener(marker, 'click', getClickHandler(count))
                    // count++;
                }
                
            }
            // console.log('debug2-1', markerList)
            // console.log('debug2-2', infoList)

            for (var y=0; y<markerList.length; y++) {
            //     console.log('debug2-3', markerList[i]);
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

            // getOverlay(position)
            
            function getOverlay(mapPosition) {
                /**
                 * 사용자 정의 오버레이 구현하기
                 */
                var CustomOverlay = function (options) {
                    this._element = ('<div style="position:absolute;left:0;top:0;width:120px;height:100px;line-height:30px;text-align:center;background-color:#fff;border:2px solid #f00;">커스텀오버레이</div>')

                    this.setPosition(options.position);
                    this.setMap(options.map || null);
                };

                // CustomOverlay는 OverlayView를 상속받습니다.
                CustomOverlay.prototype = new naver.maps.OverlayView();

                CustomOverlay.prototype.constructor = CustomOverlay;

                CustomOverlay.prototype.onAdd = function () {
                    var overlayLayer = this.getPanes().overlayLayer;
                    console.log('overlayLayer ', overlayLayer)
                    overlayLayer = (overlayLayer += this._element)
                    // overlayLayer = (this._element)

                };

                CustomOverlay.prototype.draw = function () {
                    // 지도 객체가 설정되지 않았으면 draw 기능을 하지 않습니다.
                    if (!this.getMap()) {
                        return;
                    }

                    // projection 객체를 통해 LatLng 좌표를 화면 좌표로 변경합니다.
                    var projection = this.getProjection(),
                        position = this.getPosition();

                    var pixelPosition = projection.fromCoordToOffset(position);

                    // this._element.css('left', pixelPosition.x);
                    // this._element.css('top', pixelPosition.y);
                    // this.element.style.left = pixelPosition.x + 'px';
                    // this.element.style.top = pixelPosition.y + 'px';
                    console.log('debug1', this._element);
                };

                CustomOverlay.prototype.onRemove = function () {
                    // this._element.remove();

                    // // 이벤트 핸들러를 설정했다면 정리합니다.
                    // this._element.off();
                };

                CustomOverlay.prototype.setPosition = function (position) {
                    this._position = position;
                    this.draw();
                };

                CustomOverlay.prototype.getPosition = function () {
                    return this._position;
                };

                // 오버레이 생성
                var overlay = new CustomOverlay({
                    position: mapPosition,
                    map: map
                });

                overlay.onAdd();
                
            }

            // 클릭이벤트를 적용하여 경고창으로 위도 경도를 봅니다.
            // function getLatLang() {
            //     naver.maps.Event.addListener(map, 'click', function (e) {
            //         // 지도를 클릭하면 아래 내용이 실행됩니다.
            //         alert(e.coord.lat() + ', ' + e.coord.lng());
            //         // e 는 클릭시 넘어오는 이벤트 (네이밍은 원하는 대로 하셔도 됩니다)
            //         // e 에서 필요한 것을 꺼내서 쓰면 됩니다.
            //         // e.coord.lat() 는 위도 (Latitude)  보통 약어로 lat
            //         // e.coord.lng() 는 경도 (Longitude) 보퉁 약어로 lng
            //     });
            // }
            
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