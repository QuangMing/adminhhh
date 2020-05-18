<template>
  <div>
    <page-view>
      <a-card :bordered="false">
        基本信息
        <hr />
        <a-form layout="inline" v-bind="formItemLayout">
          <a-form-item label="单位编码">
            <a-input value="465165198498" disabled />
          </a-form-item>
          <a-form-item label="单位日期">
            <a-input value="节点控制标准模板" disabled />
          </a-form-item>
          <a-form-item label="一级项目">
            <a-input value="张三" disabled />
          </a-form-item>
          <a-form-item label="单据状态">
            <a-select default-value="yes">
              <a-select-option value="yes">新增</a-select-option>
              <a-select-option value="no">否</a-select-option>
            </a-select>
          </a-form-item>
          <br />
          <br />
          <a-form-item label="二级项目">
            <a-input value="张三" disabled />
          </a-form-item>
          <a-form-item width="100px" label="评分人">
            <a-input value="张三" disabled />
          </a-form-item>
          <a-form-item label="截止日期">
            <a-input value="张三" disabled />
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
            <a-input value="02地块一期地下工程及基础" style="width:300px" disabled />
          </p>
          <a-table bordered :columns="columns" :data-source="data" />
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
const columns = [
  {
    title: '行号',
    dataIndex: 'key'
  },
  {
    title: '考核分项',
    dataIndex: 'name'
  },
  {
    title: '分值占比（%）',
    dataIndex: 'age'
  },
  {
    title: '分值'
  },
  {
    title: '级差'
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
  }
]

const data = [
  {
    key: '1',
    age: 32,
    address: 'New York No. 1 Lake Park'
  },
  {
    key: '2',
    age: 42,
    address: 'London No. 1 Lake Park'
  },
  {
    key: '3',
    age: 32,
    address: 'Sidney No. 1 Lake Park'
  },
  {
    key: '4',
    age: 32,
    address: 'London No. 2 Lake Park'
  }
]

function onChange(pagination, filters, sorter) {
  console.log('params', pagination, filters, sorter)
}

export default {
  data() {
    return {
      data,
      columns,
      formItemLayout: {
        // labelCol: { span: 6 },
        // wrapperCol: { span: 14 }
      }
    }
  },
  methods: {
    onChange
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
</style>
