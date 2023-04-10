<template>
    <div>
    


        <div class="protected" v-if="loginSuccess">
            <h1>
                <v-badge variant="success">보안 사이트에 대한 액세스가 허용되었습니다</v-badge>
            </h1>
            <h5>로그인 성공!</h5>
        </div>
        <div class="unprotected" v-else-if="loginError">
            <h1>
                <v-badge variant="danger">이 페이지에 대한 접근 권한이 없습니다.</v-badge>
            </h1>
            <h5>로그인 실패!</h5>
        </div>


        <v-card-subtitle>
        <form @submit.prevent="login()">
            <v-text-field
                v-model="userId"
                label="이메일"
                class="form__control"
                hide-details="auto"
                
                outlined
                required
            
            >
            </v-text-field>
            
            <v-text-field
                v-model="userPw"
                type="password"
                label="비밀번호"
                class="form__control"
                hide-details="auto"
                
                outlined
                required
            
            >
            </v-text-field>

            

            <v-btn  
            class="login__btn loginbtn"
            type="submit"
            
            >로그인</v-btn>
            <p v-if="error" class="error">Bad login information</p>
        </form>
        </v-card-subtitle>
    
    
    </div>
</template>
<script>
import axios from 'axios'

export default {
    name: "LoginTest",

    head(){
        return{
            title:'LoginTest',
        }
    },

    data: () => ({
        loginSuccess: false,
        loginError: false,
        userId: '',
        userPw: '',
        error: false

    }),
    
    components: {
        
    },
    methods: {
        async login() {
            
            try {
                const result = await axios.get('http://localhost:8080/api/login', {
                    auth: {
                        username: this.userId,
                        password: this.userPw
                    }
                });
                console.log(result.status);
                if (result.status === 200) {
                    this.loginSuccess = true
                }
            } catch (err) {
                this.loginError = true;
                throw new Error(err)
            }
        }
    },
};
</script>
<style>
</style>