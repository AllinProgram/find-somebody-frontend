<template>

    <div class="card bg-neutral text-neutral-content shadow-xl">
        <div class="card-body items-center text-center card-compact">
            <h1 class="card-title">{{ code }}</h1>
            <p> 60分钟内有效，请勿滥用。</p>
        </div>
    </div>
</template>

<script>

console.log(navigator.userAgent);
export default {
    data() {
        return {
            code: '',
            verifyCode: {
                "00": "8912",
                "01": "3456",
                "02": "7890",
                "03": "1234",
                "04": "5678",
                "05": "9012",
                "06": "3456",
                "07": "7890",
                "08": "1234",
                "09": "5678",
                "10": "9012",
                "11": "3456",
                "12": "7890",
                "13": "1234",
                "14": "5678",
                "15": "9012",
                "16": "3456",
                "17": "7890",
                "18": "1234",
                "19": "5678",
                "20": "9012",
                "21": "3456",
                "22": "7890",
                "23": "1234"
            },
        }
    },
    mounted() {

        if (!navigator.userAgent.includes('MicroMessenger'))
            this.code = '请从公众号中打开此页面';
        else {
            this.code = this.verifyCode[new Date().getHours()];

        }
    }
}
</script>