<template>
  <div class="video-style-panel">
    <div class="title">视频预览封面</div>
    <div class="background-image-wrapper">
      <FileInput @change="files => setVideoPoster(files)">
        <div class="background-image">
          <div class="content" :style="{ backgroundImage: `url(${handleElement.poster})` }">
            <IconPlus />
          </div>
        </div>
      </FileInput>
    </div>
    <div class="row"><Button style="flex: 1;" @click="updateVideo({ poster: '' })">重置封面</Button></div>
  </div>
</template>

<script lang="ts">
import { computed, defineComponent } from 'vue'
import { MutationTypes, useStore } from '@/store'
import { PPTVideoElement } from '@/types/slides'
import { getImageDataURL } from '@/utils/image'
import useHistorySnapshot from '@/hooks/useHistorySnapshot'

export default defineComponent({
  name: 'video-style-panel',
  setup() {
    const store = useStore()
    const handleElement = computed<PPTVideoElement>(() => store.getters.handleElement)

    const { addHistorySnapshot } = useHistorySnapshot()

    const updateVideo = (props: Partial<PPTVideoElement>) => {
      store.commit(MutationTypes.UPDATE_ELEMENT, { id: handleElement.value.id, props })
      addHistorySnapshot()
    }

    // 设置视频预览封面
    const setVideoPoster = (files: File[]) => {
      const imageFile = files[0]
      if (!imageFile) return
      getImageDataURL(imageFile).then(dataURL => updateVideo({ poster: dataURL }))
    }

    return {
      handleElement,
      updateVideo,
      setVideoPoster,
    }
  }
})
</script>

<style lang="scss" scoped>
.row {
  width: 100%;
  display: flex;
  align-items: center;
  margin-bottom: 10px;
}
.title {
  margin-bottom: 10px;
}
.background-image-wrapper {
  margin-bottom: 10px;
}
.background-image {
  height: 0;
  padding-bottom: 56.25%;
  border: 1px dashed $borderColor;
  border-radius: $borderRadius;
  position: relative;
  transition: all $transitionDelay;

  &:hover {
    border-color: $themeColor;
    color: $themeColor;
  }

  .content {
    @include absolute-0();

    display: flex;
    justify-content: center;
    align-items: center;
    background-position: center;
    background-size: contain;
    background-repeat: no-repeat;
    cursor: pointer;
  }
}
</style>