<template>
  <SvgWrapper 
    class="formula-content"
    overflow="visible" 
    :width="box.w + 32"
    :height="box.h + 32"
    stroke="#000" 
    stroke-width="1" 
    fill="none" 
    stroke-linecap="round"
    stroke-linejoin="round"
  >
    <g 
      :transform="`scale(${scale}, ${scale}) translate(0,0) matrix(1,0,0,1,0,0)`"
      transform-origin="0 50%"
    >
      <path :d="pathd"></path>
    </g>
  </SvgWrapper>
</template>

<script lang="ts">
import { computed, defineComponent, ref, watch } from 'vue'
import { hfmath } from './hfmath'

export default defineComponent({
  name: 'formula-content',
  props: {
    latex: {
      type: String,
      required: true,
    },
    width: {
      type: Number,
      required: true,
    },
    height: {
      type: Number,
      required: true,
    },
  },
  setup(props) {
    const box = ref({ x: 0, y: 0, w: 0, h: 0 })
    const pathd = ref('')

    watch(() => props.latex, () => {
      const eq = new hfmath(props.latex)
      pathd.value = eq.pathd({})
      box.value = eq.box({})
    }, { immediate: true })

    const scale = computed(() => {
      const boxW = box.value.w + 32
      const boxH = box.value.h + 32

      if (boxW > props.width || boxH > props.height) {
        if (boxW / boxH > props.width / props.height) return props.width / boxW
        return props.height / boxH
      }
      return 1
    })

    return {
      box,
      pathd,
      scale,
    }
  },
})
</script>
