<template>
    <div class="flex flex-col justify-center items-center">

        <div>
            <h1 class="card-title">“{{ content }}”</h1>
            <div class="card-actions justify-end">
                <a v-bind:href="source" target="_blank">——《{{ name }}》{{ author }}</a>
            </div>
        </div>
        <div class="divider mt-8 font-bold" style="color: hsla(160, 100%, 37%, 1);">留言板</div>

        <div id="vcomments"></div>
    </div>
</template>

<script>
import Valine from 'valine';

export default {
    data() {
        return {
            name: '',
            content: '',
            author: '',
            source: 'https://hitokoto.cn/?uuid=',
        };
    },
    created() {
        this.fetchHitokoto();
    },
    methods: {
        fetchHitokoto() {
            fetch('https://v1.hitokoto.cn/?c=i')
                .then(response => response.json())
                .then(data => {
                    this.name = data.from;
                    this.content = data.hitokoto;
                    this.author = data.from_who;
                    this.source += data.uuid;
                })
                .catch(error => {
                    console.error('Error fetching quote:', error);
                });
        }
    },
    mounted() {
        new Valine({
            el: '#vcomments',
            // https://valine.js.org/
            appId: '',
            appKey: '',
            placeholder: "欢迎你来到这里，同时也欢迎你在这里畅所欲言，分享自己的故事~\n留言请尽量使用QQ邮箱（站内已经处理隐私，他人不可见），防止收不到回复。",
            meta: ['nick', 'mail', 'link'],
            recordIP: true,
            enableQQ: true,
            requiredFields: ['nick', 'mail', 'link']
        })
    },
};
</script>