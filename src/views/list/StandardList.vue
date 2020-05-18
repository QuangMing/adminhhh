<template>
  <div class="ant-card" style="padding:20px">
    <div class="table-page-search-wrapper">
      <a-form layout="inline">
        <a-row :gutter="48">
          <a-col :md="8" :sm="24">
            <a-form-item label="关键字">
              <a-input v-model="queryParam.id" placeholder />
            </a-form-item>
          </a-col>
          <a-col :md="4" :sm="24">
            <a-form-item label="状态">
              <a-input-group compact>
                <a-select default-value="Option1-1">
                  <a-select-option value="Option1-1">正常</a-select-option>
                  <a-select-option value="Option1-2">停用</a-select-option>
                </a-select>
              </a-input-group>
            </a-form-item>
          </a-col>
          <a-col :md="!advanced && 8 || 24" :offset="4" :sm="24">
            <span
              class="table-page-search-submitButtons"
              :style="advanced && { float: 'right', overflow: 'hidden' } || {} "
            >
              <!-- <a-button type="primary"  @click="$refs.table.refresh(true)">查询</a-button> -->
              <a-button type="primary" icon="search">查询</a-button>
              <a-button icon="highlight" style="margin-left: 8px" @click="() => queryParam = {}">清除</a-button>
            </span>
          </a-col>
        </a-row>
      </a-form>
    </div>

    <s-table
      bordered
      tableLayout="fixed"
      ref="table"
      size="default"
      rowKey="key"
      :columns="columns"
      :data="loadData"
      :rowSelection="options.rowSelection"
    >
      <span slot="serial" slot-scope="text, record, index">{{ index + 1 }}</span>
      <span slot="action" slot-scope="text, record">
        <template>
          <a @click="handleEdit(record)">
            <a-icon type="highlight" />
          </a>
          <a-divider type="vertical" />
          <a @click="handleEdit(record)">
            <a-icon type="delete" style="color:red" />
          </a>
        </template>
      </span>
    </s-table>
  </div>
</template>

<script>
import moment from 'moment'
import { STable } from '@/components'
import { getRoleList, getServiceList } from '@/api/manage'

export default {
  name: 'TableList',
  components: {
    STable
  },
  data() {
    return {
      mdl: {},
      // 高级搜索 展开/关闭
      advanced: false,
      // 查询参数
      queryParam: {},
      // 表头
      columns:
       [
        {
          title: '序号',
          scopedSlots: { customRender: 'serial' }
        },
        {
          title: '单据编码',
          dataIndex: 'no'
        },
        {
          title: '单据日期',
          dataIndex: 'description'
        },
        {
          title: '单据状态',
          dataIndex: 'callNo',
          // //sorter: true,
          needTotal: true,
          customRender: text => text + ' 次'
        },
        {
          title: '公司',
          dataIndex: 'status',
          needTotal: true
        },
        {
          title: '一级项目',
          dataIndex: 'updatedAt',
          //sorter: true
        },
        {
          title: '二级项目',
          dataIndex: 'updatedAt',
          //sorter: true
        },
        {
          title: '分部工程',
          dataIndex: 'updatedAt',
          //sorter: true
        },
        {
          title: '工程类别',
          dataIndex: 'updatedAt',
          //sorter: true
        },
        {
          title: '核定工期',
          dataIndex: 'updatedAt',
          //sorter: true
        },
        {
          title: '实际工期',
          dataIndex: 'updatedAt',
          //sorter: true
        },
        {
          title: '核定时间',
          dataIndex: 'updatedAt',
          //sorter: true
        },
        {
          title: '设计时间',
          dataIndex: 'updatedAt',
          //sorter: true
        },
        {
          title: '备注',
          dataIndex: 'updatedAt',
          //sorter: true
        },
        {
          title: '操作',
          dataIndex: 'action',
          width: '100px',
          scopedSlots: { customRender: 'action' }
        }
      ],
      // 加载数据方法 必须为 Promise 对象
      loadData: parameter => {
        console.log('loadData.parameter', parameter)
        return getServiceList(Object.assign(parameter, this.queryParam)).then(res => {
          return res.result
        })
      },
      selectedRowKeys: [],
      selectedRows: [],

      // custom table alert & rowSelection
      options: {
        alert: {
          show: true,
          clear: () => {
            this.selectedRowKeys = []
          }
        },
        rowSelection: {
          selectedRowKeys: this.selectedRowKeys,
          onChange: this.onSelectChange
        }
      },
      optionAlertShow: false
    }
  },
  created() {
    this.tableOption()
    getRoleList({ t: new Date() })
  },
  methods: {
    tableOption() {
      if (!this.optionAlertShow) {
        this.options = {
          alert: {
            show: true,
            clear: () => {
              this.selectedRowKeys = []
            }
          },
          rowSelection: {
            selectedRowKeys: this.selectedRowKeys,
            onChange: this.onSelectChange
          }
        }
        this.optionAlertShow = true
      } else {
        this.options = {
          alert: false,
          rowSelection: null
        }
        this.optionAlertShow = false
      }
    },

    handleEdit(record) {
      this.$emit('onEdit', record)
    },
    handleOk() {},

    onSelectChange(selectedRowKeys, selectedRows) {
      this.selectedRowKeys = selectedRowKeys
      this.selectedRows = selectedRows
    },
    toggleAdvanced() {
      this.advanced = !this.advanced
    },

    resetSearchForm() {
      this.queryParam = {
        date: moment(new Date())
      }
    }
  }
}
</script>
