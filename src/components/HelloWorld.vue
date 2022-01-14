<template>
    <div class="hello">

        <div v-if="isLoggedIn">
            <div>
                You are logged in!
            </div>
            <div v-if="usertype">
                {{ usertype }}
            </div>
            <div>
                <button @click="logOut()">Logout</button>
            </div>
        </div>

        <div v-if="!isLoggedIn">
            <div>
                <input id="username" v-model="username" placeholder="Username">
            </div>
            <div>
                <input id="password" v-model="password" placeholder="Password">
            </div>
            <div>
                <button class="btn-sign-in" @click="signInClick">Sign In</button>
            </div>
            <div v-if="dirty">
                Details Incorrect Please Try Again
            </div>
        </div>


    </div>
</template>

<script>
export default {
    name: 'HelloWorld',
    props: {
        msg: String
    },
    data() {
        return {
            username: '',
            password: '',
            isLoggedIn: false,
            dirty: false,
            usertype: ''
        }
    },

    methods: {
        logOut() {
            this.isLoggedIn = false;
            this.username = '';
            this.password = '';
            this.dirty = false;
            this.usertype = '';
            const htmlElement = document.getElementsByTagName('html')[0];

            htmlElement.classList.remove('role-type-staff');
            htmlElement.classList.remove('role-type-parent');
            htmlElement.classList.remove('role-type-student');
            htmlElement.classList.remove('role-type-guest');
        },
        signInClick() {
            this.username = document.getElementById('username').value;
            this.password = document.getElementById('password').value;
            this.isLoggedIn = this.username === 'test-user' && this.password === 'test-pass';
            this.dirty = !this.isLoggedIn;
        },
    },
    mounted() {
        const mutationObserver = new MutationObserver((mutationsList) => {
            mutationsList.forEach(mutation => {
                if (mutation.attributeName === 'class') {
                    this.usertype = mutation.target.classList.value.replaceAll('-', ' ')
                }
            })
        })
        const htmlElement = document.getElementsByTagName('html')[0];
        mutationObserver.observe(htmlElement, {attributes: true})

    },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
    margin: 40px 0 0;
}

ul {
    list-style-type: none;
    padding: 0;
}

li {
    display: inline-block;
    margin: 0 10px;
}

a {
    color: #42b983;
}
</style>
