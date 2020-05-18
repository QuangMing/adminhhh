<template>
  <div>
    <page-view>
      <a-card :bordered="false">
        基本信息
        <!-- <a-input-search placeholder="input search text" @search="onSearch" /> -->

        <hr />
        <a-form layout="inline" v-bind="formItemLayout">
          <a-form-item label="单位编码">
            <a-input value="465165198498" disabled />
          </a-form-item>
          <a-form-item label="单位日期">
            <a-input value="节点控制标准模板" disabled />
          </a-form-item>

          <a-form-item label="单据状态">
            <a-select default-value="yes">
              <a-select-option value="yes">新增</a-select-option>
              <a-select-option value="no">否</a-select-option>
            </a-select>
          </a-form-item>
          <a-form-item label="一级项目">
            <a-input-search value="半岛明珠二号地块一期" enter-button @search="onSearch" />

            <!-- <a-input-search enter-button value="半岛明珠二号地块一期" disabled /> -->
          </a-form-item>
          <br />
          <br />

          <a-form-item label="二级项目">
            <a-input-search value="6/7/8/9工程主体" enter-button @search="onSearch" />

            <!-- <a-input value="6/7/8/9工程主体" disabled /> -->
          </a-form-item>
          <a-form-item label="截止日期">
            <a-input-search value="张三" enter-button @search="onSearch" />
            <!-- <a-input value="张三" disabled /> -->
          </a-form-item>
          <a-form-item width="100px" label="公司">
            <a-input-search value="A公司" enter-button @search="onSearch" />

            <!-- <a-input value="A公司" disabled /> -->
          </a-form-item>
          <a-form-item width="100px" label="部门">
            <a-input-search value="B部门" enter-button @search="onSearch" />

            <!-- <a-input value="B部门" disabled /> -->
          </a-form-item>
          <br />
          <br />

          <div>
            <p>摘要：</p>
            <a-textarea rows="6" style="width:100%" placeholder="You are not alone." />
          </div>
        </a-form>
        <a-row class="mb—box">
          <br />
          <a-col :span="8">综合工期</a-col>
          <br />
          <hr />
          <p>
            考核项名称：
            <a-input value="100" style="width:300px" disabled />
          </p>
          <br />
          <!-- <a-button type="dashed" style="margin-right:3px">综合考核项</a-button>
          <a-button type="dashed" style="margin-right:60px">综合考核项</a-button>
          <a-button type="primary" style="margin-right:60px">新增考核项</a-button>-->
          <a-tabs v-model="activeKey" type="editable-card" @edit="onEdit">
            <a-tab-pane
              v-for="pane in panes"
              :key="pane.key"
              :tab="pane.title"
              :closable="pane.closable"
            >{{ pane.content }}</a-tab-pane>
          </a-tabs>
          <!-- <br />
          <br />
          <br />-->
          <a-form layout="inline" v-bind="formItemLayout">
            <a-form-item label="考核项名称">
              <a-input value="02地块一期地下工程及基础" />
            </a-form-item>
            <a-form-item label="考核分值">
              <a-input value="300" />
            </a-form-item>

            <a-form-item label="考核人">
              <a-input-search value="张三（单选）" enter-button @search="onSearch" />

              <!-- <a-input value="张三（单选）" disabled /> -->
            </a-form-item>
          </a-form>
          <br />
          <a-table bordered :pagination="false" :columns="columns" :data-source="data">
            <span slot="cloumnSetButton" slot-scope="text, record, cloumnSetButton">
              <a-icon type="plus" />
              <a-icon type="minus" />
            </span>
          </a-table>
          <br />工程考核
          <hr />
          <br />
          <!-- <a-button type="dashed" style="margin-right:3px">综合考核项</a-button>
          <a-button type="dashed" style="margin-right:60px">综合考核项</a-button>-->
          <a-tabs v-model="activeKey" type="editable-card" @edit="onEdit">
            <a-tab-pane
              v-for="pane in panes"
              :key="pane.key"
              :tab="pane.title"
              :closable="pane.closable"
            >{{ pane.content }}</a-tab-pane>
          </a-tabs>
          <!-- <br />
          <br />-->
          <a-form layout="inline" v-bind="formItemLayout">
            <a-form-item label="考核项名称">
              <a-input value="02地块一期地下工程及基础" disabled />
            </a-form-item>
            <a-form-item label="考核分值">
              <a-input value="1700" disabled />
            </a-form-item>
            <br />
            <a-form-item label="考核人">
              <a-table bordered :pagination="false" :columns="columns1" :data-source="data1">
                <template slot="name" slot-scope="text">
                  <a>{{ text }}</a>
                </template>
                <span slot="cloumnSetButton" slot-scope="text, record, cloumnSetButton">
                  <a-icon type="plus" />
                  <a-icon type="minus" />
                </span>
                <!-- <template slot="title" slot-scope="currentPageData">Header</template>
                <template slot="footer" slot-scope="currentPageData">Footer</template>-->
              </a-table>
            </a-form-item>
          </a-form>
          <br />
          <br />
          <a-table bordered :pagination="false" :columns="columns2" :data-source="data">
            <span slot="cloumnSetButton" slot-scope="text, record, cloumnSetButton">
              <a-icon type="plus" />
              <a-icon type="minus" />
            </span>
          </a-table>

          <br />节点工期与工程考核综合得分系数
          <hr />
          <br />
          <!-- <a-button type="dashed" style="margin-right:3px">综合考核项</a-button>
          <a-button type="dashed" style="margin-right:60px">综合考核项</a-button>-->
          <a-tabs v-model="activeKey" type="editable-card" @edit="onEdit">
            <a-tab-pane
              v-for="pane in panes"
              :key="pane.key"
              :tab="pane.title"
              :closable="pane.closable"
            >{{ pane.content }}</a-tab-pane>
          </a-tabs>
          <p>
            考核项名称：
            <a-input value="02地块一期地下工程及基础" style="width:300px" disabled />
          </p>
          <a-table bordered :pagination="false" :columns="columns3" :data-source="data">
            <span slot="cloumnSetButton" slot-scope="text, record, cloumnSetButton">
              <a-icon type="plus" />
              <a-icon type="minus" />
            </span>
          </a-table>
        </a-row>
      </a-card>
    </page-view>
  </div>
