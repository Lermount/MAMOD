<template>
    <div class="check-content">
        <div class="check">
            <input type="checkbox" class="form-check-input" :checked="checked" @click="this.check" />
            <p class="form-check-label">Регистрируясь, Вы соглашаетесь с <a href="https://example.com/"
                    class="form-check-label-link">политикой конфиденциальности</a> и обработкой <a
                    href="https://example.com/" class="form-check-label-link">персональных данных</a>
            </p>
        </div>

        <div>
            <button form="form" type="submit" class="btn" :disabled="!checked || !valid"
                @click.prevent="submitForm">Зарегестрироваться</button>
        </div>
    </div>
</template>
<script>
export default {
    props: ['username', 'email', 'role', 'password', 'password_repeat', 'public', 'error'],

    data: () => ({
        checked: true,
        valid: true,
    }),

    watch: {
        //Проверяем совпадение паролей из password и password_repeat, если не совпадают, btn disabled
        error() {
            if (this.error) {
                this.valid = false
            } else {
                this.valid = true
            }
        }
    },

    methods: {
        //Соглашение с политикой и т.д. по дефолту true, если отключить, то btn disabled
        check() {
            if (this.checked == false) {
                this.checked = true
            } else {
                this.checked = false
            }
        },
        //Ну, раз решил делать без сторонних библиотек, то и отправку формы без axios оформим
        submitForm() {
            //Полученные данные из form и switcher
            const userData = {
                public: this.public,
                username: this.username,
                role: this.role,
                email: this.email,
                password: this.password,
                password_repeat: this.password_repeat
            }
            //Нашел какой-то сервер, куда можно все это отправить
            const requestBinUrl = 'https://reqres.in/api/users';

            const requestOptions = {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify(userData)
            };
            //Проверка отправки, если все данные заполнены, то отправляем их на сервер, и открываем элемент с оповещением
            if (this.username && this.role && this.email && this.password && this.password_repeat) {
                fetch(requestBinUrl, requestOptions)
                    .then(response => response.json())
                    .then(this.$emit('updateThanks', true))
                    .catch(error => {
                        console.error('Произошла ошибка:', error);
                    });
            } else {
                //Если же поля не заполнены, то в зависимости от незаполненого поля выводим определенную ошибку
                //Эмитим ошибку в App.vue, от туда передаем в form-app и активируем. 
                if (!this.username) {
                    this.$emit('updateUsernameError', true);
                }
                if (!this.email) {
                    this.$emit('updateEmailError', true);
                }
                if (!this.role) {
                    this.$emit('updateRoleError', true);
                }
                if (!this.password) {
                    this.$emit('updatePasswordError', true);
                }
            }

        }
    },

}
</script>
<style scoped>
.check-content {
    margin-top: 30px;
    display: flex;
    justify-content: space-between;
    align-items: flex-start;
}

.check {
    display: flex;
    max-width: 540px;
}

.form-check-input {
    margin: 0;
    margin-right: 13.95px;
    width: 19px;
    height: 17px;
    border-radius: 5px;
    cursor: pointer;
}

.form-check-input:checked {
    background: #3586FF;
}

.form-check-label {
    margin: 0;
    font-size: 14px;
    font-weight: 400;
    line-height: 19px;
    letter-spacing: -0.0015em;
    text-align: left;
    color: #000000;
}

.form-check-label-link {
    color: #3586FF;
    text-decoration: none;
}

.btn {
    padding: 11px 81.31px 10px 90.69px;
    background: rgba(73, 122, 218, 0.2);
    border: none;
    border-radius: 8px;
    font-size: 12px;
    font-weight: 400;
    line-height: 19px;
    letter-spacing: -0.0015em;
    text-align: left;
    color: #497ADA;
    cursor: pointer;
    transition: .3s linear;
}

.btn:hover {
    background: #497ADA;
    color: #fff;
    transition: .3s linear;
}

.btn:disabled {
    pointer-events: none;
    opacity: 0.5;
}

@media(max-width:610px) {
    .check-content {
        flex-wrap: wrap;
    }

    .btn {
        margin-top: 30px;
        padding: 11px 20px 10px 20px;
    }
}
</style>