<template>
  <div class="tree-transfer-box">
    <a-row>
      <a-col :span="11">
        <div class="tree-transfer-list">
          <div class="tree-transfer-header">
            <a-checkbox
              :indeterminate="indeterminate"
              @change="onCheckAllChange"
              :checked="checkedAll"
            >
              <span class="ant-transfer-list-header-selected"><span>全选</span><span
                class="ant-transfer-list-header-title"></span></span>
            </a-checkbox>
          </div>
          <div class="">
            <div class="tree-transfer-search">
              <a-input v-model="keywords" @keyup="filterTreeData" placeholder="请输入搜索内容"/>
            </div>
            <ul class="ant-transfer-list-content">
              <li v-for="(item) in treeData" :key="item.value">
                <a-checkbox
                  :indeterminate="item.indeterminate"
                  :checked="item.checked"
                  @change="onCheckChange(item,'left')"/>
                <span class="checkSpan" @click="toggleUp(item)">{{ item.title }}
                  <a-icon
                    :type="item.isShow?'down':'right'"
                    style="margin-left: 8px;"/>
                </span>
                <template v-for="child in item.children">
                  <ul v-if="item.isShow" class="tree-child-ul" :key="child.value">
                    <li>
                      <a-checkbox
                        @change="childCheckChange(child,item,'left')"
                        :checked="child.checked">{{ child.title }}
                      </a-checkbox>
                    </li>
                  </ul>
                </template>
              </li>
            </ul>
          </div>
        </div>
      </a-col>
      <a-col class="center_row" :span="2">
        <div style="text-align: center;">
          <div>
            <a-button
              type="primary"
              @click="setRightTree"
              size="small"
              :disabled="leftRow"
              icon="right"></a-button>
          </div>
          <div style="margin-top:20px;">
            <a-button
              type="primary"
              @click="setLeftTree"
              size="small"
              :disabled="rightRow"
              icon="left"></a-button>
          </div>
        </div>
      </a-col>
      <a-col :span="11">
        <div class="tree-transfer-list">
          <div class="tree-transfer-header">
            <a-checkbox
              :indeterminate="rightIndeterminate"
              @change="rightCheckAllChange"
              :checked="rightCheckedAll"
            >
              <span class="ant-transfer-list-header-selected"><span>全选</span><span
                class="ant-transfer-list-header-title"></span></span>
            </a-checkbox>
          </div>
          <div class="">
            <div class="tree-transfer-search">
              <a-input v-model="rightKeywords" @keyup="filterRightTreeData" placeholder="请输入搜索内容"/>
            </div>
            <ul class="ant-transfer-list-content">
              <li v-for="item in rightTreeData" :key="item.value">
                <a-checkbox
                  :indeterminate="item.indeterminate"
                  :checked="item.checked"
                  @change="onCheckChange(item,'right')"/>
                <span class="checkSpan" @click="toggleUp(item)">{{ item.title }}
                  <a-icon :type="item.isShow ? 'down' : 'right'" style="margin-left: 8px;"/>
                </span>
                <ul v-if="item.isShow" class="tree-child-ul" v-for="child in item.children" :key="child.value">
                  <li>
                    <a-checkbox
                      @change="childCheckChange(child,item,'right')"
                      :checked="child.checked">{{ child.title }}
                    </a-checkbox>
                  </li>
                </ul>
              </li>
            </ul>
          </div>
        </div>
      </a-col>
    </a-row>
  </div>
</template>

