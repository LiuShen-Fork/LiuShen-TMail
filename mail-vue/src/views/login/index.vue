<template>
  <div id="login-box" :style=" background ? 'background: var(--el-bg-color)' : ''" v-loading="oauthLoading" element-loading-text="登录中...">
    <div id="background-wrap" v-if="!settingStore.settings.background">
      <div class="x1 cloud"></div>
      <div class="x2 cloud"></div>
      <div class="x3 cloud"></div>
      <div class="x4 cloud"></div>
      <div class="x5 cloud"></div>
    </div>
    <div v-else :style="background"></div>
    <div class="left-panel">
      <div class="bg-decor bg-decor-1"></div>
      <div class="bg-decor bg-decor-2"></div>
      <div class="bg-decor bg-decor-3"></div>
      <div class="left-content">
        <div class="avatar-wrapper">
          <div class="avatar-ring"></div>
          <div class="avatar-ring avatar-ring-delay"></div>
          <img class="avatar" src="/mail.png" alt="avatar" />
        </div>
        <h1 class="brand-name">LiuShen</h1>
        <p class="tagline">清羽飞扬</p>
        <div class="divider"></div>
        <p class="description">轻量 · 安全 · 极简</p>
        <div class="quote">
          <span class="quote-mark">"</span>
          <span class="quote-text">万物之始，大道至简</span>
          <span class="quote-mark">"</span>
        </div>

        <div class="feature-grid">
          <div class="feature-card">
            <div class="feature-icon">
              <Icon icon="mingcute:shield-check-line" width="22" height="22" />
            </div>
            <div class="feature-title">隐私保护</div>
            <div class="feature-desc">临时邮箱按需使用，无需绑定真实身份</div>
          </div>
          <div class="feature-card">
            <div class="feature-icon">
              <Icon icon="mingcute:rocket-line" width="22" height="22" />
            </div>
            <div class="feature-title">全球加速</div>
            <div class="feature-desc">基于 Cloudflare Workers 边缘节点部署</div>
          </div>
          <div class="feature-card">
            <div class="feature-icon">
              <Icon icon="mingcute:mail-send-line" width="22" height="22" />
            </div>
            <div class="feature-title">收发一体</div>
            <div class="feature-desc">支持邮件收发与附件管理，功能完备</div>
          </div>
          <div class="feature-card">
            <div class="feature-icon">
              <Icon icon="mingcute:moon-stars-line" width="22" height="22" />
            </div>
            <div class="feature-title">深色模式</div>
            <div class="feature-desc">自适应深浅主题，舒适的阅读体验</div>
          </div>
        </div>

        <div class="stats-row">
          <div class="stat-item">
            <div class="stat-num">∞</div>
            <div class="stat-label">无限邮箱</div>
          </div>
          <div class="stat-divider"></div>
          <div class="stat-item">
            <div class="stat-num">0s</div>
            <div class="stat-label">即时送达</div>
          </div>
          <div class="stat-divider"></div>
          <div class="stat-item">
            <div class="stat-num">100%</div>
            <div class="stat-label">开源免费</div>
          </div>
        </div>

        <div class="tech-badges">
          <span class="badge">Cloudflare Workers</span>
          <span class="badge">Vue 3</span>
          <span class="badge">Element Plus</span>
          <span class="badge">D1 Database</span>
        </div>

        <div class="info-section">
          <div class="info-item">
            <div class="info-icon">
              <Icon icon="mingcute:information-line" width="16" height="16" />
            </div>
            <div class="info-body">
              <div class="info-title">关于本站</div>
              <div class="info-desc">清羽飞扬自建临时邮箱服务，可用于临时注册账号、接收验证码、保护个人隐私，避免真实邮箱被泄露和骚扰。</div>
            </div>
          </div>
          <div class="info-item">
            <div class="info-icon">
              <Icon icon="mingcute:passport-line" width="16" height="16" />
            </div>
            <div class="info-body">
              <div class="info-title">清羽通行证</div>
              <div class="info-desc">使用清羽通行证一键注册登录，全站账号打通，一个身份畅行清羽生态下的所有服务。</div>
            </div>
          </div>
          <div class="info-item legal">
            <div class="info-icon">
              <Icon icon="mingcute:warning-line" width="14" height="14" />
            </div>
            <div class="info-body">
              <div class="info-desc">请遵守当地法律法规，禁止用于任何违法用途。如收到投诉或发现违规行为，本站有权收回邮箱并配合调查。</div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class="form-wrapper">
      <div class="container">
        <div class="mobile-avatar-wrapper">
          <div class="avatar-ring"></div>
          <div class="avatar-ring avatar-ring-delay"></div>
          <img class="avatar" src="/mail.png" alt="avatar" />
        </div>
        <span class="form-title">{{ settingStore.settings.title }}</span>
        <span class="form-desc" v-if="show === 'login'">{{ $t('loginTitle') }}</span>
        <div v-show="show === 'login'">
          <el-input :class="settingStore.settings.loginDomain === 0 ? 'email-input' : ''" v-model="form.email"
                    type="text" :placeholder="$t('emailAccount')" autocomplete="off">
            <template #append v-if="settingStore.settings.loginDomain === 0">
              <div @click.stop="openSelect">
                <el-select
                    v-if="show === 'login'"
                    ref="mySelect"
                    v-model="suffix"
                    :placeholder="$t('select')"
                    class="select"
                >
                  <el-option
                      v-for="item in domainList"
                      :key="item"
                      :label="item"
                      :value="item"
                  />
                </el-select>
                <div style="color: var(--el-text-color-primary)">
                  <span>{{ suffix }}</span>
                  <Icon class="setting-icon" icon="mingcute:down-small-fill" width="20" height="20"/>
                </div>
              </div>
            </template>
          </el-input>
          <el-input v-model="form.password" :placeholder="$t('password')" type="password" autocomplete="off">
          </el-input>
          <el-button class="btn" type="primary" @click="submit" :loading="loginLoading"
          >{{ $t('loginBtn') }}
          </el-button>
          <el-button class="btn" v-if="settingStore.settings.casdoorSwitch"  style="margin-top: 10px"  @click="casdoorLogin">
            <Icon icon="mdi:shield-account" width="18" height="18" style="margin-right: 10px" />清羽通行证
          </el-button>
        </div>
        <div v-show="show !== 'login'">
          <div style="color: var(--form-desc-color); margin-bottom: 18px;">
            请通过清羽通行证完成注册
          </div>
          <el-button class="btn" type="primary" @click="casdoorLogin">
            <Icon icon="mdi:shield-account" width="18" height="18" style="margin-right: 10px" />清羽通行证注册
          </el-button>
        </div>
        <template v-if="settingStore.settings.register === 0 && settingStore.settings.casdoorSwitch">
          <div class="switch" @click="show = 'register'" v-if="show === 'login'">{{ $t('noAccount') }}
            <span>{{ $t('regSwitch') }}</span></div>
          <div class="switch" @click="show = 'login'" v-else>{{ $t('hasAccount') }} <span>{{ $t('loginSwitch') }}</span>
          </div>
        </template>
      </div>
    </div>
    <el-dialog class="bind-dialog" v-model="showBindForm"  title="注册邮箱" >
      <div class="bind-container">
        <el-input v-model="bindForm.email" type="text" :placeholder="$t('emailAccount')" autocomplete="off">
          <template #append>
            <div @click.stop="openSelect">
              <el-select
                  ref="mySelect"
                  v-model="suffix"
                  :placeholder="$t('select')"
                  class="select"
              >
                <el-option
                    v-for="item in domainList"
                    :key="item"
                    :label="item"
                    :value="item"
                />
              </el-select>
              <div>
                <span>{{ suffix }}</span>
                <Icon class="setting-icon" icon="mingcute:down-small-fill" width="20" height="20"/>
              </div>
            </div>
          </template>
        </el-input>
        <el-input v-if="settingStore.settings.regKey === 0" v-model="bindForm.code" :placeholder="$t('regKey')"
                  type="text" autocomplete="off"/>
        <el-input v-if="settingStore.settings.regKey === 2" v-model="bindForm.code"
                  :placeholder="$t('regKeyOptional')" type="text" autocomplete="off"/>
        <el-button class="btn" type="primary" @click="bind" :loading="bindLoading"
        >绑定
        </el-button>
      </div>
    </el-dialog>
    <a class="github" href="https://github.com/LiuShen-Fork/LiuShen-TMail" target="_blank" rel="noopener">
      <Icon icon="mingcute:github-line" color="#5dba7d" width="20" height="20" />
    </a>
  </div>
