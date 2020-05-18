<template>
  <div>
    <page-view>
      <a-card :bordered="false">
        基本信息
        <a-form layout="inline">
          <a-form-item label="模板编码">
            <a-input value="465165198498" disabled />
          </a-form-item>
          <a-form-item label="模板名称">
            <a-input value="节点控制标准模板" disabled />
          </a-form-item>
          <a-form-item label="创建人">
            <a-input value="张三" disabled />
          </a-form-item>
          <a-form-item label="是否预设">
            <a-select default-value="yes">
              <a-select-option value="yes">是</a-select-option>
              <a-select-option value="no">否</a-select-option>
            </a-select>
          </a-form-item>
        </a-form>
        <a-row class="mb—box">
          <a-col :span="8">节点控制明细</a-col>
          <br />
          <table class="table">
            <thead>
              <th></th>
              <th></th>
              <th v-for="item in 14" :key="item">{{'列'+item}}</th>
            </thead>
            <tr>
              <td rowspan="2">行号</td>
              <td></td>
              <td v-for="item in 14" :key="item">
                <a-icon type="plus" />
                <a-icon type="minus" />
              </td>
            </tr>
            <tr>
              <td v-for="(item,i) in tabData.heade" :key="i">{{item}}</td>
            </tr>
            <tbody>
              <template v-for="(num,k) in tabData.class.length">
                <tr v-for="(item,i) in tabData.class[k].data" :key="i+Math.random()">
                  <td></td>
                  <td>
                    <a-icon type="plus" />
                    <a-icon type="minus" />
                  </td>
                  <td :rowspan="tabData.class[k].data.length" v-if="i==0">{{tabData.class[k].title}}</td>
                  <td v-for="(item1,j) in item" :key="j">{{item1}}</td>
                </tr>
              </template>
            </tbody>
          </table>
        </a-row>
      </a-card>
    </page-view>
  </div>
</template>

<script>
import { PageView } from '@/layouts'
import { STable } from '@/components'
import DetailList from '@/components/tools/DetailList'
const DetailListItem = DetailList.Item

export default {
  components: {
    PageView,
    DetailList,
    DetailListItem,
    STable
  },
  data() {
    return {
      tabData: []
    }
  },
  created() {
    this.tabData = this.initData()
    console.log(this.tabData)
  },
  computed: {
    comNum() {
      let num = 0
      this.tabData.class.map(v => {
        v.data.map(item => {
          num += 1
        })
      })
      return num
    }
  },
  methods: {
    initData() {
      let baseData = {}
      for (let i = 0; i < 12; i++) {
        baseData['lie' + i] = ''
      }
      console.log(baseData)
      return {
        // 头部分类
        heade: [
          '',
          '分部工程',
          '工程类别',
          '核定工期',
          '实际工期',
          '核定时间',
          '实际工期',
          '备注',
          '',
          '',
          '',
          '',
          '',
          '',
          ''
        ],
        // 类
        class: [
          {
            title: '地基与基础',
            data: [
              {
                title: '桩基础',
                ...baseData
              },
              {
                title: '桩基检查',
                ...baseData
              },
              {
                title: '基坑支护',
                ...baseData
              },
              {
                title: '土方',
                ...baseData
              },
              {
                title: '综合时间',
                ...baseData
              }
            ]
          },
          {
            title: '地下结构',
            data: [
              {
                title: '底板',
                ...baseData
              },
              {
                title: '顶板',
                ...baseData
              }
            ]
          },
          {
            title: '32',
            data: [
              {
                title: '首三层',
                ...baseData
              }
            ]
          },
          {
            title: '主体结构',
            data: [
              {
                title: '预售层',
                ...baseData
              },
              {
                title: '关键节点偏差',
                ...baseData
              },
              {
                title: '结构封顶',
                ...baseData
              }
            ]
          },
          {
            title: '销售时间',
            data: [
              {
                title: '可销售面积',
                ...baseData
              }
            ]
          },
          {
            title: '装修及设备安装阶段',
            data: [
              {
                title: '外砌筑抹灰',
                ...baseData
              },
              {
                title: '铝合金门窗',
                ...baseData
              },
              {
                title: '外墙涂料',
                ...baseData
              },
              {
                title: '机电安装',
                ...baseData
              },
              {
                title: '内砌筑抹灰',
                ...baseData
              },
              {
                title: '公区装修',
                ...baseData
              }
            ]
          }
        ]
        // row1:['分部工程','地基与基础','地下结构',32,'主体结构','销售时间','装修以及设备安装阶段'],
        // row2:['工程类别','桩基础',]
      }
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
.mb—box {
  margin-top: 30px;
}
.table {
  border: 1px solid #e8e8e8;
  width: 100%;
  th {
    padding: 10px;
    color: rgba(0, 0, 0, 0.85);
    font-weight: 500;
    text-align: left;
    background: #fafafa;
    border-bottom: 1px solid #e8e8e8;
    border-right: 1px solid #e8e8e8;
    transition: background 0.3s ease;
  }
  td {
    padding: 5px 10px;
  }
  tr {
    border-bottom: 1px solid #e8e8e8;
  }
  td {
    border-right: 1px solid #e8e8e8;
  }
}
</style>
