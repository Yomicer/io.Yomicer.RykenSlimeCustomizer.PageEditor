<template>
  <div class="item-yml-form">
    <el-form label-width="auto" :model="formData" class="full-width-form">
      <!-- 物品ID -->
      <el-form-item label="物品ID (大写字母/下划线/数字):" required>
        <el-input v-model="formData.itemId" placeholder="例如：MAGIC_ITEM_1"></el-input>
      </el-form-item>

      <!-- 别名设置 -->
      <el-form-item label="启用别名(大写字母/下划线/数字):">
        <el-checkbox v-model="formData.aliasEnabled"></el-checkbox>
        <el-input v-model="formData.aliasInput" :disabled="!formData.aliasEnabled" placeholder="别名（默认不携带别名）"></el-input>
      </el-form-item>

      <!-- lateInit 设置 -->
      <el-form-item label="最后加载:(非必填项)">
        <el-select v-model="formData.lateInit" placeholder="不选择(默认否)">
          <el-option value="">不选择(默认否)</el-option>
          <el-option value="false">否</el-option>
          <el-option value="true">是</el-option>
        </el-select>
      </el-form-item>

      <!-- 注册条件 -->
      <h4>注册条件(非必填项)：</h4>
      <el-form-item label="依赖插件">
        <el-input v-model="formData.hasPlugin" placeholder="例如：MagicExpansion"></el-input>
      </el-form-item>
      <el-form-item label="冲突插件">
        <el-input v-model="formData.notHasPlugin" placeholder="例如：MagicExpansion"></el-input>
      </el-form-item>
      <el-form-item label="版本条件">
        <el-input v-model="formData.versionCondition" placeholder=">= 1.16.5"></el-input>
      </el-form-item>
      <el-form-item label="boolean类型config条件">
        <el-input v-model="formData.configBoolean" placeholder="somesetting"></el-input>
      </el-form-item>
      <el-form-item label="int类型config条件">
        <el-input v-model="formData.configInt" placeholder="points >= 10"></el-input>
      </el-form-item>
      <el-form-item label="string类型config条件">
        <el-input v-model="formData.configString" placeholder="key value"></el-input>
      </el-form-item>
      <el-form-item label="基于某粘液物品注册">
        <el-input v-model="formData.itemIfExist" placeholder="MAGIC_SWORD"></el-input>
      </el-form-item>

      <!-- 基础设置 -->
      <h4>基础设置：</h4>
      <el-form-item label="物品类型(建议选择)">
        <el-select v-model="formData.itemType" placeholder="不选择默认原版材质">
          <el-option label="原版材质" value="mc"></el-option>
          <el-option label="skull_base64" value="skull_base64"></el-option>
          <el-option label="skull_url" value="skull_url"></el-option>
          <el-option label="skull_hash" value="skull_hash"></el-option>
          <el-option label="粘液物品" value="slimefun"></el-option>
          <el-option label="保存的物品" value="saveditem"></el-option>
        </el-select>
      </el-form-item>
      <el-form-item label="材质">
        <el-input v-model="formData.material" placeholder="DIAMOND/8adb25ab9976d89d0bd8118d72c1c06bb907060c1e02a729b652d1e86b1ebbbc"></el-input>
      </el-form-item>
      <el-form-item label="物品名称(非必填项)">
        <el-input v-model="formData.name" placeholder="例如：&a魔法剑"></el-input>
      </el-form-item>
      <el-form-item label="配方类型">
        <el-input v-model="formData.recipeType" placeholder="例如：SMELTERY/NULL"></el-input>
      </el-form-item>
      <el-form-item label="物品组">
        <el-input v-model="formData.itemGroup" placeholder="例如：magic_gruop"></el-input>
      </el-form-item>
      <el-form-item label="是否可放置(非必填项)">
        <el-select v-model="formData.placeable" placeholder="不选择(默认无法放置)">
          <el-option label="是" value="true"></el-option>
          <el-option label="否" value="false"></el-option>
        </el-select>
      </el-form-item>

      <!-- Lore -->
      <el-form-item label="物品描述(非必填项)">
        <el-button type="primary" @click="addLore()">添加 Lore</el-button>
        <div v-for="(lore, idx) in formData.lores" :key="idx" class="lore-input-container">
          <el-input v-model="lore.text" style="width: 100%; margin-right: 10px;"></el-input>
          <el-button type="danger" size="small" @click="removeLore(idx)">删除</el-button>
        </div>
      </el-form-item>

      <!-- 配方 -->
      <el-form-item label="配方(非必填项)">
        <el-button type="primary" @click="addRecipe()" :disabled="formData.recipes.length >= 9">
          添加配方项 (最多9个)
        </el-button>
        <div v-for="(recipe, idx) in formData.recipes" :key="idx" class="recipe-item">
          <el-select v-model="recipe.type" style="width: 200px; margin-right: 10px;" placeholder="不选择(默认原版物品)">
            <el-option label="原版物品" value="mc"></el-option>
            <el-option label="粘液物品" value="slimefun"></el-option>
            <el-option label="保存的物品" value="saveditem"></el-option>
          </el-select>
          <el-input v-model="recipe.material" placeholder="如: DIAMOND"
            style="width: 300px; margin-right: 10px;"></el-input>
          <el-button type="danger" size="small" @click="removeRecipe(idx)">删除</el-button>
        </div>
      </el-form-item>

      <!-- 高级设置 -->
      <h4>高级设置：</h4>
      <el-form-item label="电容量(非必填项):">
        <el-input id="energyCapacityInput" v-model.number="formData.energyCapacity" type="number"
          placeholder="容量最大为 2147483647"></el-input>
      </el-form-item>
      <el-form-item label="辐射强度(非必填项):">
        <el-select id="radiationSelect" v-model="formData.radiation" placeholder="不选择(默认没有该属性)">
          <el-option value="">不选择(默认没有该属性)</el-option>
          <el-option value="VERY_DEADLY">极高</el-option>
          <el-option value="HIGH">高</el-option>
          <el-option value="LOW">低</el-option>
          <el-option value="MODERATE">中</el-option>
          <el-option value="VERY_DEADLY">致死的</el-option>
        </el-select>
      </el-form-item>
      <el-form-item label="彩虹方块(非必填项):">
        <el-select id="rainbowSelect" v-model="formData.rainbow" placeholder="不选择(默认没有该属性)">
          <el-option value="">不选择(默认没有该属性)</el-option>
          <el-option value="GLASS_PANE">玻璃板</el-option>
          <el-option value="GLASS">玻璃</el-option>
          <el-option value="STAINED_GLASS">彩色玻璃</el-option>
          <el-option value="STAINED_GLASS_PANE">彩色玻璃板</el-option>
          <el-option value="WOOL">羊毛</el-option>
          <el-option value="TERRACOTTA">陶瓦</el-option>
          <el-option value="TERRACOTTA_ALL">所有陶瓦</el-option>
          <el-option value="GLAZED_TERRACOTTA">带釉陶瓦</el-option>
        </el-select>
      </el-form-item>
      <el-form-item label="防凋零爆炸(非必填项):">
        <el-select id="antiWitherSelect" v-model="formData.antiWither" placeholder="不选择(默认没有该属性)">
          <el-option value="">不选择(默认没有该属性)</el-option>
          <el-option value="true">是</el-option>
          <el-option value="false">否</el-option>
        </el-select>
      </el-form-item>
      <el-form-item label="灵魂绑定(非必填项):">
        <el-select id="soulboundSelect" v-model="formData.soulbound" placeholder="不选择(默认没有该属性)">
          <el-option value="">不选择(默认没有该属性)</el-option>
          <el-option value="true">是</el-option>
          <el-option value="false">否</el-option>
        </el-select>
      </el-form-item>
      <el-form-item label="猪灵物品交易概率(非必填项):">
        <el-input id="piglinTradeChanceInput" v-model.number="formData.piglinTradeChance" type="number"
          placeholder="0-100"></el-input>
      </el-form-item>
      <el-form-item label="人造类物品(非必填项):">
        <el-select id="vanillaSelect" v-model="formData.vanilla" placeholder="不选择(默认不是人造类物品)">
          <el-option value="">不选择(默认不是人造类物品)</el-option>
          <el-option value="true">是</el-option>
          <el-option value="false">否</el-option>
        </el-select>
      </el-form-item>
      <el-form-item label="隐藏物品(非必填项):">
        <el-select id="hiddenSelect" v-model="formData.hidden" placeholder="不选择(默认不隐藏)">
          <el-option value="">不选择(默认不隐藏)</el-option>
          <el-option value="true">是</el-option>
          <el-option value="false">否</el-option>
        </el-select>
      </el-form-item>
      <el-form-item label="方块掉落物品(非必填项，但是与下方2项连锁):">
        <el-input id="dropFromInput" v-model="formData.dropFrom" placeholder="例如: diamond"></el-input>
      </el-form-item>
      <el-form-item label="掉落概率(非必填项):">
        <el-input id="dropChanceInput" v-model.number="formData.dropChance" type="number"
          placeholder="0-100"></el-input>
      </el-form-item>
      <el-form-item label="掉落数量(非必填项):">
        <el-input id="dropAmountInput" v-model="formData.dropAmount" placeholder="1-3 可为区间或定值"></el-input>
      </el-form-item>

      <!-- 提交按钮 -->
      <el-form-item>
        <el-button type="success" @click="generateYaml()">生成 YAML</el-button>
      </el-form-item>

      <!-- YAML 输出区域 -->
      <el-form-item label="生成的配置文件">
        <div class="yaml-output-container">
          <el-input v-model="yamlOutput" type="textarea" :rows="15" readonly class="yaml-textarea"></el-input>
          <div class="button-group">
            <el-button type="primary" @click="copyYaml">复制</el-button>
            <el-button type="success" @click="downloadYaml">下载</el-button>
          </div>
        </div>
      </el-form-item>
    </el-form>
  </div>
