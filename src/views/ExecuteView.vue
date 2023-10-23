<template>
    <div class="flex flex-col justify-center items-center">
        <form @submit.prevent="parseUrl">
            <div class="form-control">
                <div class="input-group">
                    <span>链接</span>
                    <input type="url" placeholder="填入网易云音乐分享链接" class="input input-bordered" v-model="url" required />
                    <button class="btn" type="submit">Go</button>
                </div>
            </div>
        </form>

        <div>
            <div class="divider mt-8">解析结果</div>

            <div class="radial-progress" v-if="isLoading" :style="{ '--value': loadingPercentage }">
                {{ loadingPercentage }}
            </div>
            <div v-if="!isLoading">
                <div v-if="code"><a :href="'https://nclgclub.com/outgoing?url=' + encodeURIComponent(data)"
                        target="_blank">解析成功，点我跳转。</a></div>
                <div v-else>{{ data }}</div>
            </div>
        </div>



        <div class="divider mt-8">一些帮助</div>

        <div style="max-width: 100%;">
            <p class="text-lg font-medium">音乐分享链接样例</p>
            <ul class="list-inside list-decimal">
                <li class="mb-4">
                    新版应用分享的链接格式
                    <div class="ml-4">
                        <p class="overflow-auto"><a
                                href="https://y.music.163.com/m/song?id=1406273234&amp;uct=G3Zo3HPs7EAHzH4lMl0+sQ%3D%3D&amp;dlt=2048"
                                target="_blank">https://y.music.163.com/m/song?id=1406273234&amp;uct=G3Zo3HPs7EAHzH4lMl0+sQ%3D%3D&amp;dlt=2048</a>
                        </p>
                        <p class="overflow-auto"><a
                                href="https://y.music.163.com/m/song?id=1406273234&amp;uct2=sS9/YtaXNsKoRP9/wNPXQw%3D%3D&amp;dlt=2048"
                                target="_blank">https://y.music.163.com/m/song?id=1406273234&amp;uct2=sS9/YtaXNsKoRP9/wNPXQw%3D%3D&amp;dlt=2048</a>
                        </p>
                    </div>
                </li>
                <li class="mb-4">
                    旧版应用分享的链接格式
                    <div class="ml-4">
                        <p class="overflow-auto"><a href="https://y.music.163.com/m/song?id=1406273234&amp;userid=301540571"
                                target="_blank">https://y.music.163.com/m/song?id=1406273234&amp;userid=301540571</a></p>
                    </div>
                </li>
            </ul>
            <p class="italic text-gray-500 mb-4">
                看到上述链接请不要困惑，这是链接的示例。<br>
                如果你找到的分享链接地址与其格式不符，并且解析异常，那说明找到的链接不正确。<br>
                若该信息真的对你很重要，你可以添加我的微信获取更多帮助。
            </p>
        </div>

        <div class="divider mt-8">免责声明</div>
        <p>本站点仅供学习测试，完全免费，一切分享性质以及使用过程中导致的责任仅与使用者相关，若不同意请勿使用。</p>

    </div>
</template>

<script>
export default {
    data() {
        return {
            url: null, // 填入的分享url
            code: false, // 接口处理结果
            data: '', // 接口返回数据
            isLoading: false, // 接口调用是否完成
            loadingPercentage: 0 // 进度条百分比
        }
    },
    methods: {
        async parseUrl() {
            this.isLoading = true;

            // 模拟请求过程，每隔一段时间更新进度条
            const updateLoadingPercentage = setInterval(() => {
                if (this.loadingPercentage < 100) {
                    this.loadingPercentage += 10;
                }
            }, 66);

            // 使用fetch发送请求
            const response = await fetch('api/parse', {
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify(this.url)
            })
            // 等待响应体解析完成
            const result = await response.json()
            console.log(result)
            // 处理请求成功后的数据
            this.code = result.code
            this.data = result.data

            // 清除定时器和设置 isLoading 为false
            clearInterval(updateLoadingPercentage);
            this.isLoading = false;
        }
    }
}
</script>