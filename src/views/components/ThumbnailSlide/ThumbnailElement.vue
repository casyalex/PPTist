<template>
  <div 
    class="base-element"
    :class="`base-element-${elementInfo.id}`"
    :style="{
      zIndex: elementIndex,
    }"
  >
    <component
      :is="currentElementComponent"
      :elementInfo="elementInfo"
      target="thumbnail"
    ></component>
  </div>
</template>

<script lang="ts">
import { computed, defineComponent, PropType } from 'vue'
import { ElementTypes, PPTElement } from '@/types/slides'

import BaseImageElement from '@/views/components/element/ImageElement/BaseImageElement.vue'
import BaseTextElement from '@/views/components/element/TextElement/BaseTextElement.vue'
import BaseShapeElement from '@/views/components/element/ShapeElement/BaseShapeElement.vue'
import BaseLineElement from '@/views/components/element/LineElement/BaseLineElement.vue'
import BaseChartElement from '@/views/components/element/ChartElement/BaseChartElement.vue'
import BaseTableElement from '@/views/components/element/TableElement/BaseTableElement.vue'
import BaseLatexElement from '@/views/components/element/LatexElement/BaseLatexElement.vue'
import BaseVideoElement from '@/views/components/element/VideoElement/BaseVideoElement.vue'

export default defineComponent({
  name: 'base-element',
  props: {
    elementInfo: {
      type: Object as PropType<PPTElement>,
      required: true,
    },
    elementIndex: {
      type: Number,
      required: true,
    },
  },
  setup(props) {
    const currentElementComponent = computed(() => {
      const elementTypeMap = {
        [ElementTypes.IMAGE]: BaseImageElement,
        [ElementTypes.TEXT]: BaseTextElement,
        [ElementTypes.SHAPE]: BaseShapeElement,
        [ElementTypes.LINE]: BaseLineElement,
        [ElementTypes.CHART]: BaseChartElement,
        [ElementTypes.TABLE]: BaseTableElement,
        [ElementTypes.LATEX]: BaseLatexElement,
        [ElementTypes.VIDEO]: BaseVideoElement,
      }
      return elementTypeMap[props.elementInfo.type] || null
    })

    return {
      currentElementComponent,
    }
  },
})
</script>