</template>

<script setup>
import { reactive, ref } from 'vue'
import { ElMessage } from 'element-plus'

// 表单数据对象
const formData = reactive({
  itemId: '',
  aliasEnabled: false,
  aliasInput: '',
  lateInit: '',
  hasPlugin: '',
  notHasPlugin: '',
  versionCondition: '',
  configBoolean: '',
  configInt: '',
  configString: '',
  itemIfExist: '',
  itemType: '',
  material: '',
  name: '',
  recipeType: '',
  itemGroup: '',
  placeable: '',
  lores: [],
  recipes: [],
  energyCapacity: '',
  radiation: '',
  rainbow: '',
  antiWither: '',
  soulbound: '',
  piglinTradeChance: '',
  vanilla: '',
  hidden: '',
  dropFrom: '',
  dropChance: '',
  dropAmount: ''
})
const yamlOutput = ref('')

// 添加 Lore
function addLore() {
  formData.lores.push({ text: '' })
}
function removeLore(index) {
  formData.lores.splice(index, 1)
}

// 添加 Recipe 项目
function addRecipe() {
  if (formData.recipes.length < 9) {
    formData.recipes.push({ type: '', material: '' })
  } else {
    ElMessage.warning('最多只能添加 9 个配方项')
  }
}
function removeRecipe(index) {
  formData.recipes.splice(index, 1)
}