</template>

<script setup>
import router from "@/router";
import {computed, reactive, ref} from "vue";
import {login} from "@/request/login.js";
import {isEmail} from "@/utils/verify-utils.js";
import {useSettingStore} from "@/store/setting.js";
import {useAccountStore} from "@/store/account.js";
import {useUserStore} from "@/store/user.js";
import {useUiStore} from "@/store/ui.js";
import {Icon} from "@iconify/vue";
import {cvtR2Url} from "@/utils/convert.js";
import {loginUserInfo} from "@/request/my.js";
import {permsToRouter} from "@/perm/perm.js";
import {useI18n} from "vue-i18n";
import {oauthBindUser, oauthCasdoorLogin} from "@/request/ouath.js";

const {t} = useI18n();
const accountStore = useAccountStore();
const userStore = useUserStore();
const uiStore = useUiStore();
const settingStore = useSettingStore();
const loginLoading = ref(false)
const bindLoading = ref(false)
const oauthLoading = ref(false);
const showBindForm = ref(false);
const show = ref('login')

const bindForm = reactive({
  email: '',
  oauthUserId: '',
  code: ''
})

const form = reactive({
  email: '',
  password: '',

});
const mySelect = ref()
const suffix = ref('')
const domainList = settingStore.domainList;
suffix.value = domainList[0]