</template>

<script>
import { PageView } from '@/layouts'
import { STable } from '@/components'
import DetailList from '@/components/tools/DetailList'
let base = {
  children: [
    {
      title: '指标'
    },
    {
      title: '权重'
    },
    {
      title: '分值'
    }
  ]
}
let base1 = {
  children: [
    {
      title: '名称'
    },
    {
      title: '分值'
    }
  ]
}
let base2 = {
  children: [
    {
      title: '名称'
    },
    {
      title: '分值'
    },
    {
      title: '占比'
    }
  ]
}
let base3 = {
  children: [
    {
      title: '分值'
    },
    {
      title: '得分'
    }
  ]
}
let base4 = {
  children: [
    {
      title: '分值'
    },
    {
      title: '得分'
    },
    {
      title: '综合分'
    }
  ]
}

const columns = [
  {
    title: '行号',
    dataIndex: 'key'
  },
  {
    title: '',
    dataIndex: 'name',
    scopedSlots: { customRender: 'cloumnSetButton' }
  },
  {
    title: '考核分项',
    dataIndex: 'age'
  },
  {
    title: '分值占比'
  },
  {
    title: '分值'
  },
  {
    title: '级差  '
  },
  {
    title: '提前2',
    ...base
  },
  {
    title: '提前1',
    ...base
  },
  {
    title: '标准',
    ...base
  },
  {
    title: '带后1',
    ...base
  },
  {
    title: '带后2',
    ...base
  },
  {
    title: '带后3',
    ...base
  },
  {
    title: '考核打分'
  },
  {
    title: '备注'
  }
]
const columns1 = [
  {
    title: '行号',
    width: '100px',
    dataIndex: 'name',
    scopedSlots: { customRender: 'name' }
  },
  {
    title: '',
    scopedSlots: { customRender: 'cloumnSetButton' },
    width: '80px'
  },
  {
    title: '姓名',
    width: '100px',
    dataIndex: 'address'
  },
  {
    title: '权重(%)',
    width: '100px',
    dataIndex: 'money'
  }
]
const columns2 = [
  {
    title: '行号',
    dataIndex: 'key'
  },
  {
    title: '',
    dataIndex: 'name',
    scopedSlots: { customRender: 'cloumnSetButton' }
  },
  {
    title: '工程范围',
    ...base1
  },
  {
    title: '分项工程',
    ...base2
  },
  {
    title: '质量管理',
    ...base2
  },
  {
    title: '材料管理',
    ...base2
  },
  {
    title: '计量',
    ...base2
  },
  {
    title: '工程策划',
    ...base2
  },
  {
    title: '工程签证',
    ...base2
  },
  {
    title: '成本控制',
    ...base2
  },

  {
    title: '小计',
    ...base3
  }
]
const columns3 = [
  {
    title: '行号',
    dataIndex: 'key'
  },
  {
    title: '',
    dataIndex: 'name',
    scopedSlots: { customRender: 'cloumnSetButton' }
  },
  {
    title: '工程分区间从'
    // ...base1
  },
  {
    title: '工程分区间到'
    // ...base2
  },
  {
    title: '权重（%）'
    // ...base2
  },
  {
    title: '综合工期评分'
    // ...base2
  },
  {
    title: '工程考核综合得分'
    // ...base2
  },
  {
    title: '得分'
    // ...base2
  },
  {
    title: '备注'
    // ...base2
  }
]
const data1 = [
  {
    key: '1',
    name: '1',
    money: '40',
    address: '张三'
  },
  {
    key: '2',
    name: '2',
    money: '30',
    address: '李四'
  },
  {
    key: '3',
    name: '3',
    money: '30',
    address: '王五'
  }
]

