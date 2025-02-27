<template>
  <div class="line-pool">
    <div class="category" v-for="item in lineList" :key="item.type">
      <div class="category-name">{{item.type}}</div>
      <div class="line-list">
        <div class="line-item" v-for="(line, index) in item.children" :key="index">
          <div class="line-content" @click="selectLine(line)">
            <SvgWrapper
              overflow="visible" 
              width="20"
              height="20"
            >
              <defs>
                <LinePointMarker
                  class="line-marker"
                  v-if="line.points[0]"
                  :id="`preset-line-${index}`"
                  position="start"
                  :type="line.points[0]"
                  color="currentColor"
                  :baseSize="2"
                />
                <LinePointMarker
                  class="line-marker"
                  v-if="line.points[1]"
                  :id="`preset-line-${index}`"
                  position="end"
                  :type="line.points[1]"
                  color="currentColor"
                  :baseSize="2"
                />
              </defs>
              <path
                class="line-path"
                :d="line.path" 
                stroke="currentColor" 
                fill="none" 
                stroke-width="2" 
                :stroke-dasharray="line.style === 'solid' ? '0, 0' : '4, 1'"
                stroke-linecap 
                stroke-linejoin 
                stroke-miterlimit 
                :marker-start="line.points[0] ? `url(#${`preset-line-${index}`}-${line.points[0]}-start)` : ''"
                :marker-end="line.points[1] ? `url(#${`preset-line-${index}`}-${line.points[1]}-end)` : ''"
              ></path>
            </SvgWrapper>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import { LINE_LIST, LinePoolItem } from '@/configs/lines'

import LinePointMarker from '@/views/components/element/LineElement/LinePointMarker.vue'

export default defineComponent({
  name: 'line-pool',
  emits: ['select'],
  components: {
    LinePointMarker,
  },
  setup(props, { emit }) {
    const lineList = LINE_LIST

    const selectLine = (line: LinePoolItem) => {
      emit('select', line)
    }

    return {
      lineList,
      selectLine,
    }
  },
})
</script>

<style lang="scss" scoped>
.line-pool {
  width: 220px;
  overflow: auto;
  margin-bottom: -12px;
  margin-right: -12px;
  padding-right: 12px;
}
.category-name {
  width: 100%;
  font-size: 13px;
  margin-bottom: 10px;
  border-left: 4px solid #aaa;
  background-color: #eee;
  padding: 2px 0 2px 10px;
}
.line-list {
  @include flex-grid-layout();

  margin-bottom: 10px;
}
.line-item {
  @include flex-grid-layout-children(5, 19%);

  height: 0;
  padding-bottom: 19%;
  flex-shrink: 0;
  position: relative;
  cursor: pointer;
}
.line-content {
  @include absolute-0();

  display: flex;
  justify-content: center;
  align-items: center;
  color: #999;

  &:hover {
    color: $themeColor;
  }

  svg:not(:root) {
    overflow: visible;
  }
}
</style>