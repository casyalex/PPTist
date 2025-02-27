<template>
  <div class="symbol-panel">
    <div class="tabs">
      <div 
        class="tab" 
        :class="{ 'active': selectedSymbolKey === item.key }" 
        v-for="item in symbolPoolList" 
        :key="item.key"
        @click="selectedSymbolKey = item.key"
      >{{item.label}}</div>
    </div>
    <div class="pool">
      <div class="symbol-item" v-for="(item, index) in symbolPool" :key="index" @click="selectSymbol(item)">
        <div class="symbol">{{item}}</div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { computed, defineComponent, ref } from 'vue'
import { SYMBOL_LIST } from '@/configs/symbol'
import emitter, { EmitterEvents } from '@/utils/emitter'

const symbolPoolList = SYMBOL_LIST

export default defineComponent({
  name: 'symbol-panel',
  setup() {
    const selectedSymbolKey = ref(symbolPoolList[0].key)
    const symbolPool = computed(() => {
      const selectedSymbol = symbolPoolList.find(item => item.key === selectedSymbolKey.value)
      return selectedSymbol?.children || []
    })

    const selectSymbol = (item: string) => {
      emitter.emit(EmitterEvents.RICH_TEXT_COMMAND, { command: 'insert', value: item })
    }

    return {
      symbolPoolList,
      symbolPool,
      selectedSymbolKey,
      selectSymbol,
    }
  },
})
</script>

<style lang="scss" scoped>
.symbol-panel {
  height: 100%;
  display: flex;
  flex-direction: column;

  .tabs {
    display: flex;
    justify-content: space-around;
    align-items: center;
    border-bottom: 1px solid $borderColor;
    margin-bottom: 8px;
  }
  .tab {
    padding: 6px 10px;
    border-bottom: 2px solid transparent;
    cursor: pointer;

    &.active {
      border-bottom: 2px solid $themeColor;
    }
  }
  .pool {
    padding: 5px 12px;
    margin: 0 -12px;
    flex: 1;
    font-size: 18px;

    @include overflow-overlay();
    @include flex-grid-layout();
  }
  .symbol-item {
    @include flex-grid-layout-children(5, 18%);

    height: 0;
    padding-bottom: 18%;
    position: relative;
    cursor: pointer;
    border: 1px solid $borderColor;

    &:hover {
      color: $themeColor;
    }

    .symbol {
      @include absolute-0();

      display: flex;
      justify-content: center;
      align-items: center;
      background-color: #fff;
    }
  }
}
</style>
