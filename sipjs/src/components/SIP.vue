<template>
    <div class="softphone">
        <div id="audio">
            <audio id="remoteVideo"></audio>
            <audio id="localVideo" muted="muted"></audio>
        </div>
        <el-button-group>
            <!-- <el-button type="primary" icon="el-icon-edit" id="softphone-answer" @click="onAnswer">接听</el-button> -->
            <el-button type="primary" icon="el-icon-edit" id="softphone-hangup" @click="onHangUp">挂断</el-button>
            <el-button type="primary" icon="el-icon-edit" id="softphone-makecall" @click="makeCall">拨打</el-button>
            <el-button type="primary" id="softphone-login" @click="loginFormVisible = true">登录</el-button>
        </el-button-group>

        <el-dialog title="登录" :visible.sync="loginFormVisible">
            <el-form :model="form" ref="form" label-width="80px">
                <el-form-item label="IP">
                    <el-input v-model="form.ip"></el-input>
                </el-form-item>
                <el-form-item label="分机号">
                    <el-input v-model="form.name"></el-input>
                </el-form-item>
                <el-form-item label="密码">
                    <el-input type="password" v-model="form.password"></el-input>
                </el-form-item>
            </el-form>
            <div slot="footer" class="dialog-footer">
                <el-button @click="loginFormVisible = false">取 消</el-button>
                <el-button type="primary" @click="onLogin">确 定</el-button>
            </div>
        </el-dialog>
    </div>
</template>

<script>
import SIP from 'sip.js'

export default {
    name: 'SIP',
    data () {
        return {
            loginFormVisible: false,
            form: {
                ip: '192.168.11.134',
                name: '1001',
                password: 'test',
            },
            session: undefined,
            isHolding: false
        }
    },
    methods: {
        onLogin () {
            let _that = this

            let options = {
                media: {
                    local: {
                        audio: document.getElementById('localVideo')
                    },
                    remote: {
                        audio: document.getElementById('remoteVideo')
                    }
                },
                ua: {
                    uri: _that.form.name + '@' + _that.form.ip,
                    wsServers: 'wss://' + _that.form.ip + ':7443',
                    // FreeSWITCH Default Username
                    authorizationUser: _that.form.name,

                    // FreeSWITCH Default Password
                    password: _that.form.password,
                    displayName: _that.form.name
                }
            };
            _that.ua = new SIP.Web.Simple(options)
            _that.ua.on('ringing', function () {
                _that.$notify({
                    title: '呼入',
                    message: 'success',
                    type: 'success'
                });

                _that.ua.answer();
            });

            _that.loginFormVisible=false
        },
        onHangUp () {
                this.$notify({
                    title: '挂断',
                    type: 'success'
                });

                this.ua.hangup()
        },
        makeCall () {
            this.$notify({
                title: '拨号前',
                type: 'success'
            });

            try {
                this.ua.call('1003@192.168.11.134');
            } catch (e) {
                console.log(e)
            }
        }
    }
}
</script>

<style scoped>
.softphone {
  display: flex;
}
.audio {
  display: none;
}
</style>
