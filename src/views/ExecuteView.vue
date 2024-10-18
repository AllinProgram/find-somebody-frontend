<template>
    <div class="flex flex-col justify-center items-center">

        <div class="divider mt-8 font-bold" style="color: hsla(160, 100%, 37%, 1);">输入链接</div>
        <form @submit.prevent="submitForm">
            <div class="form-control">
                <div class="join">
                    <input type="url" placeholder="小红书/网易云音乐/QQ音乐/汽水音乐/微博分享链接"
                        class="input input-bordered join-item input-success w-full" v-model="url" required />
                    <button class="btn join-item rounded-r-full" type="submit" :disabled="loading">
                        <span v-if="loading" class="loading loading-spinner loading-md"></span>
                        <span v-else>Go</span>
                    </button>
                </div>
            </div>
        </form>

        <div class="divider mt-8 font-bold" style="color: hsla(160, 100%, 37%, 1);">解析结果</div>
        <div class="alert" v-if="response.code == undefined || response.code == null">
            <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24"
                class="stroke-info shrink-0 w-6 h-6">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                    d="M13 16h-1v-4h-1m1-4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z"></path>
            </svg>
            <span>在上面输入分享链接后戳Go！</span>
        </div>

        <div class="alert alert-success" v-else-if="response.code === 1">
            <svg xmlns="http://www.w3.org/2000/svg" class="stroke-current shrink-0 h-6 w-6" fill="none"
                viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                    d="M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z" />
            </svg>
            <span>处理成功！</span>
            <a target="_blank" :href="'https://nclgclub.com/outgoing?url=' + response.data" class="btn">
                点我跳转
            </a>
        </div>
        <div v-else-if="response.code === 0">
            <div class="alert alert-error">
                <svg xmlns="http://www.w3.org/2000/svg" class="stroke-current shrink-0 h-6 w-6" fill="none"
                    viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                        d="M10 14l2-2m0 0l2-2m-2 2l-2-2m2 2l2 2m7-2a9 9 0 11-18 0 9 9 0 0118 0z" />
                </svg>
                <span>{{ response.message }}</span>
            </div>
        </div>

        <div class="divider mt-8 font-bold" style="color: hsla(160, 100%, 37%, 1);">一些帮助</div>
        <div>
            网站的设计应该是比较易用的，如果你在使用上还是遇到了一些比较棘手的问题，可以进行查阅
            <label for="my-modal" class="btn btn-xs btn-success mx-2">更加详细的文档</label>。<br><br>
            <input type="checkbox" id="my-modal" class="modal-toggle" />
            <div class="modal">
                <div class="modal-box">
                    <img src="../assets/qr_code.png">
                    <div class="modal-action">
                        <label for="my-modal" class="btn btn-sm">
                            好
                        </label>
                    </div>
                </div>
            </div>

            现已支持处理以下分享链接：
            <li class="italic">微博</li>
            <li class="italic">小红书</li>
            <li class="italic">汽水音乐</li>
            <li class="italic">网易云音乐</li>
            <li class="italic">QQ音乐（用户主页需要登陆自己账号后才能查看）</li>

            若出现任何问题，可以通过公众号进行反馈以获得帮助。<br>当然,也欢迎闲聊～
        </div>

        <div class="divider mt-8 font-bold" style="color: hsla(160, 100%, 37%, 1);">免责声明</div>
        <p>本站点仅供学习测试，完全免费，希望能帮你找到那个喜欢的TA！</p>
        <p> 一切分享性质以及使用过程中导致的责任仅与使用者相关，若不同意请勿使用。</p>

    </div>
</template>

<script>
alert("做这个工具的初衷不是让某些人用来卖钱甚至骗人的，我没有挣一分钱，网站甚至没有广告。为了防止滥用，现在只能通过微信公众号打开使用！");

export default {
    data() {
        return {
            url: '',
            loading: false,
            response: {},
        };
    },
    methods: {
        async submitForm() {
            if (!navigator.userAgent.includes('MicroMessenger')) {
                alert('滥用的人太多，请在微信公众号中打开使用！');
                return;
            }

            this.loading = true;
            this.response = {}; // 清空响应数据
            await new Promise(resolve => setTimeout(resolve, 1666));
            try {
                const response = await fetch('/api/parse', {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json',
                    },
                    body: JSON.stringify({ shareUrl: this.url }),
                });
                this.response = await response.json();
                this.url = ''; // 清空输入框
            } catch (error) {
                console.error(error);
                this.response = { code: 0, message: '请联系公众号进行处理！' };
            } finally {
                this.loading = false;
            }
        }
    },
    computed: {
        daysPassed() {
            const startDate = new Date('2022-01-01'); // 设置开始日期
            const currentDate = new Date(); // 获取当前日期

            // 计算日期差值（以毫秒为单位）
            const timeDiff = currentDate.getTime() - startDate.getTime();

            // 将毫秒转换为天数
            const daysPassed = Math.floor(timeDiff / (1000 * 60 * 60 * 24));

            return daysPassed;
        }
    }
};
</script>
