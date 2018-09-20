<template>
    <div id="camera">
        <h1>Vue PWA Camera</h1>
        <p>
            Based on <a href="//webrtc.github.io/samples/" title="WebRTC samples homepage">WebRTC samples</a>
        </p>

        <div class="select">
            <label for="videoSource">Video source: </label><select ref="videoSource" id="videoSource"></select>
        </div>

        <video ref="video" id="video" playsinline autoplay></video>

        <ul>
            <li>
                <a href="https://github.com/danieltorscho/vue-pwa-camera" title="View source for this page on GitHub">
                    View source on GitHub
                </a>
            </li>
            <li>
                <a href="https://github.com/webrtc/samples/tree/gh-pages/src/content/devices/input-output"
                   title="View source for this page on GitHub">
                    View source of WebRTC on GitHub
                </a>
            </li>
        </ul>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                videoElement: '',
                videoSelect: '',
                selectors: ''
            }
        },
        methods: {

            errorHandler(error) {
                console.log('navigator.getUserMedia error: ', error);
            },

            gotDevices(deviceInfos) {
                // Handles being called several times to update labels. Preserve values.
                let values = this.selectors.map(function (select) {
                    return select.value;
                });
                this.selectors.forEach(function (select) {
                    while (select.firstChild) {
                        select.removeChild(select.firstChild);
                    }
                });
                for (let i = 0; i !== deviceInfos.length; ++i) {
                    let deviceInfo = deviceInfos[i];
                    let option = document.createElement('option');
                    option.value = deviceInfo.deviceId;
                    if (deviceInfo.kind === 'videoinput') {
                        option.text = deviceInfo.label || 'camera ' + (this.videoSelect.length + 1);
                        this.videoSelect.appendChild(option);
                    }
                }
                this.selectors.forEach(function (select, selectorIndex) {
                    if (Array.prototype.slice.call(select.childNodes).some(function (n) {
                        return n.value === values[selectorIndex];
                    })) {
                        select.value = values[selectorIndex];
                    }
                });
            },

            gotStream(stream) {
                window.stream = stream; // make stream available to console
                this.videoElement.srcObject = stream;
                // Refresh button list in case labels have become available
                return navigator.mediaDevices.enumerateDevices();
            },

            start() {
                if (window.stream) {
                    window.stream.getTracks().forEach(function (track) {
                        track.stop();
                    });
                }
                let videoSource = this.videoSelect.value;
                let constraints = {
                    video: {deviceId: videoSource ? {exact: videoSource} : undefined}
                };
                navigator.mediaDevices.getUserMedia(constraints).then(this.gotStream).then(this.gotDevices).catch(this.errorHandler);
            }
        },

        mounted() {
            this.videoElement = this.$refs.video;
            this.videoSelect = this.$refs.videoSource;
            this.selectors = [this.$refs.videoSource];

            navigator.mediaDevices.enumerateDevices().then(this.gotDevices).catch(this.errorHandler);

            this.start();
        }
    }
</script>

<style scoped>
    h3 {
        margin: 40px 0 0;
    }

    ul {
        list-style-type: none;
        padding: 0;
    }

    li {
        display: inline-block;
        margin: 0 10px;
    }

    a {
        color: #42b983;
    }
</style>