const loginOpacity = computed(() => {
  const opacity = settingStore.settings.loginOpacity
  return uiStore.dark ? `rgba(0, 0, 0, ${opacity})` : `rgba(255, 255, 255, ${opacity})`
})

const background = computed(() => {

  return settingStore.settings.background ? {
    'background-image': `url(${cvtR2Url(settingStore.settings.background)})`,
    'background-repeat': 'no-repeat',
    'background-size': 'cover',
    'background-position': 'center'
  } : ''
})

const openSelect = () => {
  mySelect.value.toggleMenu()
}

function casdoorLogin() {
  const endpoint = settingStore.settings.casdoorEndpoint
  const clientId = settingStore.settings.casdoorClientId
  const redirectUri = encodeURIComponent(settingStore.settings.casdoorCallbackUrl)
  window.location.href =
      `${endpoint}/login/oauth/authorize?client_id=${clientId}&redirect_uri=${redirectUri}&response_type=code&scope=openid+profile+email&state=casdoor`
}

casdoorGetUser();

async function casdoorGetUser() {

  const params = new URLSearchParams(window.location.search)
  const code = params.get('code')

  if (code) {

    oauthLoading.value = true
    oauthCasdoorLogin(code).then(data => {

      bindForm.oauthUserId = data.userInfo.oauthUserId;

      if (!data.token) {
        showBindForm.value = true
        oauthLoading.value = false
        ElMessage({
          message: '请注册绑定一个邮箱',
          type: 'warning',
          duration: 4000,
          plain: true,
        })
        return;
      }

      saveToken(data.token);
    }).catch(() => {
      oauthLoading.value = false
    })
  }

  const cleanUrl = window.location.origin + window.location.pathname
  window.history.replaceState({}, '', cleanUrl)
}

