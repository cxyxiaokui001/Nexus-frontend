<template>
  <div id="userLoginView">
    <h2 class="title">用户登录</h2>
    <div class="desc">不写一行代码，生成完整应用</div>
    <a-form :model="formState" name="basic" autocomplete="off" @finish="handleSubmit">
      <a-form-item name="userAccount" :rules="[{ required: true, message: '请输入账号' }]">
        <a-input v-model:value="formState.userAccount" placeholder="请输入账号" />
      </a-form-item>
      <a-form-item
        name="userPassword"
        :rules="[
          { required: true, message: '请输入密码' },
          { min: 8, message: '密码不能小于 8 位' },
        ]"
      >
        <a-input-password v-model:value="formState.userPassword" placeholder="请输入密码" />
      </a-form-item>
      <div class="tips">
        没有账号？
        <RouterLink to="/user/register">去注册</RouterLink>
      </div>
      <a-form-item>
        <a-button type="primary" html-type="submit" style="width: 100%">登录</a-button>
      </a-form-item>
    </a-form>
  </div>
</template>

<script setup lang="ts">
import { reactive } from 'vue'
import { useRouter } from 'vue-router'
import { useLoginUserStore } from '@/stores/loginUser.ts'
import { userLogin } from '@/api/userController.ts'
import { message } from 'ant-design-vue'

const formState = reactive<API.UserLoginRequest>({
  userAccount: '',
  userPassword: '',
})
const router = useRouter()
const loginUserStore = useLoginUserStore()

/**
 * 提交表单
 * @param values
 */
const handleSubmit = async (values: any) => {
  const res = await userLogin(values)
  console.log(res)
  // 登录成功，把登录态保存到全局状态中
  if (res.data.code === 0 && res.data.data) {
    await loginUserStore.fetchLoginUser()
    message.success('登录成功')
    router.push({
      path: '/',
      replace: true,
    })
  } else {
    message.error('登录失败，' + res.data.message)
  }
}

</script>
<style scoped>
#userLoginView {
  max-width: 360px;
  margin: 0 auto;
}

.title {
  text-align: center;
  margin-bottom: 16px;
}

.desc {
  text-align: center;
  color: #bbb;
  margin-bottom: 16px;
}

.tips {
  margin-bottom: 16px;
  color: #bbb;
  font-size: 13px;
  text-align: right;
}

</style>
<!--
<template>
  <div class="body">
    <div id="userLoginView">
      <div class="title">登录</div>
      <form action="#" >
        <div class="field">
&lt;!&ndash;          <input required="" type="text" class="input" />&ndash;&gt;
          <input type="text" class="input" v-model="formState.userAccount" placeholder="账号"/>
          <span class="span"
            ><svg
              class=""
              xml:space="preserve"
              style="enable-background: new 0 0 512 512"
              viewBox="0 0 512 512"
              y="0"
              x="0"
              height="20"
              width="50"
              xmlns:xlink="http://www.w3.org/1999/xlink"
              version="1.1"
              xmlns="http://www.w3.org/2000/svg"
            >
              <g>
                <path
                  class=""
                  data-original="#000000"
                  fill="#595959"
                  d="M256 0c-74.439 0-135 60.561-135 135s60.561 135 135 135 135-60.561 135-135S330.439 0 256 0zM423.966 358.195C387.006 320.667 338.009 300 286 300h-60c-52.008 0-101.006 20.667-137.966 58.195C51.255 395.539 31 444.833 31 497c0 8.284 6.716 15 15 15h420c8.284 0 15-6.716 15-15 0-52.167-20.255-101.461-57.034-138.805z"
                ></path>
              </g></svg
          ></span>
        </div>

        <div class="field" name="userPassword" :rules="[{required: true,message:'请输入密码'},{min: 8,message:'密码长度不能小于8'}]">