const data = [
  {
    key: '1',
    age: 32,
    address: 'New York No. 1 Lake Park'
  }
]

function onChange(pagination, filters, sorter) {
  console.log('params', pagination, filters, sorter)
}
const panes = [
  { title: '综合考核项 1', content: '', key: '1' },
  { title: '综合考核项 2', content: '', key: '2' },
  { title: '综合考核项 3', content: '', key: '3' }
]
export default {
  data() {
    return {
      activeKey: panes[0].key,
      panes,
      newTabIndex: 0,
      data,
      columns,
      data1,
      columns1,
      columns2,
      columns3,
      formItemLayout: {
        // labelCol: { span: 9 }
        // wrapperCol: { span: 14 }
      }
    }
  },
  methods: {
    onChange,
    callback(key) {
      console.log(key)
    },
    onEdit(targetKey, action) {
      this[action](targetKey)
    },
    add() {
      const panes = this.panes
      const activeKey = `newTab${this.newTabIndex++}`
      panes.push({
        title: `New Tab ${activeKey}`,
        content: ``,
        key: activeKey
      })
      this.panes = panes
      this.activeKey = activeKey
    },
    remove(targetKey) {
      let activeKey = this.activeKey
      let lastIndex
      this.panes.forEach((pane, i) => {
        if (pane.key === targetKey) {
          lastIndex = i - 1
        }
      })
      const panes = this.panes.filter(pane => pane.key !== targetKey)
      if (panes.length && activeKey === targetKey) {
        if (lastIndex >= 0) {
          activeKey = panes[lastIndex].key
        } else {
          activeKey = panes[0].key
        }
      }
      this.panes = panes
      this.activeKey = activeKey
    }
  }
}
</script>

 
<style lang="less" scoped>
.title {
  color: rgba(0, 0, 0, 0.85);
  font-size: 16px;
  font-weight: 500;
  margin-bottom: 16px;
}
.ant-pagination.ant-table-pagination {
  display: none !important;
}
</style>
