<template>
  <div class="ant-card" style="padding:20px">
    <div class="table-page-search-wrapper">
      <a-form layout="inline">
        <a-row :gutter="48">
          <a-col :md="8" :sm="24">
            <a-form-item label="规则编号">
              <a-input v-model="queryParam.id" placeholder=""/>
            </a-form-item>
          </a-col>
          <a-col :md="!advanced && 8 || 24" :offset="8" :sm="24">
            <span class="table-page-search-submitButtons" :style="advanced && { float: 'right', overflow: 'hidden' } || {} ">
              <!-- <a-button type="primary"  @click="$refs.table.refresh(true)">查询</a-button> -->
              <a-button type="primary" icon="search">查询</a-button>
              <a-button icon="highlight" style="margin-left: 8px" @click="() => queryParam = {}">清除</a-button>
            </span>
          </a-col>
        </a-row>
      </a-form>
    </div>
  <a-row :gutter="48">
   <a-col :md="4" :sm="24">
   <div style="width:100%;border:1px solid black;">
   <a-tree
    class="draggable-tree"
    :default-expanded-keys="expandedKeys"
    draggable
    :tree-data="gData"
    @dragenter="onDragEnter"
    @drop="onDrop"
  />
   </div>
   </a-col>
    <a-col :md="20" :sm="24">
      <s-table
      bordered
      ref="table"
      size="default"
      rowKey="key"
      :columns="columns"
      :data="loadData"
      :rowSelection="options.rowSelection"
    >
      <span slot="serial" slot-scope="text, record, index">
        {{ index + 1 }}
      </span>
    </s-table>
    </a-col>
  </a-row>
  
  </div>
</template>

<script>
const x = 3;
const y = 2;
const z = 1;
const gData = [];

const generateData = (_level, _preKey, _tns) => {
  const preKey = _preKey || '0';
  const tns = _tns || gData;

  const children = [];
  for (let i = 0; i < x; i++) {
    const key = `${preKey}-${i}`;
    tns.push({ title: key, key });
    if (i < y) {
      children.push(key);
    }
  }
  if (_level < 0) {
    return tns;
  }
  const level = _level - 1;
  children.forEach((key, index) => {
    tns[index].children = [];
    return generateData(level, key, tns[index].children);
  });
};
generateData(z);
import moment from 'moment'
import { STable } from '@/components'
import { getRoleList, getServiceList } from '@/api/manage'

export default {
  name: 'TableList',
  components: {
    STable
  },
  data () {
    return {
      gData,
      expandedKeys: ['0-0', '0-0-0', '0-0-0-0'],
      mdl: {},
      // 高级搜索 展开/关闭
      advanced: false,
      // 查询参数
      queryParam: {},
      // 表头
      columns: [
        {
          title: '序号',
          scopedSlots: { customRender: 'serial' }
        },
        {
          title: '工号',
        },
        {
          title: '姓名',
        },
        {
          title: '手机号码',
          needTotal: true,
        },
        {
          title: '职位',
          needTotal: true
        },
        {
          title: '部门',
          
        },
        {
          title: '入职时间',
          
        },
        {
          title: '角色',
          
        },
        {
          title: '状态',
          
        },
        {
          title: '操作',
          dataIndex: 'action',
          width: '120px',
          scopedSlots: { customRender: 'action' }
        }
      ],
      // 加载数据方法 必须为 Promise 对象
      loadData: parameter => {
        console.log('loadData.parameter', parameter)
        return getServiceList(Object.assign(parameter, this.queryParam))
          .then(res => {
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
              },
              {
                key: 1,
                id: 1,
                no: 'No 1',
                description: '这是一段描述',
                callNo: 301,
                status: 3,
                updatedAt: '2019-09-25 10:54:24',
                editable: false
              },
              {
                key: 1,
                id: 1,
                no: 'No 1',
                description: '这是一段描述',
                callNo: 301,
                status: 3,
                updatedAt: '2019-09-25 10:54:24',
                editable: false
              },
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
        alert: { show: true, clear: () => { this.selectedRowKeys = [] } },
        rowSelection: {
          selectedRowKeys: this.selectedRowKeys,
          onChange: this.onSelectChange
        }
      },
      optionAlertShow: false
    }
  },
  created () {
    this.tableOption()
    getRoleList({ t: new Date() })
  },
  methods: {
     onDragEnter(info) {
      console.log(info);
      // expandedKeys 需要受控时设置
      // this.expandedKeys = info.expandedKeys
    },
    onDrop(info) {
      console.log(info);
      const dropKey = info.node.eventKey;
      const dragKey = info.dragNode.eventKey;
      const dropPos = info.node.pos.split('-');
      const dropPosition = info.dropPosition - Number(dropPos[dropPos.length - 1]);
      const loop = (data, key, callback) => {
        data.forEach((item, index, arr) => {
          if (item.key === key) {
            return callback(item, index, arr);
          }
          if (item.children) {
            return loop(item.children, key, callback);
          }
        });
      };
      const data = [...this.gData];

      // Find dragObject
      let dragObj;
      loop(data, dragKey, (item, index, arr) => {
        arr.splice(index, 1);
        dragObj = item;
      });
      if (!info.dropToGap) {
        // Drop on the content
        loop(data, dropKey, item => {
          item.children = item.children || [];
          // where to insert 示例添加到尾部，可以是随意位置
          item.children.push(dragObj);
        });
      } else if (
        (info.node.children || []).length > 0 && // Has children
        info.node.expanded && // Is expanded
        dropPosition === 1 // On the bottom gap
      ) {
        loop(data, dropKey, item => {
          item.children = item.children || [];
          // where to insert 示例添加到尾部，可以是随意位置
          item.children.unshift(dragObj);
        });
      } else {
        let ar;
        let i;
        loop(data, dropKey, (item, index, arr) => {
          ar = arr;
          i = index;
        });
        if (dropPosition === -1) {
          ar.splice(i, 0, dragObj);
        } else {
          ar.splice(i + 1, 0, dragObj);
        }
      }
      this.gData = data;
    },
    tableOption () {
      if (!this.optionAlertShow) {
        this.options = {
          alert: { show: true, clear: () => { this.selectedRowKeys = [] } },
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

    handleEdit (record) {
      this.$emit('onEdit', record)
    },
    handleOk () {

    },

    onSelectChange (selectedRowKeys, selectedRows) {
      this.selectedRowKeys = selectedRowKeys
      this.selectedRows = selectedRows
    },
    toggleAdvanced () {
      this.advanced = !this.advanced
    },

    resetSearchForm () {
      this.queryParam = {
        date: moment(new Date())
      }
    }
  }
}
</script>
