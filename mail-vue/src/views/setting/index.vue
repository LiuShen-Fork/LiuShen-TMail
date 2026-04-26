<template>
  <div class="box">
    <div class="setting-card">
      <div class="card-header">
        <div class="card-title">{{$t('profile')}}</div>
        <div class="card-desc">管理你的账号基本信息与登录凭证</div>
      </div>
      <div class="item">
        <div class="item-label">{{$t('username')}}</div>
        <div class="item-value">
          <span v-if="setNameShow" class="edit-name-input">
            <el-input v-model="accountName"  ></el-input>
            <span class="edit-name" @click="setName">
             {{$t('save')}}
            </span>
          </span>
          <span v-else class="user-name">
            <span >{{ userStore.user.name }}</span>
            <span class="edit-name" @click="showSetName">
             {{$t('change')}}
            </span>
          </span>
        </div>
      </div>
      <div class="item">
        <div class="item-label">{{$t('emailAccount')}}</div>
        <div class="item-value">{{ userStore.user.email }}</div>
      </div>
      <div class="item">
        <div class="item-label">{{$t('password')}}</div>
        <div class="item-value">
          <el-button type="primary" size="small" @click="pwdShow = true">{{$t('changePwdBtn')}}</el-button>
        </div>
      </div>
    </div>
    <div class="danger-card" v-perm="'my:delete'">
      <div class="card-header">
        <div class="card-title danger-title">{{$t('deleteUser')}}</div>
        <div class="card-desc">此操作不可逆，删除后你的所有数据将被清除</div>
      </div>
      <div class="danger-body">
        <div class="danger-icon">
          <Icon icon="mingcute:warning-line" width="20" height="20" />
        </div>
        <div class="danger-text">
          {{$t('delAccountMsg')}} 删除后无法恢复，请谨慎操作。
        </div>
      </div>
      <div>
        <el-button type="danger" size="small" @click="deleteConfirm">{{$t('deleteUserBtn')}}</el-button>
      </div>
    </div>
    <el-dialog v-model="pwdShow" :title="$t('changePassword')" width="340">
      <div class="update-pwd">
        <el-input type="password" :placeholder="$t('newPassword')" v-model="form.password" autocomplete="off"/>
        <el-input type="password" :placeholder="$t('confirmPassword')" v-model="form.newPwd" autocomplete="off"/>
        <el-button type="primary" :loading="setPwdLoading" @click="submitPwd">{{$t('save')}}</el-button>
      </div>
    </el-dialog>
  </div>
</template>
<script setup>
import {reactive, ref, defineOptions} from 'vue'
import {resetPassword, userDelete} from "@/request/my.js";
import {useUserStore} from "@/store/user.js";
import router from "@/router/index.js";
import {accountSetName} from "@/request/account.js";
import {useAccountStore} from "@/store/account.js";
import {useI18n} from "vue-i18n";
import {Icon} from "@iconify/vue";

const { t } = useI18n()
const accountStore = useAccountStore()
const userStore = useUserStore();
const setPwdLoading = ref(false)
const setNameShow = ref(false)
const accountName = ref(null)

defineOptions({
  name: 'setting'
})

function showSetName() {
  accountName.value = userStore.user.name
  setNameShow.value = true
}

function setName() {

  if (!accountName.value) {
    ElMessage({
      message: t('emptyUserNameMsg'),
      type: 'error',
      plain: true,
    })
    return;
  }

  setNameShow.value = false
  let name = accountName.value

  if (name === userStore.user.name) {
    return
  }

  userStore.user.name = accountName.value

  accountSetName(userStore.user.account.accountId,name).then(() => {
    ElMessage({
      message: t('saveSuccessMsg'),
      type: 'success',
      plain: true,
    })

    accountStore.changeUserAccountName = name

  }).catch(() => {
    userStore.user.name = name
  })
}

const pwdShow = ref(false)
const form = reactive({
  password: '',
  newPwd: '',
})

const deleteConfirm = () => {
  ElMessageBox.confirm(t('delAccountConfirm'), {
    confirmButtonText: t('confirm'),
    cancelButtonText: t('cancel'),
    type: 'warning'
  }).then(() => {
    userDelete().then(() => {
      localStorage.removeItem('token');
      router.replace('/login');
      ElMessage({
        message: t('delSuccessMsg'),
        type: 'success',
        plain: true,
      })
    })
  })
}