// 生成 YAML
function generateYaml() {
  let yaml = ''

  if (!formData.itemId.trim()) {
    ElMessage.error('请输入物品ID')
    return
  }

  yaml += `${formData.itemId}:\n`

  if (formData.aliasEnabled && formData.aliasInput.trim()) {
    yaml += `  id_alias: ${formData.aliasInput}\n`
  }

  if (formData.lateInit !== '') {
    yaml += `  lateInit: ${formData.lateInit}\n`
  }

  // register conditions
  const hasConditions =
    formData.hasPlugin.trim() ||
    formData.notHasPlugin.trim() ||
    formData.versionCondition.trim() ||
    formData.configBoolean.trim() ||
    formData.configInt.trim() ||
    formData.configString.trim() ||
    formData.itemIfExist.trim()

  if (hasConditions) {
    yaml += `  register:\n`
    yaml += `    warn: true\n`
    yaml += `    conditions:\n`
    if (formData.hasPlugin) yaml += `      - "hasplugin ${formData.hasPlugin}"\n`
    if (formData.notHasPlugin) yaml += `      - "!hasplugin ${formData.notHasPlugin}"\n`
    if (formData.versionCondition) yaml += `      - "version ${formData.versionCondition}"\n`
    if (formData.configBoolean) yaml += `      - "config.boolean ${formData.configBoolean}"\n`
    if (formData.configInt) yaml += `      - "config.int ${formData.configInt}"\n`
    if (formData.configString) yaml += `      - "config.string ${formData.configString}"\n`
    if (formData.itemIfExist) yaml += `      - "itemexist ${formData.itemIfExist}"\n`
  }

  // item:
  if (formData.itemType && formData.material) {
    yaml += `  item:\n`
    yaml += `    material_type:: ${formData.itemType}\n`
    yaml += `    material: ${formData.material}\n`
    yaml += `    name: "${formData.name || ''}"\n`
  }

  // recipe_type
  if (formData.recipeType) {
    yaml += `  recipe_type: ${formData.recipeType}\n`
  }

  // item_group
  if (formData.itemGroup) {
    yaml += `  item_group: ${formData.itemGroup}\n`
  }

  // placeable
  if (formData.placeable !== '') {
    yaml += `  placeable: ${formData.placeable}\n`
  }

  // lore
  if (formData.lores.length > 0) {
    yaml += `  lore:\n`
    formData.lores.forEach(l => {
      if (l.text.trim()) {
        yaml += `    - "${l.text}"\n`
      }
    })
  }

  // recipe
  if (formData.recipes.length > 0) {
    yaml += `  recipe:\n`
    formData.recipes.forEach((r, i) => {
      if (r.type && r.material) {
        yaml += `    ${i + 1}:\n`
        yaml += `      material_type: ${r.type}\n`
        yaml += `      material: ${r.material}\n`
      }
    })
  }

  // 其他字段...
  if (formData.energyCapacity) yaml += `  energy_capacity: ${formData.energyCapacity}\n`
  if (formData.radiation) yaml += `  radiation: ${formData.radiation}\n`
  if (formData.rainbow) yaml += `  rainbow: ${formData.rainbow}\n`
  if (formData.antiWither) yaml += `  anti_wither: ${formData.antiWither}\n`
  if (formData.soulbound) yaml += `  soulbound: ${formData.soulbound}\n`
  if (formData.piglinTradeChance) yaml += `  piglin_trade_chance: ${formData.piglinTradeChance}\n`
  if (formData.vanilla) yaml += `  vanilla: ${formData.vanilla}\n`
  if (formData.hidden) yaml += `  hidden: ${formData.hidden}\n`
  if (formData.dropFrom) yaml += `  drop_from: ${formData.dropFrom}\n`
  if (formData.dropChance) yaml += `  drop_chance: ${formData.dropChance}\n`
  if (formData.dropAmount) yaml += `  drop_amount: ${formData.dropAmount}\n`

  yamlOutput.value = yaml

}