function bind() {

  if (!bindForm.email) {
    ElMessage({
      message: t('emptyEmailMsg'),
      type: 'error',
      plain: true,
    })
    return
  }


  if (bindForm.email.length < settingStore.settings.minEmailPrefix) {
    ElMessage({
      message: t('minEmailPrefix', {msg: settingStore.settings.minEmailPrefix}),
      type: 'error',
      plain: true,
    })
    return
  }

  let email = bindForm.email + suffix.value;


  if (!isEmail(email)) {
    ElMessage({
      message: t('notEmailMsg'),
      type: 'error',
      plain: true,
    })
    return
  }

  if (settingStore.settings.regKey === 0) {

    if (!bindForm.code) {

      ElMessage({
        message: t('emptyRegKeyMsg'),
        type: 'error',
        plain: true,
      })
      return
    }

  }

  const form = {email: bindForm.email + suffix.value, oauthUserId: bindForm.oauthUserId, code: bindForm.code}

  bindLoading.value = true
  oauthBindUser(form).then(data => {
    saveToken(data.token)
  }).catch(() => {
    bindLoading.value = false
  })
}

const submit = () => {

  if (!form.email) {
    ElMessage({
      message: t('emptyEmailMsg'),
      type: 'error',
      plain: true,
    })
    return
  }

  let email = form.email + (settingStore.settings.loginDomain === 0 ? suffix.value : '');

  if (!isEmail(email)) {
    ElMessage({
      message: t('notEmailMsg'),
      type: 'error',
      plain: true,
    })
    return
  }

  if (!form.password) {
    ElMessage({
      message: t('emptyPwdMsg'),
      type: 'error',
      plain: true,
    })
    return
  }

  loginLoading.value = true
  login(email, form.password).then(async data => {
    await saveToken(data.token)
  }).finally(() => {
    loginLoading.value = false
  })
}

async function saveToken(token) {
  localStorage.setItem('token', token)
  const user = await loginUserInfo();
  accountStore.currentAccountId = user.account.accountId;
  accountStore.currentAccount = user.account;
  userStore.user = user;
  const routers = permsToRouter(user.permKeys);
  routers.forEach(routerData => {
    router.addRoute('layout', routerData);
  });
  await router.replace({name: 'layout'})
  uiStore.showNotice()
  oauthLoading.value = false;
  bindLoading.value = false;
}

</script>


<style>
.el-select-dropdown__item {
  padding: 0 15px;
}

.no-autofill-pwd {
  .el-input__inner {
    -webkit-text-security: disc !important;
  }
}
</style>

<style lang="scss" scoped>

