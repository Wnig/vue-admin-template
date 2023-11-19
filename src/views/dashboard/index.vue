<template>
  <div class="dashboard-container">
    <div>
      <el-button type="parmary" :icon="isExpend ? 'el-icon-caret-bottom' : 'el-icon-caret-top'" @click="expendAll"></el-button>
      <el-button type="parmary" @click="changeTime">{{ timeType == 'week' ? '周' : '月' }}</el-button>
    </div>
    <el-table
      ref="treeTable"
      class="el-tree-table"
      :data="showTableData"
      border :cell-class-name="getCellName" :span-method="objectSpanMethod" @cell-click="cellClick">
      <el-table-column v-for="(item, index) in columns" :key="index" :prop="item.prop" :label="item.label" :width="item.width" :align="item.align" :fixed="item.fixed">
        <template slot-scope="scope">
          <div v-if="item.isTime">
              <div v-for="(ite, ind) in scope.row.targetList" :key="ind">
                <span v-if="ite.count == item.count">{{ ite.value }}</span>
              </div>
          </div>
          <div v-else-if="item.prop == 'targetName'">
            <span class="el-table__indent" :style="{ paddingLeft: (scope.row.level - 1) * 16 + 'px' }" v-if="scope.row.level > 1"></span><span class="el-table__placeholder" v-if="scope.row.level > 1 && (scope.row.children && !scope.row.children.length)"></span><div class="el-table__expand-icon" v-if="scope.row.children && scope.row.children.length"><i :class="scope.row.isExpend ? 'el-icon-arrow-down' : 'el-icon-arrow-right'"></i></div><span>{{ scope.row[item.prop] }}</span>
          </div>
          <div v-else>
            {{ scope.row[item.prop] }}
          </div>
        </template>
      </el-table-column>
    </el-table>

    <!-- <el-table
      row-key="targetId"
      :data="tableData2"
      border
      :tree-props="{children: 'children'}">
      <el-table-column v-for="(item, index) in columns2" :key="'key'+index" :prop="item.prop" :label="item.label" :width="item.width" :align="item.align">
        <template slot-scope="scope">
          <span>
            {{ scope.row[item.prop] }}
          </span>
        </template>
      </el-table-column>
    </el-table> -->
  </div>

</template>

<script>

