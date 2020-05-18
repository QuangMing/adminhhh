<template>
  <div>
    <page-view :title="title">
      <a-card :bordered="false">
        <div class="ant-card" style="padding:20px">
          <div class="table-page-search-wrapper">
            <a-form layout="inline">
              <a-row :gutter="48">
                <a-col :md="8" :sm="24">
                  <a-form-item label="关键字">
                    <a-input v-model="queryParam.id" placeholder="名称/编码" />
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
                    <a-button
                      icon="highlight"
                      style="margin-left: 8px"
                      @click="() => queryParam = {}"
                    >清除</a-button>
                  </span>
                  
                </a-col>
                <a-button type="primary" icon="search">添加项目</a-button>

              </a-row>
            </a-form>
          </div>
        </div>
        <s-table
          bordered
          tableLayout="fixed"
          ref="table"
          size="default"
          rowKey="key"
          :pagination="{ pageSize: 5 }"
          :columns="scheduleColumns"
          :data="loadScheduleData"
          :row-selection="rowSelection"
          >
          <span slot="serial" slot-scope="text, record, index">{{ index + 1 }}</span>
          <span slot="description" slot-scope="text, record, description">
             <a-icon type="arrow-up" style="color:red" />
            
            </span>
          <span slot="action" slot-scope="text, record">
            <template>
              <a @click="handleEdit(record)">
                <a-icon type="highlight" />
              </a>
              <a-divider type="vertical" />
              <a @click="handleEdit(record)">
                <a-icon type="delete" style="color:red" />
              </a>
              <a-divider type="vertical" />
<a-switch default-checked @change="onChange" />

              <!-- <a-button type="primary" size="small" @click="Scoring(true)">评分单据</a-button> -->
            </template>
          </span>
        </s-table>

      </a-card>
    </page-view>
    <div id="components-modal-demo-position">
    <a-modal
      v-model="modal2Visible"
      width="1000px"
      centered
    
    >
        <a-card :bordered="false">
        <div class="ant-card" style="padding:20px">
          <div class="table-page-search-wrapper">
            <a-form layout="inline">
              <a-row :gutter="48">
                <a-col :md="8" :sm="24">
                  <a-form-item label="关键字">
                    <a-input v-model="queryParam.id" placeholder="名称/编码" />
                  </a-form-item>
                </a-col>
                <a-col :md="!advanced && 8 || 24" :offset="4" :sm="24">
                  <span
                    class="table-page-search-submitButtons"
                    :style="advanced && { float: 'right', overflow: 'hidden' } || {} "
                  >
                    <!-- <a-button type="primary"  @click="$refs.table.refresh(true)">查询</a-button> -->
                    <a-button type="primary" icon="search">查询</a-button>
                    <a-button
                      icon="highlight"
                      style="margin-left: 8px"
                      @click="() => queryParam = {}"
                    >清除</a-button>
                  </span>
                </a-col>
              </a-row>
            </a-form>
          </div>
        </div>
        <s-table
          bordered
          tableLayout="fixed"
          ref="table"
          size="default"
          rowKey="key"
          :columns="alertTitle"
          :data="loadScheduleData"
        >
         
          <span
            slot="serial"
            slot-scope="text, record, index"
          >{{ index + 1 }}</span>
          <a slot="name" slot-scope="text">{{ text }}</a>
          <span slot="action" slot-scope="text, record">
            <template>
              <a @click="handleEdit(record)">
                <a-icon type="highlight" />
              </a>
              <a-divider type="vertical" />
              <a @click="handleEdit(record)">
                <a-icon type="delete" style="color:red" />
              </a>
              <a-divider type="vertical" />
              <!-- <a-button type="primary" size="small" @click="Scoring(true)">评分单据</a-button> -->
            </template>
          </span>
        </s-table>
      </a-card>
    </a-modal>
  </div>
  </div>
</template>

<script>
import { PageView } from '@/layouts'
import { STable } from '@/components'
import DetailList from '@/components/tools/DetailList'
const DetailListItem = DetailList.Item
const rowSelection = {
  onChange: (selectedRowKeys, selectedRows) => {
    console.log(`selectedRowKeys: ${selectedRowKeys}`, 'selectedRows: ', selectedRows);
  },
  onSelect: (record, selected, selectedRows) => {
    console.log(record, selected, selectedRows);
  },
  onSelectAll: (selected, selectedRows, changeRows) => {
    console.log(selected, selectedRows, changeRows);
  },
};