.left-panel {
  position: fixed;
  left: 0;
  top: 0;
  height: 100%;
  width: calc(100% - 440px);
  z-index: 10;
  display: flex;
  align-items: flex-start;
  justify-content: flex-start;
  pointer-events: none;
  overflow: hidden;
  @media (max-width: 1024px) {
    width: calc(100% - 400px);
  }
  @media (max-width: 767px) {
    display: none;
  }

  .bg-decor {
    position: absolute;
    border-radius: 32px;
    pointer-events: none;
    z-index: 0;
  }

  .bg-decor-1 {
    width: 320px;
    height: 320px;
    top: -80px;
    right: -60px;
    background: rgba(93, 186, 125, 0.04);
    border: 1px solid rgba(93, 186, 125, 0.06);
    animation: floatDecor 20s ease-in-out infinite;
  }

  .bg-decor-2 {
    width: 240px;
    height: 240px;
    bottom: 60px;
    right: 40px;
    background: rgba(93, 186, 125, 0.03);
    border: 1px solid rgba(93, 186, 125, 0.05);
    border-radius: 28px;
    animation: floatDecor 24s ease-in-out infinite 3s;
  }

  .bg-decor-3 {
    width: 180px;
    height: 180px;
    top: 45%;
    right: -40px;
    background: rgba(93, 186, 125, 0.03);
    border: 1px solid rgba(93, 186, 125, 0.05);
    border-radius: 24px;
    animation: floatDecor 18s ease-in-out infinite 6s;
  }

  .left-content {
    pointer-events: auto;
    max-width: 880px;
    width: 100%;
    padding: 24px 40px;
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    text-align: left;
    max-height: 100vh;
    overflow-y: auto;
    scrollbar-width: none;
    -ms-overflow-style: none;
    animation: fadeInUp 0.8s ease both;

    &::-webkit-scrollbar {
      display: none;
    }
  }

  .avatar-wrapper {
    position: relative;
    width: 120px;
    height: 120px;
    margin-bottom: 18px;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .avatar {
    width: 96px;
    height: 96px;
    border-radius: 50%;
    object-fit: cover;
    border: 3px solid rgba(93, 186, 125, 0.3);
    box-shadow: 0 8px 32px rgba(93, 186, 125, 0.15);
    position: relative;
    z-index: 2;
    transition: transform 0.4s ease, box-shadow 0.4s ease;
  }

  .avatar:hover {
    transform: scale(1.05);
    box-shadow: 0 12px 40px rgba(93, 186, 125, 0.25);
  }

  .avatar-ring {
    position: absolute;
    width: 120px;
    height: 120px;
    border-radius: 50%;
    border: 1.5px solid rgba(93, 186, 125, 0.25);
    animation: pulseRing 3s ease-out infinite;
    z-index: 1;
  }

  .avatar-ring-delay {
    animation-delay: 1.5s;
    width: 136px;
    height: 136px;
    border-color: rgba(93, 186, 125, 0.12);
  }

  .brand-name {
    font-size: 32px;
    font-weight: 700;
    letter-spacing: 2px;
    margin: 0 0 6px 0;
    background: linear-gradient(135deg, #3a9a5b, #5dba7d, #3a9a5b);
    background-size: 200% 200%;
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
    animation: gradientText 6s ease infinite;
  }

  .tagline {
    font-size: 14px;
    color: var(--secondary-text-color);
    margin: 0 0 12px 0;
    letter-spacing: 4px;
    font-weight: 300;
    opacity: 0.8;
  }

  .divider {
    width: 40px;
    height: 2px;
    background: linear-gradient(90deg, transparent, var(--el-color-primary-light-3), transparent);
    margin-bottom: 14px;
    border-radius: 1px;
    background: linear-gradient(90deg, var(--el-color-primary-light-5), var(--el-color-primary-light-7), transparent);
  }

  .description {
    font-size: 13px;
    letter-spacing: 6px;
    color: var(--el-text-color-secondary);
    margin: 0 0 16px 0;
    font-weight: 400;
    text-transform: uppercase;
  }

  .quote {
    display: flex;
    align-items: center;
    gap: 6px;
    font-style: italic;
    color: var(--el-text-color-placeholder);
    font-size: 13px;
    letter-spacing: 1px;
    opacity: 0.7;

    .quote-mark {
      font-size: 18px;
      color: var(--el-color-primary-light-5);
      font-family: Georgia, serif;
      line-height: 1;
    }

    .quote-text {
      font-weight: 300;
    }
  }

  .feature-grid {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 10px;
    width: 100%;
    @media (max-width: 1024px) {
      grid-template-columns: repeat(2, 1fr);
    }
    margin-top: 16px;
    margin-bottom: 16px;
    animation: fadeInUp 0.9s ease both;
    animation-delay: 0.2s;
  }

  .feature-card {
    background: rgba(93, 186, 125, 0.04);
    border: 1px solid rgba(93, 186, 125, 0.08);
    border-radius: 12px;
    padding: 12px 14px;
    text-align: left;
    transition: all 0.3s ease;

    &:hover {
      background: rgba(93, 186, 125, 0.08);
      border-color: rgba(93, 186, 125, 0.15);
      transform: translateY(-2px);
    }

    .feature-icon {
      display: flex;
      justify-content: flex-start;
      margin-bottom: 8px;
      color: var(--el-color-primary);
    }

    .feature-title {
      font-size: 13px;
      font-weight: 600;
      color: var(--el-text-color-primary);
      margin-bottom: 4px;
    }

    .feature-desc {
      font-size: 11px;
      color: var(--el-text-color-secondary);
      line-height: 1.5;
    }
  }

  .stats-row {
    display: flex;
    align-items: center;
    justify-content: flex-start;
    gap: 20px;
    margin-bottom: 16px;
    animation: fadeInUp 1s ease both;
    animation-delay: 0.3s;
  }

  .stat-item {
    text-align: left;

    .stat-num {
      font-size: 20px;
      font-weight: 700;
      background: linear-gradient(135deg, #3a9a5b, #5dba7d);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      background-clip: text;
      margin-bottom: 2px;
    }

    .stat-label {
      font-size: 11px;
      color: var(--el-text-color-secondary);
      letter-spacing: 1px;
    }
  }

  .stat-divider {
    width: 1px;
    height: 24px;
    background: linear-gradient(180deg, transparent, var(--el-color-primary-light-7), transparent);
  }

  .tech-badges {
    display: flex;
    flex-wrap: wrap;
    justify-content: flex-start;
    gap: 8px;
    animation: fadeInUp 1.1s ease both;
    animation-delay: 0.4s;
  }

  .badge {
    padding: 4px 12px;
    border-radius: 20px;
    font-size: 12px;
    font-weight: 500;
    background: rgba(93, 186, 125, 0.08);
    color: var(--el-color-primary);
    border: 1px solid rgba(93, 186, 125, 0.12);
    letter-spacing: 0.2px;
    transition: all 0.2s ease;

    &:hover {
      background: rgba(93, 186, 125, 0.14);
      border-color: rgba(93, 186, 125, 0.2);
    }
  }

  .info-section {
    width: 100%;
    margin-top: 16px;
    display: flex;
    flex-direction: column;
    gap: 14px;
    animation: fadeInUp 1.2s ease both;
    animation-delay: 0.5s;
  }

  .info-item {
    display: flex;
    align-items: flex-start;
    gap: 10px;
    text-align: left;
    padding: 12px 14px;
    border-radius: 10px;
    background: rgba(93, 186, 125, 0.03);
    border: 1px solid rgba(93, 186, 125, 0.06);
    transition: all 0.25s ease;

    &:hover {
      background: rgba(93, 186, 125, 0.06);
      border-color: rgba(93, 186, 125, 0.1);
    }

    .info-icon {
      flex-shrink: 0;
      margin-top: 2px;
      color: var(--el-color-primary);
      opacity: 0.8;
    }

    .info-body {
      display: flex;
      flex-direction: column;
      gap: 3px;
    }

    .info-title {
      font-size: 13px;
      font-weight: 600;
      color: var(--el-text-color-primary);
    }

    .info-desc {
      font-size: 12px;
      color: var(--el-text-color-secondary);
      line-height: 1.6;
    }

    &.legal {
      background: rgba(245, 158, 11, 0.03);
      border-color: rgba(245, 158, 11, 0.08);

      .info-icon {
        color: var(--el-color-warning);
      }

      .info-desc {
        color: var(--el-text-color-placeholder);
        font-size: 11px;
      }

      &:hover {
        background: rgba(245, 158, 11, 0.05);
        border-color: rgba(245, 158, 11, 0.12);
      }
    }
  }
}

@keyframes floatDecor {
  0%, 100% {
    transform: translateY(0) rotate(0deg);
  }
  33% {
    transform: translateY(-12px) rotate(2deg);
  }
  66% {
    transform: translateY(8px) rotate(-1deg);
  }
}

@keyframes fadeInUp {
  from {
    opacity: 0;
    transform: translateY(30px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes pulseRing {
  0% {
    transform: scale(0.8);
    opacity: 0.6;
  }
  50% {
    opacity: 0.3;
  }
  100% {
    transform: scale(1.15);
    opacity: 0;
  }
}

@keyframes gradientText {
  0%, 100% {
    background-position: 0% 50%;
  }
  50% {
    background-position: 100% 50%;
  }
}

.form-wrapper {
  position: fixed;
  right: 0;
  height: 100%;
  z-index: 10;
  display: flex;
  align-items: center;
  justify-content: center;
  @media (max-width: 767px) {
    width: 100%;
  }
}

.container {
  background: v-bind(loginOpacity);
  backdrop-filter: blur(20px);
  -webkit-backdrop-filter: blur(20px);
  padding: 0 44px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  width: 440px;
  height: 100%;
  border-left: 1px solid var(--light-border-color);
  @media (max-width: 1024px) {
    padding: 24px 22px;
    width: 380px;
    margin-left: 18px;
  }
  @media (max-width: 767px) {
    padding: 28px 24px;
    border-radius: 16px;
    border: 1px solid var(--light-border-color);
    height: fit-content;
    width: 100%;
    margin-right: 18px;
    margin-left: 18px;
    box-shadow: 0 8px 32px rgba(0, 0, 0, 0.06);
  }

  .mobile-avatar-wrapper {
    display: none;
    position: relative;
    width: 100px;
    height: 100px;
    margin: 0 auto 20px auto;
    align-items: center;
    justify-content: center;

    @media (max-width: 767px) {
      display: flex;
    }

    .avatar {
      width: 72px;
      height: 72px;
      border-radius: 50%;
      object-fit: cover;
      border: 2.5px solid rgba(93, 186, 125, 0.3);
      box-shadow: 0 6px 24px rgba(93, 186, 125, 0.15);
      position: relative;
      z-index: 2;
    }

    .avatar-ring {
      position: absolute;
      width: 100px;
      height: 100px;
      border-radius: 50%;
      border: 1.5px solid rgba(93, 186, 125, 0.25);
      animation: pulseRing 3s ease-out infinite;
      z-index: 1;
    }

    .avatar-ring-delay {
      animation-delay: 1.5s;
      width: 114px;
      height: 114px;
      border-color: rgba(93, 186, 125, 0.12);
    }
  }

  .btn {
    height: 40px;
    width: 100%;
    border-radius: 10px;
    font-weight: 500;
    font-size: 14px;
  }

  .form-desc {
    margin-top: 4px;
    margin-bottom: 22px;
    color: var(--form-desc-color);
    font-size: 13.5px;
  }

  .form-title {
    font-weight: 700;
    font-size: 23px !important;
    letter-spacing: 0.3px;
  }

  .switch {
    margin-top: 22px;
    text-align: center;
    font-size: 13px;

    span {
      color: var(--login-switch-color);
      cursor: pointer;
      font-weight: 500;
    }
  }

  :deep(.el-input__wrapper) {
    border-radius: 10px;
    background: var(--el-bg-color);
    box-shadow: 0 0 0 1px var(--light-border-color) inset;
    transition: box-shadow 0.2s ease;
  }

  :deep(.el-input__wrapper:hover) {
    box-shadow: 0 0 0 1px var(--el-color-primary-light-5) inset;
  }

  :deep(.el-input__wrapper.is-focus) {
    box-shadow: 0 0 0 1px var(--el-color-primary) inset;
  }

  .email-input :deep(.el-input__wrapper) {
    border-radius: 10px 0 0 10px;
    background: var(--el-bg-color);
  }

  .el-input {
    height: 40px;
    width: 100%;
    margin-bottom: 16px;

    :deep(.el-input__inner) {
      height: 38px;
    }
  }
}

:deep(.el-select-dropdown__item) {
  padding: 0 10px;
}

:deep(.bind-dialog) {
  width: 400px !important;
  border-radius: 14px !important;
  @media (max-width: 440px) {
    width: calc(100% - 40px) !important;
    margin-right: 20px !important;
    margin-left: 20px !important;
  }
}

.bind-container {
  display: grid;
  grid-template-columns: 1fr;
  gap: 14px;
}

.setting-icon {
  position: relative;
  top: 6px;
}

.github {
  position: fixed;
  width: 36px;
  height: 36px;
  display: flex;
  justify-content: center;
  align-items: center;
  border-radius: 10px;
  background: var(--el-bg-color);
  bottom: 12px;
  right: 12px;
  z-index: 1000;
  border: 1px solid var(--light-border-color);
  cursor: pointer;
  opacity: 0.7;
  transition: opacity 0.15s ease;
  &:hover {
    opacity: 1;
  }
}

:deep(.el-input-group__append) {
  padding: 0 !important;
  padding-left: 8px !important;
  padding-right: 4px !important;
  background: var(--el-bg-color);
  border-radius: 0 10px 10px 0;
}

:deep(.el-button+.el-button) {
  margin: 0;
}

.select {
  position: absolute;
  right: 30px;
  width: 100px;
  opacity: 0;
  pointer-events: none;
}

.custom-style {
  margin-bottom: 10px;
}

.custom-style .el-segmented {
  --el-border-radius-base: 10px;
  width: 180px;
}


#login-box {
  background: linear-gradient(145deg, #e8f5ec, #d4edda, #f0f9f4, #eaf6f0);
  background-size: 300% 300%;
  animation: gradientShift 12s ease infinite;
  height: 100%;
  margin: 0;
  padding: 0;
  overflow-x: hidden;
  display: grid;
  grid-template-columns: 1fr;
}

@keyframes gradientShift {
  0%, 100% { background-position: 0% 50%; }
  50% { background-position: 100% 50%; }
}

.dark #login-box {
  background: linear-gradient(145deg, #1a2e20, #1e2b22, #1a261e);
}


#background-wrap {
  height: 100%;
  z-index: 0;
  position: relative;
  overflow: hidden;
}

/* Floating leaf-like shapes instead of clouds */
@keyframes floatShape {
  0% { transform: translateX(-200px) translateY(0) rotate(0deg); opacity: 0; }
  10% { opacity: 0.15; }
  90% { opacity: 0.15; }
  100% { transform: translateX(calc(100vw + 200px)) translateY(-40px) rotate(180deg); opacity: 0; }
}

.x1 {
  animation: floatShape 28s ease-in-out infinite;
  position: absolute;
  top: 15%;
  width: 120px;
  height: 120px;
  border-radius: 50%;
  background: radial-gradient(circle, rgba(93, 186, 125, 0.12), transparent 70%);
}

.x2 {
  animation: floatShape 20s ease-in-out infinite 3s;
  position: absolute;
  top: 40%;
  width: 80px;
  height: 80px;
  border-radius: 50%;
  background: radial-gradient(circle, rgba(93, 186, 125, 0.1), transparent 70%);
}

.x3 {
  animation: floatShape 24s ease-in-out infinite 7s;
  position: absolute;
  top: 60%;
  width: 140px;
  height: 140px;
  border-radius: 50%;
  background: radial-gradient(circle, rgba(58, 154, 91, 0.08), transparent 70%);
}

.x4 {
  animation: floatShape 18s ease-in-out infinite 2s;
  position: absolute;
  top: 25%;
  width: 60px;
  height: 60px;
  border-radius: 50%;
  background: radial-gradient(circle, rgba(93, 186, 125, 0.1), transparent 70%);
}

.x5 {
  animation: floatShape 22s ease-in-out infinite 5s;
  position: absolute;
  top: 75%;
  width: 100px;
  height: 100px;
  border-radius: 50%;
  background: radial-gradient(circle, rgba(93, 186, 125, 0.09), transparent 70%);
}

.cloud {
  /* Reusing existing class for soft gradient orbs */
  background: none;
  border-radius: 50%;
  box-shadow: none;
  height: auto;
  width: auto;
}

.cloud:after,
.cloud:before {
  content: none;
}

</style>
