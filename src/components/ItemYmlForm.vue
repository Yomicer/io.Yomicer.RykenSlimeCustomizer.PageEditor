<template>
    <div>
      <!-- Tabs -->
      <div class="tabs">
        <button
          v-for="(tab, index) in tabs"
          :key="index"
          class="tablinks"
          :class="{ active: currentTab === tab.id }"
          @click="currentTab = tab.id"
        >
          {{ tab.label }}
        </button>
      </div>
  
      <!-- Tab Content -->
      <div v-show="currentTab === 'Content1'" class="content">
        <h2 class="section-title">item.yml</h2>
  
        <!-- 表单开始 -->
        <form id="configForm">
          <!-- 物品ID -->
          <div class="form-group">
            <label for="itemIdInput" class="wide-label">物品ID(大写字母/下划线/数字):</label>
            <input id="itemIdInput" type="text" v-model="formData.itemId" placeholder="例如：MAGIC_ITEM_1" />
          </div>
  
          <!-- 别名设置 -->
          <div class="form-group">
            <label for="aliasInput" class="wide-label">启用别名(大写字母/下划线/数字):</label>
            <input id="aliasInput" type="text" v-model="formData.alias" placeholder="别名（默认不携带别名）" />
          </div>
  
          <!-- lateInit 设置 -->
          <div class="form-group">
            <label for="lateInitSelect">最后加载:</label>
            <select id="lateInitSelect" v-model="formData.lateInit">
              <option value="">不选择(默认否)</option>
              <option value="false">否</option>
              <option value="true">是</option>
            </select>
          </div>
  
          <!-- 注册条件 -->
          <div class="form-group custom-margin">
            <h4>注册条件：</h4>
            <label for="hasPluginInput">依赖于插件</label>
            <input id="hasPluginInput" type="text" v-model="formData.hasplugin" placeholder="例如：hasplugin MagicExpansion(默认不启用)" /><br>
  
            <label for="notHasPluginInput">与插件冲突:</label>
            <input id="notHasPluginInput" type="text" v-model="formData.notHasplugin" placeholder="例如：!hasplugin MagicExpansion(默认不启用)" /><br>
  
            <label for="versionConditionInput">版本条件:</label>
            <input id="versionConditionInput" type="text" v-model="formData.versionCondition" placeholder="例如：>= 1.16(默认不启用)" /><br>
  
            <label for="configBooleanInput">Boolean类型config判断:</label>
            <input id="configBooleanInput" type="text" v-model="formData.configBoolean" placeholder="例如：someSetting(默认不启用)" /><br>
  
            <label for="configIntInput">Int类型config判断:</label>
            <input id="configIntInput" type="text" v-model="formData.configInt" placeholder="例如：someNumber >= 5(默认不启用)" /><br>
  
            <label for="configStringInput">String类型config判断:</label>
            <input id="configStringInput" type="text" v-model="formData.configString" placeholder="例如：someKey someValue(默认不启用)" /><br>
  
            <label for="itemIfExist">基于某粘液物品注册:</label>
            <input id="itemIfExist" type="text" v-model="formData.itemIfExist" placeholder="例如：MAGIC_ITEM_1(默认不启用)" />
          </div>
  
          <!-- 基础设置 -->
          <div class="form-group">
            <label for="itemTypeSelect">物品类型:</label>
            <select id="itemTypeSelect" v-model="formData.itemType">
              <option value="mc">原版物品</option>
              <option value="skull_base64">skull_base64</option>
              <option value="skull_url">skull_url</option>
              <option value="skull_hash">skull_hash</option>
              <option value="slimefun">粘液物品</option>
              <option value="saveditem">保存物品</option>
            </select>
          </div>
  
          <div class="form-group">
            <label for="materialInput">材质:</label>
            <input id="materialInput" type="text" v-model="formData.material" placeholder="材质（例如：DIAMOND）" />
          </div>
  
          <div class="form-group">
            <label for="materialName">物品名字:</label>
            <input id="materialName" type="text" v-model="formData.name" placeholder="例如：&a魔法道具" />
          </div>
  
          <div class="form-group">
            <label for="recipeTypeInput">配方类型:</label>
            <input id="recipeTypeInput" type="text" v-model="formData.recipeType" placeholder="请输入配方类型如：SMELTERY " />
          </div>
  
          <div class="form-group">
            <label for="itemGroupInput">物品组别:</label>
            <input id="itemGroupInput" type="text" v-model="formData.itemGroup" placeholder="例如：magic_group_1" />
          </div>
  
          <div class="form-group">
            <label for="placeableInput">是否可以放置:</label>
            <select id="placeableInput" v-model="formData.placeable">
              <option value="">不选择(默认否)</option>
              <option value="false">否</option>
              <option value="true">是</option>
            </select>
          </div>
  
          <!-- Lore 输入框容器 -->
          <div class="form-group">
            <label>物品描述:</label>
            <button type="button" @click="addLore()">添加 Lore</button>
            <div v-for="(lore, idx) in formData.lores" :key="idx" class="lore-item lore-input-container">
              <input v-model="lore.text" />
              <button type="button" @click="removeLore(idx)">删除</button>
            </div>
          </div>
  
          <!-- Recipe 输入框容器 -->
          <div class="form-group">
            <label>配方:</label>
            <button type="button" @click="addRecipe()">添加条数</button>
            <div v-for="(recipe, idx) in formData.recipes" :key="idx" class="recipe-item">
              <input v-model="recipe.input" />
              <button type="button" @click="removeRecipe(idx)">删除</button>
            </div>
          </div>
  
          <!-- 高级设置 -->
          <div class="form-group">
            <label for="energyCapacityInput">电容量:</label>
            <input id="energyCapacityInput" type="number" v-model.number="formData.energyCapacity" placeholder="容量最大为 2147483647" />
          </div>
  
          <div class="form-group">
            <label for="radiationSelect">辐射强度:</label>
            <select id="radiationSelect" v-model="formData.radiation">
              <option value="">不选择(默认没有该属性)</option>
              <option value="VERY_DEADLY">极高</option>
              <option value="HIGH">高</option>
              <option value="LOW">低</option>
              <option value="MODERATE">中</option>
            </select>
          </div>
  
          <div class="form-group">
            <label for="rainbowSelect">彩虹方块:</label>
            <select id="rainbowSelect" v-model="formData.rainbow">
              <option value="">不选择(默认没有该属性)</option>
              <option value="GLASS_PANE">玻璃板</option>
              <option value="GLASS">玻璃</option>
              <option value="STAINED_GLASS">彩色玻璃</option>
              <option value="STAINED_GLASS_PANE">彩色玻璃板</option>
              <option value="WOOL">羊毛</option>
              <option value="TERRACOTTA">陶瓦</option>
              <option value="TERRACOTTA_ALL">所有陶瓦</option>
              <option value="GLAZED_TERRACOTTA">带釉陶瓦</option>
            </select>
          </div>
  
          <div class="form-group">
            <label for="antiWitherSelect">防凋零爆炸:</label>
            <select id="antiWitherSelect" v-model="formData.antiWither">
              <option value="">不选择(默认没有该属性)</option>
              <option value="true">是</option>
              <option value="false">否</option>
            </select>
          </div>
  
          <div class="form-group">
            <label for="soulboundSelect">灵魂绑定:</label>
            <select id="soulboundSelect" v-model="formData.soulbound">
              <option value="">不选择(默认没有该属性)</option>
              <option value="true">是</option>
              <option value="false">否</option>
            </select>
          </div>
  
          <div class="form-group">
            <label for="piglinTradeChanceInput">猪灵物品交易概率:</label>
            <input id="piglinTradeChanceInput" type="number" v-model.number="formData.piglinTradeChance" placeholder="0-100" />
          </div>
  
          <div class="form-group">
            <label for="vanillaSelect">人造类物品:</label>
            <select id="vanillaSelect" v-model="formData.vanilla">
              <option value="">不选择(默认不是人造类物品)</option>
              <option value="true">是</option>
              <option value="false">否</option>
            </select>
          </div>
  
          <div class="form-group">
            <label for="hiddenSelect">隐藏物品:</label>
            <select id="hiddenSelect" v-model="formData.hidden">
              <option value="">不选择(默认不隐藏)</option>
              <option value="true">是</option>
              <option value="false">否</option>
            </select>
          </div>
  
          <div class="form-group">
            <label for="dropFromInput">方块掉落物品:</label>
            <input id="dropFromInput" type="text" v-model="formData.dropFrom" placeholder="例如: diamond" />
          </div>
  
          <div class="form-group">
            <label for="dropChanceInput">掉落概率:</label>
            <input id="dropChanceInput" type="number" v-model.number="formData.dropChance" placeholder="0-100" />
          </div>
  
          <div class="form-group">
            <label for="dropAmountInput">掉落数量:</label>
            <input id="dropAmountInput" type="text" v-model="formData.dropAmount" placeholder="1-3 可为区间或定值" />
          </div>
  
          <!-- 生成按钮 -->
          <button type="button" @click="generateYaml">生成</button>
        </form>
  
        <!-- YAML预览区域 -->
        <div id="yamlPreview" style="border: 1px solid #ccc; padding: 10px; margin-top: 20px;">
          <h3>生成的配置预览：</h3>
          <pre id="yamlContent">{{ yamlOutput }}</pre>
        </div>
      </div>
  
      <!-- 暂未开放页面 -->
      <div v-show="currentTab === 'Content2'" class="content">
        <p>这部分内容暂未开放，请等待后续更新。</p>
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref } from 'vue'
  
  const currentTab = ref('Content1')
  const tabs = [
    { id: 'Content1', label: 'item.yml' },
    { id: 'Content2', label: '敬请期待...' }
  ]
  
  const formData = ref({
    itemId: '',
    alias: '',
    lateInit: '',
    hasplugin: '',
    notHasplugin: '',
    versionCondition: '',
    configBoolean: '',
    configInt: '',
    configString: '',
    itemIfExist: '',
    itemType: 'mc',
    material: '',
    name: '',
    recipeType: '',
    itemGroup: '',
    placeable: '',
    energyCapacity: null,
    radiation: '',
    rainbow: '',
    antiWither: '',
    soulbound: '',
    piglinTradeChance: null,
    vanilla: '',
    hidden: '',
    dropFrom: '',
    dropChance: null,
    dropAmount: '',
    lores: [],
    recipes: []
  })
  
  const yamlOutput = ref('')
  
  function addLore () {
    formData.value.lores.push({ text: '' })
  }
  
  function removeLore (index) {
    formData.value.lores.splice(index, 1)
  }
  
  function addRecipe () {
    formData.value.recipes.push({ input: '' })
  }
  
  function removeRecipe (index) {
    formData.value.recipes.splice(index, 1)
  }
  
  function generateYaml () {
    const result = []
  
    // 主体部分
    if (formData.value.itemId) result.push(`${formData.value.itemId}:`)
    else result.push('ITEM_ID_HERE:') // 提示需要填写 ID
  
    if (formData.value.alias) result.push(`  alias: ${formData.value.alias}`)
    if (formData.value.lateInit) result.push(`  late_init: ${formData.value.lateInit}`)
    if (formData.value.hasplugin) result.push(`  hasplugin: ${formData.value.hasplugin}`)
    if (formData.value.notHasplugin) result.push(`  !hasplugin: ${formData.value.notHasplugin}`)
    if (formData.value.versionCondition) result.push(`  version_condition: ${formData.value.versionCondition}`)
    if (formData.value.configBoolean) result.push(`  config_boolean: ${formData.value.configBoolean}`)
    if (formData.value.configInt) result.push(`  config_int: ${formData.value.configInt}`)
    if (formData.value.configString) result.push(`  config_string: ${formData.value.configString}`)
    if (formData.value.itemIfExist) result.push(`  item_if_exist: ${formData.value.itemIfExist}`)
  
    result.push(`  type: ${formData.value.itemType}`)
    if (formData.value.material) result.push(`  material: ${formData.value.material}`)
    if (formData.value.name) result.push(`  name: "${formData.value.name}"`)
  
    if (formData.value.recipeType) result.push(`  recipe_type: ${formData.value.recipeType}`)
    if (formData.value.itemGroup) result.push(`  item_group: ${formData.value.itemGroup}`)
    if (formData.value.placeable) result.push(`  placeable: ${formData.value.placeable}`)
  
    if (formData.value.lores.length > 0) {
      result.push('  lore:')
      formData.value.lores.forEach(lore => {
        result.push(`    - "&f${lore.text}"`)
      })
    }
  
    if (formData.value.recipes.length > 0) {
      result.push('  recipe:')
      formData.value.recipes.forEach(recipe => {
        result.push(`    - "${recipe.input}"`)
      })
    }
  
    if (formData.value.energyCapacity !== null && formData.value.energyCapacity !== '') {
      result.push(`  energy_capacity: ${formData.value.energyCapacity}`)
    }
  
    if (formData.value.radiation) result.push(`  radiation: ${formData.value.radiation}`)
    if (formData.value.rainbow) result.push(`  rainbow: ${formData.value.rainbow}`)
    if (formData.value.antiWither) result.push(`  anti_wither_explosion: ${formData.value.antiWither}`)
    if (formData.value.soulbound) result.push(`  soulbound: ${formData.value.soulbound}`)
    if (formData.value.piglinTradeChance !== null && formData.value.piglinTradeChance !== '') {
      result.push(`  piglin_trade_chance: ${formData.value.piglinTradeChance}`)
    }
    if (formData.value.vanilla) result.push(`  vanilla_item: ${formData.value.vanilla}`)
    if (formData.value.hidden) result.push(`  hidden: ${formData.value.hidden}`)
    if (formData.value.dropFrom) result.push(`  drop_from: ${formData.value.dropFrom}`)
    if (formData.value.dropChance !== null && formData.value.dropChance !== '') {
      result.push(`  drop_chance: ${formData.value.dropChance}%`)
    }
    if (formData.value.dropAmount) result.push(`  drop_amount: ${formData.value.dropAmount}`)
  
    yamlOutput.value = result.join('\n')
  }
  </script>
  
  <style scoped>
  /* 你可以继续在这里添加 style，或者从你的原始 HTML 中复制过来 */
  .tabs {
    overflow: hidden;
    background-color: #ffffff;
    border-bottom: 1px solid #ddd;
    margin-bottom: 20px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.05);
  }
  
  .tabs button {
    background-color: inherit;
    float: left;
    border: none;
    outline: none;
    cursor: pointer;
    padding: 14px 16px;
    transition: all 0.3s ease;
    font-size: 16px;
    color: #555;
  }
  
  .tabs button:hover {
    background-color: #f1f1f1;
  }
  
  .tabs button.active {
    background-color: transparent;
    color: #007bff;
    border-bottom: 2px solid #007bff;
    font-weight: bold;
  }
  
  .content {
    display: none;
    padding: 20px;
    background-color: #ffffff;
    border-radius: 6px;
    box-shadow: 0 2px 6px rgba(0, 0, 0, 0.05);
  }
  
  .content.active {
    display: block;
  }
  
  .form-group {
    margin-bottom: 1.2em;
  }
  
  label {
    display: inline-block;
    width: 180px;
    vertical-align: top;
    font-weight: 500;
  }
  
  input[type="text"],
  input[type="number"],
  select {
    width: 300px;
    padding: 8px 10px;
    border: 1px solid #ccc;
    border-radius: 4px;
    font-size: 14px;
    margin-left: 10px;
    transition: border-color 0.3s ease;
  }
  
  input:focus,
  select:focus {
    border-color: #007bff;
    outline: none;
    box-shadow: 0 0 0 2px rgba(0, 123, 255, 0.25);
  }
  
  button {
    padding: 6px 12px;
    font-size: 14px;
    border: none;
    border-radius: 4px;
    background-color: #007bff;
    color: white;
    cursor: pointer;
    transition: background-color 0.3s ease;
    margin: 5px 0;
  }
  
  button:hover {
    background-color: #0056b3;
  }
  
  .recipe-item,
  .lore-item {
    margin-top: 6px;
  }
  
  .lore-input-container,
  .recipe-item {
    display: flex;
    align-items: center;
    gap: 10px;
  }
  
  .lore-input-container input,
  .recipe-item input,
  .recipe-item select {
    margin: 0 !important;
    width: auto;
    flex-grow: 1;
  }
  
  .lore-input-container button,
  .recipe-item button {
    margin-left: 10px;
    background-color: #dc3545;
  }
  
  .lore-input-container button:hover,
  .recipe-item button:hover {
    background-color: #c82333;
  }
  </style>