function submitPwd() {

  if (!form.password) {
    ElMessage({
      message: t('emptyPwdMsg'),
      type: 'error',
      plain: true,
    })
    return
  }

  if (form.password.length < 6) {
    ElMessage({
      message: t('pwdLengthMsg'),
      type: 'error',
      plain: true,
    })
    return
  }

  if (form.password !== form.newPwd) {
    ElMessage({
      message: t('confirmPwdFailMsg'),
      type: 'error',
      plain: true,
    })
    return
  }

  setPwdLoading.value = true
  resetPassword(form.password).then(() => {
    ElMessage({
      message: t('saveSuccessMsg'),
      type: 'success',
      plain: true,
    })
    pwdShow.value = false
    setPwdLoading.value = false
    form.password = ''
    form.newPwd = ''
  }).catch(() => {
    setPwdLoading.value = false
  })

}

</script>
<style scoped lang="scss">
.box {
  padding: 40px 44px;
  max-width: 720px;

  @media (max-width: 767px) {
    padding: 28px 20px;
  }

  .update-pwd {
    display: flex;
    flex-direction: column;
    gap: 14px;
  }

  .card-header {
    margin-bottom: 20px;

    .card-title {
      font-size: 18px;
      font-weight: 700;
      letter-spacing: 0.3px;
      margin-bottom: 4px;
    }

    .card-desc {
      font-size: 13px;
      color: var(--el-text-color-secondary);
    }
  }

  .setting-card {
    background: var(--el-bg-color);
    border-radius: 14px;
    border: 1px solid var(--el-border-color);
    padding: 24px 28px;
    margin-bottom: 24px;
    box-shadow: 0 2px 12px rgba(0, 0, 0, 0.04);
    transition: box-shadow 0.3s ease;

    &:hover {
      box-shadow: 0 4px 20px rgba(0, 0, 0, 0.06);
    }

    .item {
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding: 14px 0;
      border-bottom: 1px solid var(--el-border-color-lighter);

      &:last-child {
        border-bottom: none;
      }

      .item-label {
        font-weight: 600;
        color: var(--el-text-color-primary);
        font-size: 14px;
        flex-shrink: 0;
        min-width: 60px;
      }

      .item-value {
        font-size: 14px;
        color: var(--el-text-color-regular);
        text-align: right;
        overflow: hidden;
        white-space: nowrap;
        text-overflow: ellipsis;
      }

      .user-name {
        display: flex;
        align-items: center;
        gap: 8px;

        span:first-child {
          overflow: hidden;
          white-space: nowrap;
          text-overflow: ellipsis;
        }
      }

      .edit-name-input {
        display: flex;
        align-items: center;
        gap: 8px;

        .el-input {
          width: min(200px, calc(100vw - 222px));
        }
      }

      .edit-name {
        color: var(--el-color-primary);
        cursor: pointer;
        font-weight: 500;
        font-size: 13px;
        transition: opacity 0.2s;

        &:hover {
          opacity: 0.7;
        }
      }
    }
  }

  .danger-card {
    background: var(--el-bg-color);
    border-radius: 14px;
    border: 1px solid var(--el-border-color);
    padding: 24px 28px;
    box-shadow: 0 2px 12px rgba(0, 0, 0, 0.04);
    transition: box-shadow 0.3s ease;

    &:hover {
      box-shadow: 0 4px 20px rgba(0, 0, 0, 0.06);
    }

    .danger-title {
      color: var(--el-color-danger);
    }

    .danger-body {
      display: flex;
      align-items: flex-start;
      gap: 10px;
      padding: 14px 16px;
      margin: 16px 0 20px 0;
      background: var(--el-color-danger-light-9);
      border-radius: 10px;
      border: 1px solid var(--el-color-danger-light-7);

      .danger-icon {
        flex-shrink: 0;
        margin-top: 1px;
        color: var(--el-color-danger);
      }

      .danger-text {
        font-size: 13px;
        line-height: 1.6;
        color: var(--el-text-color-regular);
      }
    }
  }
}
</style>
