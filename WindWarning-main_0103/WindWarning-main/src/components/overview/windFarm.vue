<script setup>
import { ref, computed } from 'vue'
const windFarmList = ref([
  {
    windFarmId: 1,
    windFarmName: '化德分散式'
  },
  {
    windFarmId: 2,
    windFarmName: '化德长顺'
  },
  {
    windFarmId: 3,
    windFarmName: '协和扎兰屯'
  },
  {
    windFarmId: 4,
    windFarmName: '扎兰屯二期'
  },
  {
    windFarmId: 5,
    windFarmName: '丰镇分散式'
  },
  {
    windFarmId: 6,
    windFarmName: '伊旗风储'
  }
])
// 原有的windfarm1的turbines
const originalTurbines = [
  {
    turbineId: 1,
    windFarmId: 1,
    turbineName: '101',
    warning: 2
  },
  {
    turbineId: 2,
    windFarmId: 1,
    turbineName: '102',
    warning: 3
  },
  {
    turbineId: 3,
    windFarmId: 1,
    turbineName: '103',
    warning: 4
  },
  {
    turbineId: 4,
    windFarmId: 1,
    turbineName: '104',
    warning: 6
  },
  {
    turbineId: 5,
    windFarmId: 1,
    turbineName: '105',
    warning: 2
  },
  {
    turbineId: 6,
    windFarmId: 1,
    turbineName: '106',
    warning: 9
  },
  {
    turbineId: 7,
    windFarmId: 1,
    turbineName: '107',
    warning: 2
  },
  {
    turbineId: 8,
    windFarmId: 1,
    turbineName: '108',
    warning: 1
  },
  {
    turbineId: 9,
    windFarmId: 1,
    turbineName: '109',
    warning: 2
  },
  {
    turbineId: 10,
    windFarmId: 1,
    turbineName: '110',
    warning: 3
  },
  {
    turbineId: 11,
    windFarmId: 1,
    turbineName: '111',
    warning: 2
  },
  {
    turbineId: 12,
    windFarmId: 1,
    turbineName: '112',
    warning: 5
  },
  {
    turbineId: 13,
    windFarmId: 1,
    turbineName: '113',
    warning: 0
  },
  {
    turbineId: 14,
    windFarmId: 1,
    turbineName: '114',
    warning: 5
  },
  {
    turbineId: 15,
    windFarmId: 1,
    turbineName: '115',
    warning: 5
  }
]

const turbineList = ref([
  // 直接展开原有的 turbines
  ...originalTurbines,

  // 动态生成 wind farm 2, 3, 4, 5, 6 的 turbines
  ...[2, 3, 4, 5, 6].flatMap((windFarmId) =>
    Array.from({ length: 10 }, (_, index) => ({
      turbineId: originalTurbines.length + 1 + (windFarmId - 2) * 10 + index,
      windFarmId,
      turbineName: `${index + 1 < 10 ? '0' : ''}${index + 1}`,
      warning: 0
    }))
  )
])

// 根据windFarmList和turbineList动态生成按钮组
const buttonGroups = computed(() => {
  return windFarmList.value.map((windFarm) => {
    const turbinesInFarm = turbineList.value.filter((turbine) => turbine.windFarmId === windFarm.windFarmId)
    const maxWarning = Math.max(...turbinesInFarm.map((turbine) => turbine.warning))

    return {
      name: windFarm.windFarmName,
      buttons: turbinesInFarm.map((turbine) => ({
        label: turbine.turbineName,
        warning: turbine.warning,
        percentage: maxWarning > 0 ? (turbine.warning / maxWarning) * 100 : 0
      }))
    }
  })
})
</script>

<template>
  <div class="top">
    <span class="custom-span"> 风机预警矩阵 </span>
  </div>
  <div class="container">
    <div class="row" v-for="(group, groupIndex) in buttonGroups" :key="groupIndex">
      <span class="row-title">{{ group.name }}</span>
      <div class="button-group">
        <div v-for="(button, buttonIndex) in group.buttons" :key="buttonIndex" class="button-container">
          <button :class="{ active: button.active }">{{ button.label }}</button>
          <div class="progress-bar">
            <div class="progress" :style="{ width: button.percentage + '%' }">
              <span class="progress-text" v-if="button.warning > 0">{{ button.warning }}</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.top {
  display: flex;
  justify-content: space-between;
  height: 50px;
}

.custom-span {
  display: inline-flex;
  align-items: center;
  background-color: #164476;
  color: white;
  padding: 5px 10px;
  font-size: 17px;
  border-radius: 5px;
  position: relative;
  font-weight: bold;
}

.container {
  color: #2043f2;
  padding: 20px;
  border-radius: 10px;
  overflow-x: hidden;
}

.row {
  margin-left: 15px;
  margin-bottom: 40px;
  display: flex;
  align-items: center;
  border-top: none;
  border-left: none;
  border-right: none;
  border-bottom: 1px solid #0a85d9;
  background: linear-gradient(90deg, transparent, #0a85d9 50%, transparent);
  box-shadow: 0 2px 3px rgba(0, 0, 0, 0.1);
}

.row-title {
  display: inline-flex;
  justify-content: center;
  align-items: center;
  padding: 10px 10px;
  font-size: 16px;
  color: #ffffff;
  background: linear-gradient(90deg, #184558, #20667f);
  margin-right: 8px;
  border: 1px solid #20667f;
  border-radius: 5px;
  text-align: center;
  cursor: pointer;
  white-space: nowrap;
  min-width: 100px;
  height: 40px;
  line-height: 40px;
}

.button-group {
  display: flex;
  flex-wrap: wrap;
  gap: 15px;
  justify-content: flex-start;
}

button {
  width: 52px;
  height: 40px;
  background: linear-gradient(90deg, #1f5b4c, #1c594e);
  color: #ffffff;
  border: 1px solid #39a58a;
  border-radius: 5px;
  font-size: 16px;
}
.button-container {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.progress-bar {
  width: 95%;
  background-color: transparent;
  border: 1px solid orange;
  border-radius: 2px;
  margin-top: 1px;
  height: 10px;
  position: relative;
}

.progress {
  background-color: orange;
  height: 100%;
  border-radius: 1px;
  position: relative;
}

.progress-text {
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
  color: black;
  font-size: 12px;
  line-height: 10px;
}
</style>
