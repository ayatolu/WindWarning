<script setup>
import { reactive, ref, computed } from 'vue'
import { House } from '@element-plus/icons-vue'
import { Document } from '@element-plus/icons-vue'

const countList = ref([
  {
    windFarm_id: 1,
    turbine_id: 1,
    count: 9
  },
  {
    windFarm_id: 1,
    turbine_id: 2,
    count: 8
  },
  {
    windFarm_id: 1,
    turbine_id: 3,
    count: 7
  },
  {
    windFarm_id: 1,
    turbine_id: 4,
    count: 6
  },
  {
    windFarm_id: 1,
    turbine_id: 5,
    count: 5
  },
  {
    windFarm_id: 5,
    turbine_id: 1,
    count: 55
  }
])

// 初始化树形数据
const treeData = reactive([
  {
    windFarm_id: 1,
    label: '化德分散式',
    expanded: false,
    children: [
      {
        turbine_id: 1,
        turbine_name: '#101风机',
        selected: false
      },
      {
        turbine_id: 2,
        turbine_name: '#102风机',
        selected: false
      },
      {
        turbine_id: 3,
        turbine_name: '#103风机',
        selected: false
      },
      {
        turbine_id: 4,
        turbine_name: '#104风机',
        selected: false
      },
      {
        turbine_id: 5,
        turbine_name: '#105风机',
        selected: false
      }
    ]
  },
  {
    windFarm_id: 2,
    label: '化德长顺',
    expanded: false,
    children: []
  },
  {
    windFarm_id: 3,
    label: '协和扎兰屯',
    expanded: false,
    children: []
  },
  {
    windFarm_id: 4,
    label: '扎兰屯二期',
    expanded: false,
    children: []
  },
  {
    windFarm_id: 5,
    label: '丰镇分散式',
    expanded: false,
    children: [
      {
        turbine_id: 1,
        turbine_name: '#101风机',
        selected: false
      },
      {
        turbine_id: 2,
        turbine_name: '#102风机',
        selected: false
      },
      {
        turbine_id: 3,
        turbine_name: '#103风机',
        selected: false
      },
      {
        turbine_id: 4,
        turbine_name: '#104风机',
        selected: false
      },
      {
        turbine_id: 5,
        turbine_name: '#105风机',
        selected: false
      }
    ]
  },
  {
    windFarm_id: 6,
    label: '伊旗风储',
    expanded: false,
    children: []
  }
])

// 计算每个风场和风机的数量
const calculateCount = computed(() => {
  const farmCounts = {}
  const turbineCounts = {}

  // 计算每个风机的数量
  countList.value.forEach((item) => {
    turbineCounts[`${item.windFarm_id}_${item.turbine_id}`] = item.count
  })

  // 计算每个风场的总数量
  countList.value.forEach((item) => {
    if (!farmCounts[item.windFarm_id]) {
      farmCounts[item.windFarm_id] = 0
    }
    farmCounts[item.windFarm_id] += item.count
  })

  return {
    farmCounts,
    turbineCounts
  }
})

// 切换展开/收起状态
const toggleExpand = (node) => {
  node.expanded = !node.expanded
}

// 处理风机节点点击事件
const handleTurbineClick = (turbine, parentNode) => {
  // 取消同一风场下其他风机的选中状态
  parentNode.children.forEach((child) => {
    if (child !== turbine) {
      child.selected = false
    }
  })

  turbine.selected = true
  console.log(turbine.turbine_name)
}
</script>

<template>
  <div class="top">
    <span class="custom-span">
      <el-icon><Document /></el-icon>
      内蒙古公司
    </span>
  </div>
  <div class="tree-container">
    <ul class="tree">
      <li v-for="(node, index) in treeData" :key="index" class="tree-node">
        <div class="tree-label">
          <span @click="toggleExpand(node)" class="expand-icon">
            {{ node.expanded ? '▼' : '▶' }} <el-icon style="font-size: 15px"><House /></el-icon>
          </span>

          <span class="label">{{ node.label }}</span>

          <!-- 风场节点右上角的数量标签 -->
          <span v-if="calculateCount.farmCounts[node.windFarm_id]" class="windFarm-count-badge">
            {{ calculateCount.farmCounts[node.windFarm_id] }}
          </span>
        </div>
        <ul v-if="node.expanded && node.children">
          <li
            v-for="(child, childIndex) in node.children"
            :key="childIndex"
            class="child-node"
            :class="{ 'selected-turbine': child.selected }"
          >
            <div class="tree-label" @click="handleTurbineClick(child, node)">
              <span class="label">{{ child.turbine_name }}</span>

              <!-- 风机节点右上角的数量标签 -->
              <span
                v-if="calculateCount.turbineCounts[`${node.windFarm_id}_${child.turbine_id}`]"
                class="turbine-count-badge"
              >
                {{ calculateCount.turbineCounts[`${node.windFarm_id}_${child.turbine_id}`] }}
              </span>
            </div>
          </li>
        </ul>
      </li>
    </ul>
  </div>
</template>

<style scoped>
.top {
  display: flex;
  justify-content: space-between;
  height: 50px;
  margin-bottom: 10px;
}
.custom-span {
  display: inline-flex;
  align-items: center;
  background-color: #164476;
  color: white;
  padding: 5px 10px;
  font-size: 15px;
  border-radius: 5px;
  position: relative;
  font-weight: bold;
}
.tree-container {
  max-height: 100%;
  border-radius: 5px;
  margin: 10px;
}

.tree {
  list-style-type: none;
  padding-left: 0;
  margin-left: 20px;
}

.tree-node {
  margin-bottom: 25px;
  position: relative;
}
.child-node {
  margin-bottom: 25px;
  margin-left: 40px;
  position: relative;
}
.label {
  background: linear-gradient(
    rgba(30, 96, 120, 0) 0%,
    rgba(30, 96, 120, 0.5) 25%,
    rgba(30, 96, 120, 1) 50%,
    rgba(30, 96, 120, 0.5) 75%,
    rgba(30, 96, 120, 0) 100%
  );
  padding: 5px 12px;
  cursor: pointer;
}

.selected-turbine .label {
  background: linear-gradient(
    rgba(119, 102, 30, 0) 0%,
    rgba(119, 102, 30, 0.5) 25%,
    rgba(119, 102, 30, 1) 50%,
    rgba(119, 102, 30, 0.5) 75%,
    rgba(119, 102, 30, 0) 100%
  );
}

.tree-label {
  display: flex;
  align-items: center;
  gap: 10px;
  position: relative;
}

.expand-icon {
  cursor: pointer;
  font-size: 16px;
  text-align: center;
}

.windFarm-count-badge {
  position: absolute;
  top: -10px;
  left: 140px;
  background-color: #2098b2;
  color: white;
  font-size: 12px;
  padding: 2px 6px;
  border-radius: 3px;
}
.turbine-count-badge {
  position: absolute;
  top: -6px;
  left: 85px;
  background-color: #2098b2;
  color: white;
  font-size: 12px;
  padding: 2px 6px;
  border-radius: 3px;
}
</style>