// 复制 YAML 内容到剪贴板
const copyYaml = () => {
    if (!yamlOutput.value.trim()) {
      ElMessage.warning('没有可复制的内容')
      return
    }

    navigator.clipboard
      .writeText(yamlOutput.value)
      .then(() => {
        ElMessage.success('已复制到剪贴板')
      })
      .catch(() => {
        ElMessage.error('复制失败')
      })
  }

  // 下载 YAML 文件
  const downloadYaml = () => {
    if (!yamlOutput.value.trim()) {
      ElMessage.warning('没有内容可以下载')
      return
    }

    const id = formData.itemId || 'item'
    const blob = new Blob([yamlOutput.value], { type: 'text/yaml' })
    const url = URL.createObjectURL(blob)
    const link = document.createElement('a')
    link.href = url
    link.download = `${id}.yml`
    link.click()
    URL.revokeObjectURL(url)
  }






</script>

<style scoped>
.full-width-form {
  width: 100%;
  padding: 20px;
}

/* 确保每个表单项占据全宽 */
.el-form-item {
  width: 200%;
}

/* 自动计算标签宽度 */
.el-form--label-left .el-form-item__label {
  width: auto !important;
}

.el-form-item__content {
  width: calc(100% - 150px);
  /* 自动计算宽度以适应内容 */
}

/* 对所有输入框应用统一宽度 */
.el-input,
.el-select {
  width: 100%;
}




/* 整个 lore 列表作为一个 block 容器 */
.lore-list {
  display: block;
  width: 100%;
}

/* 每个 lore 行必须独占一行，并且内部元素水平排列 */
.lore-input-container {
  display: flex;
  align-items: center;
  margin-bottom: 10px;
  width: 100%; /* 占满整行 */
}

/* 输入框占据大部分空间 */
.lore-input {
  flex: 1; /* 自动伸缩填充剩余空间 */
  margin-right: 10px;
}

/* 删除按钮保持固定大小 */
.lore-input-container .el-button {
  flex: 0 0 auto;
}



/* 确保配方项列表整体为块级容器 */
.recipe-list {
  display: block;
  width: 100%;
}

/* 每个配方项独占一行，内部元素水平排列 */
.recipe-item {
  display: flex;
  align-items: center;
  margin-bottom: 10px;
  width: 100%;
  flex-wrap: nowrap;
}

/* 可选：美化输入组件之间的间距 */
.recipe-item .el-select,
.recipe-item .el-input {
  margin-right: 10px;
}



/* yml显示区域 */
.yaml-output-container {
  position: relative;
}

.yaml-textarea {
  width: 388%;
  border: 1px solid #999;
  border-radius: 6px;
  background-color: #f5f7fa;
  font-family: monospace;
  font-size: 14px;
  color: #333;
  resize: none;
}

.button-group {
  margin-top: 10px;
  display: flex;
  justify-content: space-between;
}

/* 可选：给按钮一点间距 */
.button-group .el-button {
  flex: 1;
  margin-right: 10px;
}

.button-group .el-button:last-child {
  margin-right: 0;
}
</style>