export default {
  components: {
    PageView,
    DetailList,
    DetailListItem,
    STable
  },
  data() {
    return {
       modal1Visible: false,
       rowSelection,
      modal2Visible: false,
      currentPage:2,
      mdl: {},
      // 高级搜索 展开/关闭
      advanced: false,
      // 查询参数
      queryParam: {},
      scheduleColumns: [
        {
          title: '项目编码',
          scopedSlots: { customRender: 'serial' }
        },
        {
          title: '项目名称',
          dataIndex: 'no'
        },
        {
          title: '移动',
          dataIndex: 'description',
           scopedSlots: { customRender: 'description' }
        },
        {
          title: '创建时间',
          dataIndex: 'callNo',
          // //sorter: true,
          needTotal: true
          // customRender: text => text + ' 次'
        },
        {
          title: '状态',
          dataIndex: 'status',
         
          // needTotal: true
        },
        {
          title: '备注',
         dataIndex: 'updatedAt'
          //sorter: true
        },
      
        {
          title: '操作',
          dataIndex: 'action',
          width: '180px',
          scopedSlots: { customRender: 'action' }
        }
      ],
      alertTitle: [
        {
          title: '序号',
          scopedSlots: { customRender: 'serial' }
        },
        {
          title: '单据编码',
          dataIndex: 'no'
        },
        {
          title: '评分项',
          dataIndex: 'description'
        },
        {
          title: '评分日期',
          dataIndex: 'callNo',
          // //sorter: true,
          needTotal: true
          // customRender: text => text + ' 次'
        },
        {
          title: '评分类型'
          // dataIndex: 'status',
          // needTotal: true
        },
        {
          title: '评分人',
         // dataIndex: 'updatedAt'
          //sorter: true
        },
      ],
      loadScheduleData: () => {
        return new Promise(resolve => {
          resolve({
            data: [
              {
                key: '1',
                time: '2017-10-01 14:10',
                rate: '联系客户',
                status: '正常',
                operator: '取货员 ID1234',
                cost: '5mins'
              },
              {
                key: '2',
                time: '2017-10-01 14:05',
                rate: '取货员出发',
                status: '正常',
                operator: '取货员 ID1234',
                cost: '1h'
              },
              {
                key: '3',
                time: '2017-10-01 13:05',
                rate: '取货员接单',
                status: '正常',
                operator: '取货员 ID1234',
                cost: '5mins'
              },
              {
                key: '4',
                time: '2017-10-01 13:00',
                rate: '申请审批通过',
                status: '正常',
                operator: '系统',
                cost: '1h'
              },
              {
                key: '5',
                time: '2017-10-01 12:00',
                rate: '发起退货申请',
                status: '正常 ',
                operator: '用户',
                cost: '5mins'
              }
            ],
            pageSize: 10,
            pageNo: 1,
            totalPage: 1,
            totalCount: 10
          })
        }).then(res => {
          return res
        })
      },
       loadScheduleData1: () => {
        return new Promise(resolve => {
          resolve({
            data: [
              {
                key: '1',
                num: 'GCKH202004230001-01-01',
                value: '02地块一期地下工程及基础',
                time: '评分日期',
              ren: '张三',
              }
            ],
            pageSize: 10,
            pageNo: 1,
            totalPage: 1,
            totalCount: 10
          })
        }).then(res => {
          return res
        })
      }
    }
  },
  methods: {
    onChange(checked) {
      console.log(`a-switch to ${checked}`);
    },
    Scoring(modal1Visible) {
      this.modal2Visible = modal1Visible;
    },
     setModal1Visible(modal1Visible) {
      this.modal1Visible = modal1Visible;
    },
  },
  filters: {
    statusFilter(status) {
      const statusMap = {
        processing: '进行中',
        success: '完成',
        failed: '失败'
      }
      return statusMap[status]
    }
  },
  computed: {
    title() {
      return this.$route.meta.title
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
</style>
