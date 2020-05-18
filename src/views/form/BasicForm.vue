<template>
  <div class="ant-card" style="padding:20px">
    <div class="table-page-search-wrapper">
      <a-form layout="inline">
        <a-row :gutter="48">
          <a-col :md="8" :sm="24">
            <a-form-item label="规则编号">
              <a-input v-model="queryParam.id" placeholder="编码/名称" />
            </a-form-item>
          </a-col>
          <a-col :md="!advanced && 8 || 24" :offset="8" :sm="24">
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
      ref="table"
      size="default"
      rowKey="key"
      :columns="columns"
      :data="loadData"
      :rowSelection="options.rowSelection"
    >
      <span slot="serial" slot-scope="text, record, index">{{ index + 1 }}</span>
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
      columns: [
        {
          title: '序号',
          width: '80px',
          scopedSlots: { customRender: 'serial' }
        },
        {
          width: '100px',
          title: '组织编码',
          dataIndex: 'no'
        },
        {
          width: '180px',
          title: '组织名称'
          // dataIndex: 'description'
        },
        {
          width: '180px',
          title: '上级组织',
          // dataIndex: 'callNo',
          needTotal: true
          // customRender: (text) => text + ' 次'
        },
        {
          width: '100px',
          title: '状态'
          // dataIndex: 'status',
        },
        {
          width: '250px',
          title: '备注'
          // dataIndex: 'updatedAt',
        },
        {
          title: '操作',
          // dataIndex: 'action',
          scopedSlots: { customRender: 'action' }
        }
      ],
      // 加载数据方法 必须为 Promise 对象
      loadData: parameter => {
        console.log('loadData.parameter', parameter)
        return getServiceList(Object.assign(parameter, this.queryParam)).then(res => {
          console.log()
          console.log(JSON.stringify(res.result))
          return {
            pageSize: 10,
            pageNo: 1,
            totalCount: 100,
            totalPage: 10,
            data: [
              {
                key: 1,
                id: 1,
                no: 'No 1',
                description: '这是一段描述',
                callNo: 301,
                status: 3,
                updatedAt: '2019-09-25 10:54:24',
                editable: false
              }
            ]
          }
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