export default {
  name: 'Dashboard',
  computed: {

  },
  watch: {
    tableData: {
      immediate: true,
      deep: true,
      handler(val) {
        console.log(val)
        this.showTable()
        this.$nextTick(() => {
          this.$refs.treeTable.doLayout()
        })
      }
    }
  },
  data() {
    return {
      isExpend: false,
      tableData: [],
      showTableData: [],
      columns: [],
      columns2: [{
        label: '目标指数标题',
        prop: 'targetName',
        width: '260px',
        align: 'left',
        fixed: 'left'
      }, {
        label: '区域',
        prop: 'area',
        align: 'center',
        width: '60px',
        fixed: 'left'
      }, {
        label: '城市',
        prop: 'city',
        align: 'center',
        width: '60px',
        fixed: 'left'
      }],
      tableData2: [{
        id: 1,
        city: '厦门',
        cityCode: '2333',
        area: '区域一',
        targetName: '1-财务目标',
        targetId: 1,
        parentId: 0,
        children: [{
          targetId: 11,
          parentId: 1,
          targetName: '2-车辆财务目标',
          targetList: [{
            count: 1,
            value: 222.3
          }, {
            count: 3,
            value: 2212.3
          }, {
            count: 7,
            value: 3212.3
          }, {
            count: 14,
            value: 320
          }],
          children: [{
            targetId: 111,
            parentId: 11,
            targetName: '3-站财务目标',
            children: [{
              targetId: 1111,
              parentId: 111,
              targetName: '4-电量财务目标',
              targetList: [{
                count: 4,
                value: 22
              }, {
                count: 6,
                value: 221
              }, {
                count: 9,
                value: 312.3
              }, {
                count: 16,
                value: 3210
              }],
              children: [{
                targetId: 11111,
                parentId: 1111,
                targetName: '5-电量财务目标',
                children: [{
                  targetId: 111111,
                  parentId: 11111,
                  targetName: '6-电量财务目标',
                  children: [{
                    targetId: 1111111,
                    parentId: 111111,
                    targetName: '7-电量财务目标',
                    children: [{
                      targetId: 11111111,
                      parentId: 1111111,
                      targetName: '8-dianliang电量财务目标',
                      children: []
                    }]
                  }]
                }]
              }]
            }, {
              targetId: 1112,
              parentId: 111,
              targetName: '4-电量财务目标2',
              targetList: [{
                count: 4,
                value: 22
              }, {
                count: 6,
                value: 221
              }, {
                count: 9,
                value: 312.3
              }, {
                count: 16,
                value: 322
              }],
              children: []
            }]
          }]
        }, {
          targetId: 12,
          parentId: 1,
          targetName: '2-站财务目标2',
          children: [{
            targetId: 112,
            parentId: 12,
            targetName: '3-车财务目标2',
            children: [{
              targetId: 1112,
              parentId: 112,
              targetName: '4-用电量财务目标2',
              children: []
            }]
          }]
        }, {
          targetId: 13,
          targetName: '2-经营财务目标3',
          children: []
        }, {
          targetId: 14,
          targetName: '2-车辆财务目标4',
          children: [{
            targetId: 114,
            parentId: 14,
            targetName: '3-建站财务目标4',
            children: []
          }]
        }]
      }, {
        id: 2,
        city: '厦门',
        cityCode: '2333',
        area: '区域一',
        targetName: '1-首站营业目标',
        targetId: 2,
        parentId: 0,
        children: [{
          targetId: 21,
          parentId: 2,
          targetName: '2-出车率营业目标',
          children: [{
            targetId: 211,
            parentId: 21,
            targetName: '3-用电率营业目标',
            children: []
          }]
        }, {
          targetId: 22,
          parentId: 2,
          targetName: '2-金额营业目标2',
          children: [{
            targetId: 221,
            parentId: 22,
            targetName: '3-现金营业目标2',
            children: []
          }]
        }]
      }, {
        id: 3,
        city: '深圳',
        area: '区域一',
        cityCode: '22345',
        targetName: '1-车辆用电指标',
        targetId: 2,
        parentId: 0,
        children: [{
          targetId: 31,
          parentId: 3,
          targetName: '2-站车辆指标',
          children: []
        }, {
          targetId: 32,
          parentId: 3,
          targetName: '2-城市车辆指标2',
          children: []
        }, {
          targetId: 33,
          parentId: 3,
          targetName: '2-剩余车辆指标3',
          children: []
        }, {
          targetId: 34,
          parentId: 3,
          targetName: '2-土地车辆指标4',
          children: []
        }]
      }, {
        id: 4,
        city: '泉州',
        area: '区域二',
        cityCode: '5322',
        targetName: '1-站电量指标',
        targetId: 4,
        parentId: 0,
        children: [{
          targetId: 41,
          parentId: 4,
          targetName: '2-车电量指标',
          children: []
        }, {
          targetId: 42,
          parentId: 4,
          targetName: '2-城市电量指标2',
          children: [{
            targetId: 442,
            parentId: 42,
            targetName: '3-耗损电量指标2',
            children: []
          }]
        }, {
          targetId: 43,
          parentId: 4,
          targetName: '2-解约电量指标3',
          children: []
        }]
      }],
      timeType: 'month',
      currentMonth: 6,
      currentWeek: 16
    }
  },
  created() {
    this.getList()
  },
  methods: {
    mergeComon(id, rowIndex) { // 合并单元格
      let idName = this.showTableData[rowIndex][id]
      if (rowIndex > 0) {
        if (this.showTableData[rowIndex][id] != this.showTableData[rowIndex - 1][id]) {
          let i = rowIndex;
          let num = 0
          while (i < this.showTableData.length) {
            if (this.showTableData[i][id] === idName) {
              i++
              num++
            } else {
              i = this.showTableData.length
            }
          }
          return {
            rowspan: num,
            colspan: 1
          }
        } else {
          return {
            rowspan: 0,
            colspan: 1
          }
        }
      } else {
        let i = rowIndex;
        let num = 0
        while (i < this.showTableData.length) {
          if (this.showTableData[i][id] === idName) {
            i++
            num++
          } else {
            i = this.showTableData.length
          }
        }
        return {
          rowspan: num,
          colspan: 1
        }
      }
    },
    objectSpanMethod({ row, column, rowIndex, columnIndex }) {
      if (columnIndex === 0 || columnIndex === 1) {
        return this.mergeComon('cityCode', rowIndex)
      }
    },
    getCellName({ row, column, rowIndex, columnIndex }) {
      if (!~[0, 1].indexOf(columnIndex) && !row.children) {
        return 'target-cell-class'
      }
    },
    getList() {
      let columns = [{
        label: '区域',
        prop: 'area',
        align: 'center',
        width: '60px',
        fixed: 'left'
      }, {
        label: '城市',
        prop: 'city',
        align: 'center',
        width: '60px',
        fixed: 'left'
      }, {
        label: '目标指数标题',
        prop: 'targetName',
        width: '260px',
        align: 'left',
        fixed: 'left'
      }]
      const current = this.timeType === 'month' ? this.currentMonth : this.currentWeek
      for (let i = 1; i <= current; i++) {
        columns.push({
          label: this.timeType === 'month' ? i + '月' : 'W' + i,
          prop: 'value' + i,
          count: i,
          isTime: true,
          align: 'center',
          width: '60px'
        })
      }
      this.columns = columns

      const list = [{
        id: 1,
        city: '厦门',
        cityCode: '2333',
        area: '区域一',
        targetName: '1-财务目标',
        targetId: 1,
        targetList: [{
          count: 2,
          value: 222
        }],
        children: [{
          targetId: 11,
          targetName: '2-车辆财务目标',
          targetList: [{
            count: 1,
            value: 222.3
          }, {
            count: 3,
            value: 2212.3
          }, {
            count: 7,
            value: 3212.3
          }, {
            count: 14,
            value: 320
          }],
          children: [{
            targetId: 111,
            parentId: 11,
            targetName: '3-站财务目标',
            children: [{
              targetId: 1111,
              parentId: 111,
              targetName: '4-电量财务目标',
              targetList: [{
                count: 4,
                value: 22
              }, {
                count: 6,
                value: 221
              }, {
                count: 9,
                value: 312.3
              }, {
                count: 16,
                value: 3210
              }],
              children: [{
                targetId: 11111,
                parentId: 1111,
                targetName: '5-电量财务目标',
                children: [{
                  targetId: 111111,
                  parentId: 11111,
                  targetName: '6-电量财务目标',
                  children: [{
                    targetId: 1111111,
                    parentId: 111111,
                    targetName: '7-电量财务目标',
                    children: []
                  }]
                }]
              }]
            }, {
              targetId: 1112,
              parentId: 111,
              targetName: '4-电量财务目标2',
              targetList: [{
                count: 4,
                value: 22
              }, {
                count: 6,
                value: 221
              }, {
                count: 9,
                value: 312.3
              }, {
                count: 16,
                value: 322
              }],
              children: []
            }]
          }]
        }, {
          targetId: 12,
          targetName: '2-站财务目标2',
          children: [{
            targetId: 112,
            parentId: 12,
            targetName: '3-车财务目标2',
            children: [{
              targetId: 1112,
              parentId: 112,
              targetName: '4-用电量财务目标2',
              children: []
            }]
          }]
        }, {
          targetId: 13,
          targetName: '2-经营财务目标3',
          children: []
        }, {
          targetId: 14,
          targetName: '2-车辆财务目标4',
          children: [{
            targetId: 114,
            parentId: 14,
            targetName: '3-建站财务目标4',
            children: []
          }]
        }]
      }, {
        id: 2,
        city: '厦门',
        cityCode: '2333',
        area: '区域一',
        targetName: '1-首站营业目标',
        targetId: 2,
        children: [{
          targetId: 21,
          targetName: '2-出车率营业目标',
          children: [{
            targetId: 211,
            parentId: 21,
            targetName: '3-用电率营业目标',
            children: []
          }]
        }, {
          targetId: 22,
          targetName: '2-金额营业目标2',
          children: [{
            targetId: 221,
            parentId: 22,
            targetName: '3-现金营业目标2',
            children: []
          }]
        }]
      }, {
        id: 3,
        city: '深圳',
        area: '区域一',
        cityCode: '22345',
        targetName: '1-车辆用电指标',
        targetId: 2,
        children: [{
          targetId: 31,
          targetName: '2-站车辆指标',
          children: []
        }, {
          targetId: 32,
          targetName: '2-城市车辆指标2',
          children: []
        }, {
          targetId: 33,
          targetName: '2-剩余车辆指标3',
          children: []
        }, {
          targetId: 34,
          targetName: '2-土地车辆指标4',
          children: []
        }]
      }, {
        id: 4,
        city: '泉州',
        area: '区域二',
        cityCode: '5322',
        targetName: '1-站电量指标',
        targetId: 4,
        children: [{
          targetId: 41,
          targetName: '2-车电量指标',
          children: []
        }, {
          targetId: 42,
          targetName: '2-城市电量指标2',
          children: [{
            targetId: 442,
            parentId: 42,
            targetName: '3-耗损电量指标2',
            children: []
          }]
        }, {
          targetId: 43,
          targetName: '2-解约电量指标3',
          children: []
        }]
      }]

      let array = []
      list?.forEach((item) => {
        const { id, city, area, cityCode } = item
        const temp = { id, city, area, cityCode }
        if (item?.children?.length) {
          array = [...array, { ...temp, targetName: item.targetName, isShow: true, isExpend: false, level: 0 }]
          array = [...array, ...this.treeToArray(item?.children, temp, 0)]
        } else {
          array = [...array, { ...temp, targetName: item.targetName, isShow: true, isExpend: false, level: 0 }]
        }
      })

      this.tableData = array
    },
    treeToArray(data, temp, level) {
      level++
      return data.reduce((pre, cur) => {
        const { children = [] } = cur
        const isShow = !cur.parentId
        return pre.concat([{ ...cur, ...temp, isShow: isShow, isExpend: false, level: level }], this.treeToArray(children, temp, level))
      }, [])
    },
    showTable() {
      let array = []
      this.tableData?.forEach((item) => {
        if (item.isShow) {
          array.push(item)
        }
      })
      this.showTableData = array
    },
    expendAll() {
      this.isExpend = !this.isExpend
      this.setAllExpend()
    },
    changeTime() {
      this.timeType = this.timeType == 'month' ? 'week' : 'month'
      this.getList()
    },
    cellClick(row, column, cell, event) {
      if (column.property == 'targetName') {
        let isExpend = !row.isExpend
        this.setChildrenExpend(row, row, isExpend)
      }
    },
    setChildrenExpend(root, row, isExpend) {
      if (row?.children?.length) {
        this.tableData?.forEach((item, index) => {
          if (item?.targetId == root.targetId) {
            this.$set(this.tableData[index], 'isExpend', isExpend)
          }
          if (item?.parentId == row.targetId) {
            this.$set(this.tableData[index], 'isShow', isExpend)
            if (isExpend) {
              if (item.isExpend) {
                this.setChildrenExpend(root, item, isExpend)
              }
            } else {
              this.setChildrenExpend(root, item, isExpend)
            }
          }
        })
      }
    },
    setAllExpend() {
      this.tableData?.forEach((item, index) => {
        const isShow = this.isExpend ? true : (!item?.parentId)

        this.$set(this.tableData[index], 'isShow', isShow)
        this.$set(this.tableData[index], 'isExpend', this.isExpend)
      })
    }
  }
}
</script>

<style lang="scss" scoped>
.dashboard-container {
  padding: 30px;
}
::v-deep .el-tree-table {
  .target-cell-class {
    background: #ccc;
  }
  .el-table__expand-icon {
    display: inline-block;
    width: 20px;
    line-height: 20px;
    height: 20px;
    text-align: center;
    margin-right: 3px;
    i {
      font-weight: bold;
      color: #2cb274;
    }
  }
  .el-table__placeholder {
    display: inline-block;
    width: 20px;
  }
}
</style>
