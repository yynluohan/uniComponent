<template>
    <view 
        class="ply_video_container"
        :style="{ width: width, height: height }"
    >
        <video
            class="ply_video"
            id="myVideo"
            :controls="true"
            :style="{ width: width, height: height }"
            :src="url"
            :poster="coverImage"
            :initial-time="initialTime"
            :direction="direction"
            @play="handlePlay"
            @fullscreenchange="handleFullscreenchange"
            @timeupdate="handleTimeupdate"
            @loadedmetadata="handleLoadedmetadata"
        />
    </view>
</template>

<script>
    export default {
        props: {
          url: {
              type: String,
              default: ''
          },
          coverImage: {
              type: String,
              default: ''
          },
          isStopPlay: {
              type: Boolean,
              default: false
          },
          width: {
              type: String,
              default: '100%'
          },
          height: {
            type: String,
            default: '450rpx'
          },
          idIndex: [String, Number],
          initialTime: {
              type: Number,
              default: 0
          },
          direction: {
              type: Number,
              default: -1
          }
        },
        data () {
            return {
                videoContext: null
            }
        },
        watch: {
            isStopPlay: {
                handler (next, prev) {
                    if (next) {
                        try {
                            this.videoContext.pause()
                        } catch {}
                    }
                }
            }
        },
        beforeDestroy() {
           this.videoContext.pause()
        },
        mounted() {
            this.videoContext = uni.createVideoContext('myVideo', this)
        },
        methods: {
            handlePlay () {
                this.$emit('play', this.idIndex)
            },
            // 切换全屏
            handleFullscreenchange (e) {
                this.$emit('fullscreenchange', e.detail || {})
            },
            // 播放进度变化时
            handleTimeupdate (e) {
                this.$emit('timeupdate', e.detail || {})
                this.$emit('loadedmetadata', {
                    duration: e.detail.duration
                })
            },
            //视频元数据加载完成时触发
            handleLoadedmetadata (e) {
                this.$emit('loadedmetadata', e.detail || {})
            }
        }
    }
</script>

<style lang="scss">
    .ply_video_container {
        overflow: hidden;
        position: relative;
        display: flex;
        justify-content: center;
        align-items: center;
        .ply_video {
            flex: 1;
        }
    }
</style>