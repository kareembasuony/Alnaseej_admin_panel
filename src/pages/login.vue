<template>
    <div>
        <div class="h-100 bg-plum-plate bg-animation">
            <div class="d-flex h-100 justify-content-center align-items-center">
                <b-col md="8" class="mx-auto app-login-box">
                    <div class="app-logo-inverse mx-auto mb-3" />

                    <div class="modal-dialog w-100 mx-auto">
                        <div class="modal-content">
                            <div class="modal-body">
                                <div class="h5 modal-title text-center">
                                    <h4 class="mt-2">
                                        <div>اهلاً بعودتك،</div>
                                        <span
                                            >من فضلك قم بتسجيل الدخول في
                                            الاسفل</span
                                        >
                                    </h4>
                                </div>
                                <b-form-group label-for="email">
                                    <b-form-input
                                        v-model="email"
                                        id="email"
                                        type="email"
                                        required
                                        placeholder="الايميل"
                                    >
                                    </b-form-input>
                                </b-form-group>
                                <b-form-group label-for="password">
                                    <b-form-input
                                        v-model="password"
                                        id="password"
                                        type="password"
                                        required
                                        placeholder="كلمة المرور"
                                    >
                                    </b-form-input>
                                </b-form-group>

                                <div class="divider" />
                            </div>
                            <div class="modal-footer clearfix">
                                <div class="float-right">
                                    <b-button
                                        variant="primary"
                                        size="lg"
                                        @click="login"
                                        >تسجيل الدخول الى لوحة التحكم</b-button
                                    >
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="text-center text-white opacity-8 mt-3 ltr">
                        Copyright &copy; Alnaseej 2020 powered by&nbsp;

                        <a href="https://pithsoft.com/" target="_blank"
                            >Pithsoft</a
                        >
                    </div>
                </b-col>
            </div>
        </div>
    </div>
</template>
<script>
export default {
    data() {
        return {
            password: null,
            email: null
        };
    },
    methods: {
        async login() {
            await this.$http
                .post("/authenticate", {
                    email: this.email,
                    password: this.password
                })
                .then(res => {
                    localStorage.setItem("token", res.data.jwt);
                    this.$router.replace("/info");
                });
        }
    }
};
</script>
