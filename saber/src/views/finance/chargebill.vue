<template>
  <basic-container>
    <avue-crud
      :option="option"
      :table-loading="loading"
      :data="data"
      :page.sync="page"
      :permission="permissionList"
      :before-open="beforeOpen"
      v-model="form"
      ref="crud"
      @row-update="rowUpdate"
      @row-save="rowSave"
      @row-del="rowDel"
      @search-change="searchChange"
      @search-reset="searchReset"
      @selection-change="selectionChange"
      @current-change="currentChange"
      @size-change="sizeChange"
      @refresh-change="refreshChange"
      @on-load="onLoad"
    >
      <template slot="menuLeft">
        <el-button
          type="danger"
          size="small"
          icon="el-icon-delete"
          plain
          v-if="permission.xjorderinfo_delete"
          @click="handleDelete"
          >删 除
        </el-button>
      </template>
      <template slot="searchMenu" slot-scope="scope">
        <el-button size="small" @click="searchaaa">自定义按钮</el-button>
      </template>
    </avue-crud>
  </basic-container>
</template>

<script>
import {
  getList,
  getDetail,
  add,
  update,
  remove,
} from "@/api/business/xjorderinfo";
import { mapGetters } from "vuex";

export default {
  data() {
    return {
      form: {},
      query: {},
      loading: true,
      page: {
        currentPage: 1,
        total: 0,
        layout: "total, prev, pager, next, jumper",
        background: true,
        pageSize: 10,
      },
      selectionList: [],
      option: {
        height: "auto", //657
        calcHeight: 30,
        tip: false,
        searchMenuSpan: 6,
        border: true,
        index: true,
        viewBtn: true,
        selection: true,
        menuWidth: 150,
        excelBtn: true,
        menu: false,
        dialogClickModal: false,
        indexLabel: "序号",
        column: [
          {
            label: "主键ID",
            prop: "id",
            hide: true,
            rules: [
              {
                required: true,
                message: "请输入主键ID",
                trigger: "blur",
              },
            ],
          },
          {
            label: "订单编号",
            prop: "orderId",
            search: true,
            searchOrder: 3,
            rules: [
              {
                required: true,
                message: "请输入订单编号",
                trigger: "blur",
              },
            ],
          },
          {
            label: "订单名称",
            prop: "orderName",
            hide: true,
            rules: [
              {
                required: true,
                message: "请输入订单名称",
                trigger: "blur",
              },
            ],
          },
          {
            label: "订单类型",
            prop: "orderType",
            type: "select",
            width: 70,
            dicData: [
              {
                label: "充电",
                value: "1",
              },
              {
                label: "待定",
                value: "2",
              },
            ],
            rules: [
              {
                required: true,
                message: "请输入订单类型",
                trigger: "blur",
              },
            ],
          },
          {
            label: "订单金额",
            prop: "ordeAmtVo",
            width: 70,
            // formatter: function (value) {
            //   return value.ordeAmt / 100 + "元";
            // },
            rules: [
              {
                required: true,
                message: "请输入订单金额",
                trigger: "blur",
              },
            ],
          },
          {
            label: "抵扣金额",
            prop: "discountAmt",
            hide: true,
            rules: [
              {
                required: true,
                message: "请输入抵扣金额",
                trigger: "blur",
              },
            ],
          },
          {
            label: "客户ID",
            prop: "custId",
            hide: true,
            rules: [
              {
                required: true,
                message: "请输入客户ID",
                trigger: "blur",
              },
            ],
          },
          {
            label: "商户ID",
            prop: "orgId",
            hide: true,
            rules: [
              {
                required: true,
                message: "请输入商户ID",
                trigger: "blur",
              },
            ],
          },
          {
            label: "开始时间",
            prop: "startTime",
            hide: true,
            addDisplay: false,
            rules: [
              {
                required: true,
                message: "请输入开始时间",
                trigger: "blur",
              },
            ],
          },
          {
            label: "结束时间",
            prop: "endTime",
            addDisplay: false,
            hide: true,
            rules: [
              {
                required: true,
                message: "请输入结束时间",
                trigger: "blur",
              },
            ],
          },
          {
            label: "订单状态",
            prop: "status",
            dicData: [
              {
                label: "充电中",
                value: "1",
              },
              {
                label: "订单完成",
                value: "2",
              },
            ],
            rules: [
              {
                required: true,
                message: "请输入订单状态",
                trigger: "blur",
              },
            ],
          },
          {
            label: "消费时间",
            prop: "createTime",
            search: true,
            width: 150,
            type: "datetimerange",
            addDisplay: false,
            format: "yyyy-MM-dd HH:mm:ss",
            valueFormat: "yyyy-MM-dd HH:mm:ss",
            searchSpan: 10,
            searchRange: true,
            searchOrder: 0,
            rules: [
              {
                required: true,
                message: "请输入消费时间",
                trigger: "blur",
              },
            ],
          },
          {
            label: "更新时间",
            prop: "updateTime",
            hide: true,
            addDisplay: false,
            rules: [
              {
                required: true,
                message: "请输入更新时间",
                trigger: "blur",
              },
            ],
          },
          {
            label: "充电量",
            prop: "totalPowerVo",
            // formatter: function (value) {
            //   return value.totalPower / 100 + "度";
            // },
            rules: [
              {
                required: true,
                message: "请输入充电量",
                trigger: "blur",
              },
            ],
          },
          {
            label: "用户来源",
            prop: "userSource",
            search: true,
            searchOrder: 2,
            dicData: [
              {
                label: "环建物业",
                value: 1,
              },
              {
                label: "联城一家",
                value: 2,
              },
            ],
            rules: [
              {
                required: true,
                message: "请输入用户来源",
                trigger: "blur",
              },
            ],
          },
          {
            label: "站点名称",
            prop: "remark1",
            search: true,
            width: 130,
            searchOrder: 1,
            rules: [
              {
                required: true,
                message: "请输入站点名称",
                trigger: "blur",
              },
            ],
          },
          {
            label: "折扣率",
            prop: "remark2",
            width: 60,
            rules: [
              {
                required: true,
                message: "请输入折扣率",
                trigger: "blur",
              },
            ],
          },
          {
            label: "总电费",
            prop: "totalElecMoneyVo",
            width: 80,
            // formatter: function (value) {
            //   return value.totalElecMoney / 100 + "元";
            // },
            rules: [
              {
                required: true,
                message: "请输入",
                trigger: "blur",
              },
            ],
          },
          {
            label: "总服务费",
            width: 80,
            prop: "totalSeviceMoneyVo",
            // formatter: function (value) {
            //   return value.totalSeviceMoney / 100 + "元";
            // },
            rules: [
              {
                required: true,
                message: "请输入",
                trigger: "blur",
              },
            ],
          },
          {
            label: "枪Id",
            prop: "connectorId",
            hide: true,
            rules: [
              {
                required: true,
                message: "请输入",
                trigger: "blur",
              },
            ],
          },
          {
            label: "充电手动结束 默认为空 -1为用户点击结束充电按钮",
            prop: "manualEnd",
            hide: true,
            rules: [
              {
                required: true,
                message: "请输入充电手动结束 默认为空 -1为用户点击结束充电按钮",
                trigger: "blur",
              },
            ],
          },
          {
            label: "默认为空,为1时表示余额不足而断电",
            prop: "balanStatus",
            hide: true,
            rules: [
              {
                required: true,
                message: "请输入默认为空,为1时表示余额不足而断电",
                trigger: "blur",
              },
            ],
          },
          {
            label: "记录余额不足时停桩默认为空,1为已发送停桩指令",
            prop: "recStopCharge",
            hide: true,
            rules: [
              {
                required: true,
                message: "请输入记录余额不足时停桩默认为空,1为已发送停桩指令",
                trigger: "blur",
              },
            ],
          },
        ],
      },
      data: [],
    };
  },
  computed: {
    ...mapGetters(["permission"]),
    permissionList() {
      return {
        // addBtn: this.vaildData(this.permission.xjorderinfo_add, false),
        // viewBtn: this.vaildData(this.permission.xjorderinfo_view, false),
        // delBtn: this.vaildData(this.permission.xjorderinfo_delete, false),
        // editBtn: this.vaildData(this.permission.xjorderinfo_edit, false),
      };
    },
    ids() {
      let ids = [];
      this.selectionList.forEach((ele) => {
        ids.push(ele.id);
      });
      return ids.join(",");
    },
  },
  methods: {
    rowSave(row, done, loading) {
      add(row).then(
        () => {
          this.onLoad(this.page);
          this.$message({
            type: "success",
            message: "操作成功!",
          });
          done();
        },
        (error) => {
          loading();
          window.console.log(error);
        }
      );
    },
    rowUpdate(row, index, done, loading) {
      update(row).then(
        () => {
          this.onLoad(this.page);
          this.$message({
            type: "success",
            message: "操作成功!",
          });
          done();
        },
        (error) => {
          loading();
          console.log(error);
        }
      );
    },
    rowDel(row) {
      this.$confirm("确定将选择数据删除?", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        type: "warning",
      })
        .then(() => {
          return remove(row.id);
        })
        .then(() => {
          this.onLoad(this.page);
          this.$message({
            type: "success",
            message: "操作成功!",
          });
        });
    },
    handleDelete() {
      if (this.selectionList.length === 0) {
        this.$message.warning("请选择至少一条数据");
        return;
      }
      this.$confirm("确定将选择数据删除?", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        type: "warning",
      })
        .then(() => {
          return remove(this.ids);
        })
        .then(() => {
          this.onLoad(this.page);
          this.$message({
            type: "success",
            message: "操作成功!",
          });
          this.$refs.crud.toggleSelection();
        });
    },
    beforeOpen(done, type) {
      if (["edit", "view"].includes(type)) {
        getDetail(this.form.id).then((res) => {
          this.form = res.data.data;
        });
      }
      done();
    },
    searchReset() {
      this.query = {};
      this.onLoad(this.page);
    },
    searchaaa() {
      console.log(123456);
    },
    searchChange(params, done) {
      this.query = params;
      this.page.currentPage = 1;
      this.onLoad(this.page, params);
      done();
    },
    selectionChange(list) {
      this.selectionList = list;
    },
    selectionClear() {
      this.selectionList = [];
      this.$refs.crud.toggleSelection();
    },
    currentChange(currentPage) {
      this.page.currentPage = currentPage;
    },
    sizeChange(pageSize) {
      this.page.pageSize = pageSize;
    },
    refreshChange() {
      this.onLoad(this.page, this.query);
    },
    onLoad(page, params = {}) {
      var createTime = this.query.createTime;
      var quee = this.query;
      let values = {
        ...params,
      };
      if (createTime) {
        values = {
          ...params,
          startTime: createTime[0],
          endTime: createTime[1],
        };
        values.createTime = null;
        quee = null;
      }
      this.loading = true;
      getList(
        page.currentPage,
        page.pageSize,
        Object.assign(values, quee)
      ).then((res) => {
        const data = res.data.data;
        this.page.total = data.total;
        this.data = data.records;
        this.loading = false;
        this.selectionClear();
      });
    },
  },
};
</script>

<style>
</style>