&lt;!&ndash;          <input required="" type="password" class="input" />&ndash;&gt;
          <input type="text" class="input" v-model="formState.userPassword" placeholder="密码"/>
          <span class="span"
            ><svg
              class=""
              xml:space="preserve"
              style="enable-background: new 0 0 512 512"
              viewBox="0 0 512 512"
              y="0"
              x="0"
              height="20"
              width="50"
              xmlns:xlink="http://www.w3.org/1999/xlink"
              version="1.1"
              xmlns="http://www.w3.org/2000/svg"
            >
              <g>
                <path
                  class=""
                  data-original="#000000"
                  fill="#595959"
                  d="M336 192h-16v-64C320 57.406 262.594 0 192 0S64 57.406 64 128v64H48c-26.453 0-48 21.523-48 48v224c0 26.477 21.547 48 48 48h288c26.453 0 48-21.523 48-48V240c0-26.477-21.547-48-48-48zm-229.332-64c0-47.063 38.27-85.332 85.332-85.332s85.332 38.27 85.332 85.332v64H106.668zm0 0"
                ></path>
              </g></svg
          ></span>
        </div>

        <div class="forgot-pass">
          <a href="#">忘记密码？</a>
        </div>
        <button class="button" type="submit" @click="handleSubmit">登录</button>
        <div class="sign-up">
          没有账号？
          <RouterLink to="/user/register">立即注册</RouterLink>
&lt;!&ndash;          <a href="#">立即注册</a>&ndash;&gt;
        </div>
      </form>
    </div>
  </div>
</template>

<script setup lang="ts">
import { reactive } from 'vue'
import { useRouter } from 'vue-router'
import { userLogin } from '@/api/userController.ts'
import { useLoginUserStore } from '@/stores/loginUser.ts'
import { message } from 'ant-design-vue'

const formState = reactive<API.UserLoginRequest>({
  userAccount: '',
  userPassword: '',
})
const router = useRouter()
const loginUserStore = useLoginUserStore()


/**
 * 提交表单
 * @param values
 */
const handleSubmit = async () => {
  const res = await userLogin(formState)
  // 登录成功，把登录态保存到全局状态中
  if (res.data.code === 0 && res.data.data) {
    await loginUserStore.fetchLoginUser()
    message.success('登录成功')
    console.log('准备跳转...')
    await router.push({ path: '/', replace: true })
    console.log('跳转完成')
    router.push({
      path: '/',
      replace: true,
    })
  } else {
    message.error('登录失败，' + res.data.message)
  }
}
</script>

<style scoped>
.body {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 70vh;
  padding: 20px;
}

#userLoginView {
  width: 330px;
  padding: 40px 30px;
  background: rgb(248, 220, 152, 0.5);
  border-radius: 10px;
  box-shadow:
    -3px -3px 7px #ffffff73,
    2px 2px 5px rgba(94, 104, 121, 0.288);
}

#userLoginView .title {
  font-size: 33px;
  font-weight: 600;
  margin-bottom: 35px;
  color: #595959;
}

.field {
  height: 50px;
  width: 100%;
  display: flex;
  position: relative;
}

.field:nth-child(2) {
  margin-top: 20px;
}

.field .input {
  height: 100%;
  width: 100%;
  padding-left: 45px;
  outline: none;
  border: none;
  font-size: 18px;
  background: rgb(248, 220, 152);
  color: #595959;
  border-radius: 25px;
  box-shadow:
    inset 2px 2px 5px #babecc,
    inset -5px -5px 10px #ffffff73;

}
/* 修改占位符字体样式 */
.field .input::placeholder {
  color: #666666;
  font-size: 15px;
  letter-spacing: 0.5px;

}
.field .input:focus {
  box-shadow:
    inset 1px 1px 2px #babecc,
    inset -1px -1px 2px #ffffff73;
}

.field .span {
  position: absolute;
  color: #595959;
  width: 50px;
  line-height: 55px;
}

.field .input:valid ~ label {
  opacity: 0;
}

.forgot-pass {
  text-align: left;
  margin: 10px 0 10px 5px;
}

.forgot-pass a {
  font-size: 16px;
  color: #666666;
  text-decoration: none;
}

.forgot-pass:hover a {
  text-decoration: underline;
}

.button {
  margin: 15px 0;
  width: 100%;
  height: 50px;
  font-size: 18px;
  line-height: 50px;
  font-weight: 600;
  background: rgb(248, 220, 152);
  border-radius: 25px;
  border: none;
  outline: none;
  cursor: pointer;
  color: #595959;
  box-shadow:
    2px 2px 5px #babecc,
    -5px -5px 10px #ffffff73;
}

.button:focus {
  color: #3498db;
  box-shadow:
    inset 2px 2px 5px #babecc,
    inset -5px -5px 10px #ffffff73;
}

.sign-up {
  margin: 10px 0;
  color: #595959;
  font-size: 16px;
}

.sign-up a {
  color: #3498db;
  text-decoration: none;
}

.sign-up a:hover {
  text-decoration: underline;
}
</style>

-->
