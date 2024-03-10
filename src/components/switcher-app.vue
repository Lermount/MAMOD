<template>
    <div class="switcher-content">
        <div class="switch">
            <input class="switcher" type="checkbox" :checked="this.public" @click="this.switch"/>
        </div>
        <div class="switcher-description">
            <h3 class="switcher-description__title">Хотите чтобы Ваш профиль видели другие участники платформы? </h3>
            <p class="switcher-description__text">Включает профиль для просмотра другими пользователями по ссылке</p>
        </div>
    </div>
</template>
<script>
export default {
    data: () => ({
        public: false
    }),

    watch: {
        public() {
            this.$emit('updateSwitch', this.public);
        },
    },

    methods: {
        switch() {
            if(this.public == false) {
                this.public = true
            } else {
                this.public = false
            }
        }
    }
}
</script>
<style scoped>
.switcher-content {
    display: flex;
    align-items: flex-start;
}

.switch {
    margin-right: 5px;
}

.switcher {
    position: relative;
    width: 39px;
    height: 19px;
    margin: 0;
    border-radius: 100px;
    background: #A0AEC0;
    appearance: none;
    transition: .3s linear;
}

.switcher:checked {
    background: #3586FF;
}

.switcher::before {
    content: '';
    position: absolute;
    left: 0;
    width: 19px;
    height: 19px;
    border-radius: 50%;
    background-color: #fff;
    cursor: pointer;
    transition: 0.3s;
}

.switcher:checked:before {
    left: 20px;
}

.switcher-description {
    display: flex;
    flex-direction: column;
}

.switcher-description__title {
    margin-top: 0;
    margin-bottom: 10px;
    font-size: 16px;
    font-weight: 500;
    line-height: 19px;
    letter-spacing: 0em;
    text-align: left;
    color: #000000;
}

.switcher-description__text {
    margin-top: 0;
    margin-bottom: 0;
    font-size: 14px;
    font-weight: 400;
    line-height: 19px;
    letter-spacing: -0.0015em;
    text-align: left;
    color: #696977;
}
</style>