<script>
  const sourceData = [{
    title: 'Node1',
    value: '0-0',
    key: '0-0',
    checked: false,
    indeterminate: false,
    children: [{
      title: 'Child Node1',
      value: '0-0-0',
      key: '0-0-0',
      checked: false
    }]
  }, {
    title: 'Node2',
    value: '0-1',
    key: '0-1',
    checked: false,
    indeterminate: false,
    children: [{
      title: 'Child Node3',
      value: '0-1-0',
      key: '0-1-0',
      checked: false,
      disabled: true
    }, {
      title: 'Child Node4',
      value: '0-1-1',
      key: '0-1-1',
      checked: false
    }, {
      title: 'Child Node5',
      value: '0-1-2',
      checked: false,
      key: '0-1-2'
    }]
  }]
  export default {
    name: 'TreeTransfer',
    components: {},
    data () {
      return {
        keywords: '',
        rightKeywords: '',
        leftData: [],
        treeData: sourceData,
        rightTreeData: [],
        rightData: [],
        indeterminate: false,
        checkedAll: false,
        leftRow: true,
        rightIndeterminate: false,
        rightCheckedAll: false,
        rightRow: true
      }
    },
    mounted () {
      this.leftData = this.treeData
    },
    methods: {
      toggleUp (item) {
        if (!item.isShow) {
          this.$set(item, 'isShow', false)
        }
        item.isShow = !item.isShow
      },
      rightCheckAllChange () { // 右边全选
        if (this.rightCheckedAll) {
          this.rightCheckedAll = false
          this.rightIndeterminate = false
          this.rightRow = true
          this.rightTreeData.forEach(row => {
            row.checked = false
            row.indeterminate = false
            if (row.children) {
              row.children.forEach(child => {
                child.checked = false
              })
            }
          })
        } else {
          this.rightCheckedAll = true
          this.rightIndeterminate = false
          this.rightRow = false
          this.rightTreeData.forEach(row => {
            row.checked = true
            row.indeterminate = false
            if (row.children) {
              row.children.forEach(child => {
                child.checked = true
              })
            }
          })
        }
        // this.setIndeterminate('right');
      },
      onCheckAllChange () { // 全选
        if (this.checkedAll) {
          this.checkedAll = false
          this.indeterminate = false
          this.leftRow = true
          this.treeData.forEach(row => {
            row.checked = false
            row.indeterminate = false
            if (row.children) {
              row.children.forEach(child => {
                child.checked = false
              })
            }
          })
        } else {
          this.checkedAll = true
          this.indeterminate = false
          this.leftRow = false
          this.treeData.forEach(row => {
            row.checked = true
            row.indeterminate = false
            if (row.children) {
              row.children.forEach(child => {
                child.checked = true
              })
            }
          })
        }
        console.log(this.treeData)
        //  this.setIndeterminate('left');
      },
      onCheckChange (item, flag) { // 父选
        if (item.checked) {
          this.$set(item, 'checked', false)
          this.$set(item, 'indeterminate', false)
          if (flag === 'left') {
            this.leftData.forEach(row => {
              if (row.title === item.title) {
                this.$set(row, 'checked', false)
                this.$set(row, 'indeterminate', false)
                this.leftRow = true
                if (row.children) {
                  row.children.forEach(child => {
                    this.$set(child, 'checked', false)
                  })
                }
              }
            })
          } else {
            this.rightData.forEach(row => {
              if (row.title === item.title) {
                this.$set(row, 'checked', false)
                this.$set(row, 'indeterminate', false)
                this.rightRow = true
                if (row.children) {
                  row.children.forEach(child => {
                    this.$set(child, 'checked', false)
                  })
                }
              }
            })
          }
        } else {
          this.$set(item, 'checked', true)
          this.$set(item, 'indeterminate', false)
          if (flag === 'left') {
            this.leftData.forEach(row => {
              if (row.title === item.title) {
                this.$set(row, 'checked', true)
                this.$set(row, 'indeterminate', false)
                this.leftRow = false
                if (row.children) {
                  row.children.forEach(child => {
                    this.$set(child, 'checked', true)
                  })
                }
              }
            })
          } else {
            this.rightData.forEach(row => {
              if (row.title === item.title) {
                this.$set(row, 'checked', true)
                this.$set(row, 'indeterminate', false)
                this.rightRow = false
                if (row.children) {
                  row.children.forEach(child => {
                    this.$set(child, 'checked', true)
                  })
                }
              }
            })
          }
        }
        this.setIndeterminate(flag)
      },
      childCheckChange (child, parent, flag) {
        if (child.checked) {
          this.$set(child, 'checked', false)
        } else {
          this.$set(child, 'checked', true)
        }
        // 设置中间态
        const checkedList = []
        let children = []
        if (flag === 'left') {
          this.leftData.forEach(row => {
            if (row.title === parent.title) {
              children = row.children
              row.children.forEach(childRow => {
                if (childRow.title === child.title) {
                  childRow.checked = child.checked
                }
                if (childRow.checked) {
                  checkedList.push(childRow)
                }
              })
            }
          })
          this.$set(parent, 'indeterminate', !!checkedList.length && (checkedList.length < children.length))
          this.$set(parent, 'checked', checkedList.length === children.length)
          const _this = this
          this.leftData.forEach(row => {
            if (row.title === parent.title) {
              _this.$set(row, 'checked', parent.checked)
              _this.$set(row, 'indeterminate', parent.indeterminate)
            }
          })
        } else {
          this.rightData.forEach(row => {
            if (row.title === parent.title) {
              children = row.children
              row.children.forEach(childRow => {
                if (childRow.title === child.title) {
                  childRow.checked = child.checked
                }
                if (childRow.checked) {
                  checkedList.push(childRow)
                }
              })
            }
          })
          this.$set(parent, 'indeterminate', !!checkedList.length && (checkedList.length < children.length))
          this.$set(parent, 'checked', checkedList.length === children.length)
          const _this = this
          this.rightData.forEach(row => {
            if (row.title === parent.title) {
              _this.$set(row, 'checked', parent.checked)
              _this.$set(row, 'indeterminate', parent.indeterminate)
            }
          })
        }
        this.setIndeterminate(flag)
      },
      setIndeterminate (flag) {
        // 设置中间态
        const checkedList = []
        const indeterminateList = []
        if (flag === 'left') {
          const plainOptions = this.treeData
          if (plainOptions.length === 0) {
            this.leftRow = true
            this.checkedAll = false
            this.indeterminateList = false
            return
          }
          this.treeData.forEach(row => {
            if (row.checked) {
              checkedList.push(row)
            }
            if (row.indeterminate) {
              indeterminateList.push(row)
            }
          })
          this.indeterminate = !!indeterminateList.length || !!checkedList.length && (checkedList.length < plainOptions.length)
          this.checkedAll = checkedList.length === plainOptions.length
          this.leftRow = !(this.indeterminate || this.checkedAll)
        } else {
          const plainOptions = this.rightTreeData
          if (plainOptions.length === 0) {
            this.rightRow = true
            this.rightCheckedAll = false
            this.rightIndeterminateList = false
            return
          }
          this.rightTreeData.forEach(row => {
            if (row.checked) {
              checkedList.push(row)
            }
            if (row.indeterminate) {
              indeterminateList.push(row)
            }
          })
          this.rightIndeterminate = !!indeterminateList.length || !!checkedList.length && (checkedList.length < plainOptions.length)
          this.rightCheckedAll = checkedList.length === plainOptions.length
          this.rightRow = !(this.rightIndeterminate || this.rightCheckedAll)
        }
      },
      filterRightTreeData () { // 过滤右边搜索
        const keywords = this.rightKeywords
        const searchArr = []
        if (!keywords) {
          this.rightTreeData = this.rightData
          return
        }
        this.rightTreeData.forEach(row => {
          if (row.children) {
            const obj = JSON.parse(JSON.stringify(row))
            obj.children = []
            row.children.forEach(child => {
              if (child.title.indexOf(keywords) !== -1) {
                obj.children.push(child)
              }
            })
            if (obj.children.length > 0) {
              searchArr.push(obj)
            }
          }
        })
        this.rightTreeData = searchArr
      },
      filterTreeData () { // 过滤左边搜索
        const keywords = this.keywords
        const searchArr = []
        if (!keywords) {
          this.treeData = this.leftData
          return
        }
        this.treeData.forEach(row => {
          if (row.children) {
            const obj = JSON.parse(JSON.stringify(row))
            obj.children = []
            row.children.forEach(child => {
              if (child.title.indexOf(keywords) !== -1) {
                obj.children.push(child)
              }
            })
            if (obj.children.length > 0) {
              searchArr.push(obj)
            }
          }
        })
        this.treeData = searchArr
      },
      setRightTree () { // 设置右边树
        const rightTree = this.rightTreeData
        const newLeftTree = []
        this.leftData.forEach(row => {
          if (row.checked) {
            const isOn = rightTree.filter(rightRow => {
              return rightRow.title === row.title
            })
            if (isOn.length === 0) {
              const tempObj = row
              tempObj.checked = false
              tempObj.isShow = true
              tempObj.children.forEach(tem => {
                tem.checked = false
              })
              rightTree.push(tempObj)
            } else {
              rightTree.forEach(rightRow => {
                if (rightRow.title === row.title) {
                  row.children.forEach(child => {
                    if (child.checked) {
                      const tempObj = JSON.parse(JSON.stringify(child))
                      tempObj.checked = false
                      rightRow.children.push(tempObj)
                    }
                  })
                }
              })
            }
          } else if (row.indeterminate && row.children) {
            const isOn = rightTree.filter(rightRow => {
              return rightRow.title === row.title
            })
            if (isOn.length === 0) {
              const rightObj = JSON.parse(JSON.stringify(row))
              rightObj.children = []
              rightObj.indeterminate = false
              row.children.forEach(child => {
                if (child.checked) {
                  const tempObj = JSON.parse(JSON.stringify(child))
                  tempObj.checked = false
                  rightObj.children.push(tempObj)
                }
              })
              if (rightObj.children.length > 0) {
                rightObj.checked = false
                rightObj.isShow = true
                rightTree.push(rightObj)
              }
            } else {
              rightTree.forEach(rightRow => {
                if (rightRow.title === row.title) {
                  row.children.forEach(child => {
                    if (child.checked) {
                      const tempObj = JSON.parse(JSON.stringify(child))
                      tempObj.checked = false
                      rightRow.children.push(tempObj)
                    }
                  })
                }
              })
            }
            const filterRow = row.children.filter(row => {
              return !row.checked
            })
            if (filterRow.length > 0) {
              const obj = {}
              obj.title = row.title
              obj.value = row.value
              obj.key = row.key
              obj.checked = row.checked
              obj.isShow = row.isShow
              obj.children = filterRow
              newLeftTree.push(obj)
            }
          } else {
            newLeftTree.push(row)
          }
        })
        this.treeData = newLeftTree
        this.leftData = newLeftTree
        this.rightTreeData = this.rightData = rightTree
        this.filterTreeData()
        this.setIndeterminate('left')
      },
      setLeftTree () { // 设置左边树
        const leftTree = this.treeData
        const newRightTree = []
        this.rightData.forEach(row => {
          if (row.checked) {
            const isOn = leftTree.filter(leftRow => {
              return leftRow.title === row.title
            })
            if (isOn.length === 0) {
              const tempObj = row
              tempObj.checked = false
              tempObj.isShow = true
              tempObj.children.forEach(tem => {
                tem.checked = false
              })
              leftTree.push(tempObj)
            } else {
              leftTree.forEach(leftRow => {
                if (leftRow.title === row.title) {
                  row.children.forEach(child => {
                    if (child.checked) {
                      const tempObj = JSON.parse(JSON.stringify(child))
                      tempObj.checked = false
                      leftRow.children.push(tempObj)
                    }
                  })
                }
              })
            }
          } else if (row.indeterminate && row.children) {
            const isOn = leftTree.filter(leftRow => {
              return leftRow.title === row.title
            })
            if (isOn.length === 0) {
              const leftObj = JSON.parse(JSON.stringify(row))
              leftObj.children = []
              leftObj.indeterminate = false
              row.children.forEach(child => {
                if (child.checked) {
                  const tempObj = JSON.parse(JSON.stringify(child))
                  tempObj.checked = false
                  leftObj.children.push(tempObj)
                }
              })
              if (leftObj.children.length > 0) {
                leftObj.checked = false
                leftObj.isShow = true
                leftTree.push(leftObj)
              }
            } else {
              leftTree.forEach(leftRow => {
                if (leftRow.title === row.title) {
                  row.children.forEach(child => {
                    if (child.checked) {
                      const tempObj = JSON.parse(JSON.stringify(child))
                      tempObj.checked = false
                      leftRow.children.push(tempObj)
                    }
                  })
                }
              })
            }
            const filterRow = row.children.filter(row => {
              return !row.checked
            })
            if (filterRow.length > 0) {
              const obj = {}
              obj.title = row.title
              obj.value = row.value
              obj.key = row.key
              obj.checked = row.checked
              obj.isShow = row.isShow
              obj.children = filterRow
              newRightTree.push(obj)
            }
          } else {
            newRightTree.push(row)
          }
        })
        this.rightTreeData = newRightTree
        this.rightData = newRightTree
        this.treeData = this.leftData = leftTree
        this.filterRightTreeData()
        this.setIndeterminate('right')
      }
    }
  }
</script>
<style lang="less">
  .tree-transfer-box .ant-checkbox {
    margin-right: 4px;
  }
</style>
<style type="text/css" scoped>
  .tree-transfer-box {
    width: 100%;
  }

  .center_row {
    height: 430px;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .tree-transfer-list {
    border: 1px solid #eee;
    border-radius: 5px;
  }

  .tree-transfer-list .checkSpan {
    padding-left: 4px;
    cursor: pointer;
  }

  .tree-transfer-header {
    padding: 10px 10px;
    border-bottom: 1px solid #eee;
  }

  .tree-transfer-search {
    padding: 8px 10px;
  }

  .ant-transfer-list-content {
    height: 300px;
    overflow-y: auto;
    padding: 0 10px;
  }

  .ant-transfer-list-content > li {
    line-height: 35px;
  }

  .tree-child-ul {
    list-style: none;
    margin: 0;
    padding: 0 0 0 15px;
  }

  .tree-child-ul > li {
    line-height: 35px;
  }
</style>
