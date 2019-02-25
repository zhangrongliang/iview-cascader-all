<template>
<div class="display-inline-block">
    <ul class="ivu-cascader-menu">
        <CheckboxGroup v-model="checkBoxGroup" @on-change="handleCheckBoxChange">
            <li class="ivu-cascader-menu-item" v-for="(item, index) in data" :key="index" :class="{'ivu-cascader-menu-item-active': (!item.parentId && selected == index )|| selected == index}">
                <!-- 判断是否第一项开启多选功能，或是否为’全选‘’字段 -->
                <span v-if="((item.parentId && !oneToone) || allToone) && item.value !== 'all'">
                    <Checkbox @click.native.stop="handleCheckBoxClick" :label="index" class="w-full">{{item.label}}
                        <i class="ivu-icon ivu-icon-ios-arrow-right" v-if="item.children && item.children.length"></i>
                    </Checkbox>
                </span>
                <span v-else>
                    <p @click="handleClick(index,item)">
                        {{item.label}}
                        <i class="ivu-icon ivu-icon-ios-arrow-right" v-if="item.children && item.children.length"></i>
                    </p>
                </span>
            </li>
        </CheckboxGroup>
    </ul>
    <casMultiPanel @handleGetSelected="selectedData" v-if="children.length && !destroy" :data="children.length && children" :childrencheckedData="childrenSelected" :componentId="componentId" :oneToone="oneToone" :allToone="allToone"></casMultiPanel>
</div>
</template>

<script>
export default {
    name: "casMultiPanel",
    props: {
        data: {
            type: Array,
            default () {
                return [];
            }
        },
        // 子级选中数据
        childrencheckedData: {
            type: Array,
            default () {
                return [];
            }
        },
        componentId: {
            type: String,
            default: ''
        },
        oneToone: {
            type: Boolean,
            default: false
        },
        allToone: {
            type: Boolean,
            default: false
        }
    },
    data() {
        return {
            // 当前已选择项
            checkBoxGroup: [],
            // 子组件数据
            children: [],
            // 子组件选中数据
            childrenSelected: [],
            // 一级组件被选中高亮
            selected: -1,
            destroy: false
        };
    },
    watch: {
        // 重新渲染组件
        destroy(val) {
            if (val) {
                this.$nextTick(_ => {
                    this.destroy = false;
                });
            }
        },
        checkBoxGroup(val, oldVal) {
            if (val.length > oldVal.length) return
            oldVal.map((vv, kk) => {
                let type = false
                val.map((v, k) => {
                    if (v == vv) type = true
                })
                if (!type) {
                    if (this.data && this.data.length && this.data[vv].children) {
                        this.data[vv].children.map((dataVal, dataKey) => {
                            dataVal.checked = false
                        })
                    }
                }
            })
        }
    },
    mounted() {
        // 当前组件数据选中状态赋值
        let arr = []
        this.data.map((val, key) => {
            if (val.checked) arr.push(key)
        })
        this.handleCheckBoxChange(arr)
    },
    methods: {
        // 获取选择项数据
        selectedData(val = []) {
            let arr = []
            this.data.map((val, key) => {
                if (val.checked) arr.push(val)
            })
            // 合并子组件传递的参数,并emit到父组件
            this.$emit("handleGetSelected", arr.concat(val));
        },
        //   防止时间冒泡到父组件handleClose事件
        handleCheckBoxClick() {},
        // 处理数据排序问题，push带来的顺序跟原始数据可能会不一致问题异常
        sortNumber(a, b) {
            return a.parentId - b.parentId
        },
        // checkGroup变更事件,返回已选中的数组
        handleCheckBoxChange(arr) {
            this.checkBoxGroup = arr
            // 清空记录
            this.children = [];
            // 遍历选择的数据
            arr.map((k, v) => {
                // 存在children字段则记录
                if (this.data[k].children && this.data[k].children.length) {
                    // 判断添加多选数据
                    if (this.allToone && this.children.length === 0) {
                        this.children.push({
                            value: 'all',
                            label: '全部',
                            parentId: 0
                        })
                    }
                    // 记录数据并渲染到子组件
                    Array.prototype.push.apply(this.children, this.data[k].children);
                    this.children.sort(this.sortNumber)
                }
            });

            // 重置checked
            this.data.map((val, key) => {
                val.checked = false
            })
            this.checkBoxGroup.map((val, key) => {
                this.data[val].checked = true
            })

            this.destroy = true;
            // 触发父组件emit
            this.selectedData();
        },
        // 一级组件点击事件
        handleClick(index, item) {
            this.selected = index;
            if (item.value === 'all') {
                let data = [];
                for (let i = 1; i < this.data.length; i++) {
                    data.push(i);
                }
                this.checkBoxGroup = data;
                this.handleCheckBoxChange(data);
            } else {
                this.checkBoxGroup = [index];
                this.handleCheckBoxChange([index]);
            }
            this.destroy = true;
        }
    }
};
</script>
