<template>
  <div class="home">
    <h1>RABC-3 Demo</h1>
    <div>
      选择用户:
      <select @change="onUserChangeListener" v-model="selectedUser" id="username">
        <option v-for="user in RBAC.User" :key="user.UID" :value="user.UID">
          {{user.name}}
        </option>
      </select>
    </div>
    <div>
      选择角色:
      <select @change="onRolesChangeListener" v-model="selectedRole" id="rolename">
        <option v-for="(role,index) in user.roleNames" :key="role" :value="index">
          {{role}}
        </option>
      </select>
    </div>
    <h2>我是{{user.name}}，角色为{{user.roleNames[user.activatedRoleNo]}}</h2>
    <h3>我的父角色为:{{pRoleNames}}</h3>
    <div class="button-field">
      <function-button button-name="作业提交" p-i-d-required="1"></function-button>
      <function-button button-name="作业点评" p-i-d-required="2"></function-button>
      <function-button button-name="成绩统计" p-i-d-required="3"></function-button>
      <function-button button-name="系统信息管理" p-i-d-required="4"></function-button>
      <function-button button-name="个人信息管理" p-i-d-required="5"></function-button>
    </div>
    <div class="info-board">
      <h4>系统日志：</h4>
      <info-board></info-board>
    </div>
  </div>
</template>

<script>
import { mapState } from "vuex";
import FunctionButton from "@/components/FunctionButton.vue";
import InfoBoard from "@/components/InfoBoard.vue";

export default {
  name: "home",
  components: {
    FunctionButton,
    InfoBoard
  },
  mounted() {
    // 设置初始默认用户
    this.$store.commit("setUser", "1001");
  },
  computed: {
    pRoleNames() {
      return this.$store.getters.pRoleNames;
    },
    ...mapState(["user", "RBAC"])
  },
  data() {
    return {
      selectedUser: "1001", //默认值为二狗子
      selectedRole: 0 //默认为第一个角色
    };
  },
  methods: {
    onUserChangeListener(event) {
      this.$store.commit("setUser", event.target.value);
      this.selectedRole = 0; //恢复默认选项为第一个角色
    },
    onRolesChangeListener(event) {
      // 提交role的index
      this.$store.commit("setActivatedRoleNo", event.target.value);
    }
  }
};
</script>
